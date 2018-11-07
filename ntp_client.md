# NTP-CLIENT安裝使用說明文件
1. 安裝ntp-client的module

```npm install ntp-client```

會出現以下畫面(參考)

```shell
pi@raspberrypi:~ $ npm install ntp-client
npm WARN @google-cloud/logging-winston@0.9.0 requires a peer of winston@^2.x but none is installed. You must install peer dependencies yourself.
npm WARN pi@1.0.0 No description
npm WARN pi@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: xpc-connection@0.1.4 (node_modules/xpc-connection):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for xpc-connection@0.1.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"arm"})

+ ntp-client@0.5.3
added 1 package in 21.737s
```

2.選擇一資料夾創建nodejs檔"ntp_client.js"

```shell
var ntpClient = require('ntp-client');
 
ntpClient.getNetworkTime("pool.ntp.org", 123, function(err, date) {
    if(err) {
        console.error(err);
        return;
    }
 
    console.log("Current time : ");
    console.log(date); // Mon Jul 08 2013 21:31:31 GMT+0200 (Paris, Madrid (heure d’été))
});
```
3.運行`node ntp_client.js`會出現正確地現在時間
