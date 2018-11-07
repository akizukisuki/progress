# NTP-CLIENT安裝使用說明文件
1. 安裝ntp-client的module
```npm install ntp-client```
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
