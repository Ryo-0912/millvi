1.　millviにファイルをアップロードする

以下のcurlを実行し、どのようなデータでレスポンスが返ってくるか確認してみる。
```
curl "https://capi.miovp.com/file/upload " -F "ticket=(チケット文字列)" -F "filename=(ファイル名.mp4)" -F "file=@(ローカルのファイルフルパス.mp4)"```
```

ただ、公式には次のようなcurlが記述されているが、公式に問い合わせをしたところ、次のような回答が返ってきた。
```
curl "https://capi.miovp.com/report/file/upload " -F "ticket=(チケット文字列)" -F "filename=(ファイル名.mp4)" -F "file=@(ローカルのファイルフルパス.mp4)"
```
**reportが指定されている場合、millviの視聴履歴をGETする際に指定するものとなっております。**

なので、新規で動画をmilviに登録したい場合は、urlから**report**を除く。

### millviにコード上でリクエストを送る(Guzzle利用)

```
<?php

namespace App\Http\Controllers\Schools\Api;
use GuzzleHttp\Client;
use Illuminate\Support\Facades\Storage;
use GuzzleHttp\Psr7;

略

public function uploadFile($ticket, $filename)
{
    $client = new Client();

    $response = $client->request('POST', 'https://capi.miovp.com/file/upload', [
        'multipart' => [
            [
                'name' => 'ticket',
                'contents' => $ticket,
            ],
            [
                'name' => 'filename',
                'contents' => $filename,
            ],
            [
                'name' => 'file',
                'contents' => Psr7\Utils::tryFopen(storage_path('app/public/uploaded/' . $filename), 'r'),
            ]
        ]
    ]);

    $csvData = $response->getBody()->getContents();
    $value = explode(',' ,$csvData);
    $key = array('fileName', 'fileKey', 'fileType');
    return array_combine($key, $value)['fileKey'];

}
```

millviでファイルをアップロードする際、**multipart/form-dataメディアタイプでPOSTメソッドでリクエストを送信する必要**があるので、Guzzleを利用する時も** 'multipart'**を使っている。
また、Guzzleでmultipartを使うと、自動的にheaderは決まるので、設定する必要がない。


2.　millviに動画を登録
次に以下のcurlを実行する。
```
curl "https://capi.miovp.com/contents/create_video" -X POST -d "ticket=(チケット文字列)" -d "name=testmovie" -d "autocommit=1" -d "filekey=(ファイルキー)"
```
パラメータとして渡さないといけないものは[参照URL](https://support-mv.millvi.jp/hc/ja/articles/5947278255513-contents-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E7%99%BB%E9%8C%B2-)確認


### millviにコード上でリクエストを送る(Guzzle利用)

```
<?php

namespace App\Http\Controllers\Schools\Api;
use GuzzleHttp\Client;

略

public function createVideo($ticket, $filekey)
{
    $client = new Client();

    $response = $client->post('https://capi.miovp.com/contents/create_video', [
        'headers' => [
            'Content-Type' => 'application/x-www-form-urlencoded'
        ],
        'form_params' => [
            'ticket' => $ticket,
            'autocommit' => 1,
            'parent_id_contents' => 212,
            'filekey' => $filekey
        ]
    ]);
    $res = json_decode($response->getBody()->getContents(), true);

    if ($res['status'] == false) {
        return "create video error";
    } else {
        return $res;
    }
}
```

