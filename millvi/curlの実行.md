##curlの実行

どのような形でレスポンスが返ってくるかを確認するためにも、**ターミナル上でcurlをあらかじめ実行**しておいた方がいい。

例) 

[millviログイン API](https://support-mv.millvi.jp/hc/ja/articles/5950034559001-root-%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3-%E3%83%81%E3%82%B1%E3%83%83%E3%83%88%E9%96%A2%E9%80%A3-)

```
curl "https://capi.miovp.com/login" -X POST -d host=edu2020 -d userid=skrum -d password=dQwvbQ3HRG47tkg
{"ticket":"1719,1,3,normal,1,1,1683708085,98c80c0a6102687c5c24e3ff2696b12eef0ed930","type":"normal","vhost_name":"group1","permissions":{"contents":"1","report":"1"},"accountinfo":{"keitai":0,"admin_recipe":0,"use_chapter":0,"use_playbackrate":0,"use_broadcast":0,"use_broadcast_archive":0,"use_broadcast_transcode":0,"uploader_plan":"free"},"status":true}%       
 ```
