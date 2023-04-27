// **millviは基本的に既存でAPIを用意してくれている** ので、そのAPIを利用すれば、ログインやアップロードなどが行えるが、 **ログインするために必要な情報は.envに記載する** 必要がある。

API参考URL : https://support-mv.millvi.jp/hc/ja/articles/7261676225689-millvi-Web-API-%E3%82%AF%E3%82%A4%E3%83%83%E3%82%AF%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%88%E3%82%AC%E3%82%A4%E3%83%89

ログインAPI : https://support-mv.millvi.jp/hc/ja/articles/5950034559001-root-%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3-%E3%83%81%E3%82%B1%E3%83%83%E3%83%88%E9%96%A2%E9%80%A3-

## .env
```
MILLVI_HOST=XXXXXXXXXXXX
MILLVI_USERID=XXXXXXXXXXXXX
MILLVI_PASSWORD=XXXXXXXXXXXXXXXXX
```

## MillviServices.php(よく使うので、servicesクラス用いている)

````
<?php

namespace App\Http\Controllers\Schools\Api;
use GuzzleHttp\Client;
use illuminate\Support\Facades\Log;

class MillviServices
{
    public function loginMillvi() 
    {
        $client = new Client();

        $response = $client->post('https://capi.miovp.com/login', [
            'headers' => [
                'Content-Type' => 'application/x-www-form-urlencoded'
            ],
            'form_params' => [
                'host' => env('MILLVI_HOST'),              // ログインするときにenv関数を用いる。
                'userid' => env('MILLVI_USERID'),          // Millviの場合、ログイン時に必要な情報は左の3つ。これらは、[ログインAPI](https://support-mv.millvi.jp/hc/ja/articles/5950034559001-root-%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3-%E3%83%81%E3%82%B1%E3%83%83%E3%83%88%E9%96%A2%E9%80%A3-)に記載されている。
                'password' => env('MILLVI_PASSWORD')
            ]
        ]);

        Log::info('Here');
        Log::info($response);
        $res = json_decode($response->getBody()->getContents(), true);

        if ($res['status'] == false) {
            return "LoginError";
        } else {
            return $res;
        }
    }
}
```
