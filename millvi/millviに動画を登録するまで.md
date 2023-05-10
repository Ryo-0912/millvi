1.　millviにファイルをアップロードする

以下のcurlを実行し、どのようなデータでレスポンスが返ってくるか確認してみる。
```
curl "https://capi.miovp.com/file/upload " -F "ticket=(チケット文字列)" -F "filename=(ファイル名.mp4)" -F "file=@(ローカルのファイルフルパス.mp4)"```
```

ただ、公式には次のようなcurlが記述されているが、公式に問い合わせをしたところ、次のような回答が返ってきた。
```
curl "https://capi.miovp.com/report/file/upload " -F "ticket=(チケット文字列)" -F "filename=(ファイル名.mp4)" -F "file=@(ローカルのファイルフルパス.mp4)"
```
**reportが指定されている場合、millviの視聴履歴をGETする際に指定するものとなっております。
そのためreportを削除した形でリクエストを送っていただけますと幸いです。**

なので、新規で動画をmilviに登録したい場合は、urlから**report**を除く。


2.　millviに動画を登録
次に以下のcurlを実行する。
[参照URL](https://support-mv.millvi.jp/hc/ja/articles/5947278255513-contents-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E7%99%BB%E9%8C%B2-)
```
curl "https://capi.miovp.com/contents/create_video" -X POST -d "ticket=(チケット文字列)" -d "name=testmovie" -d "autocommit=1" -d "filekey=(ファイルキー)"
```
パラメータとして渡さないといけないものは[参照URL](https://support-mv.millvi.jp/hc/ja/articles/5947278255513-contents-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E7%99%BB%E9%8C%B2-)確認
