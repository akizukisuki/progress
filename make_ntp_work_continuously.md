# 一.嘗試用C輸入linux指令
再任意目錄下建立一test.cpp
```shell
#include<stdlib>
int main()
{
    system("sudo ntpdate time.google.com");
}
```
運行後確實和直接運行

`sudo ntpdate time.google.co`

結果相同

# 二.嘗試用node製作迴圈(定時)
## 1.`setInterval`會定時重複執行
建立一test.js檔如下
```shell
var sec= 1000;
    setInterval(function() {
        console.log(‘HI’);
                  }, oneSecond);
```
以上程式運行結果為每一秒輸出HI至頁面

然而若直接運行

`node test.js`

程式會無限執行，不會回到一般指令輸入介面

因此我們需要以下程式的協助
## 2.node.js永久背景執行
首先我們需要先安裝forever套件

```sudo npm install forever -g```
可能會出現warning但目前測試未出現影響

接著如同一般運行node程式時一樣，先到檔案存在的資料夾下再運行
```forever start test.js```
最後的js檔為你要運行的檔案

你可以透過以下指令查看正在被forever所運行的程式
```forever list```
會出現以下畫面：
```shell
info: Forever stopped process: 
    uid  command       script  forever pid  id logfile                    uptime
[0] 1ZVb /usr/bin/node test.js 2377    2383    /home/pi/.forever/1ZVb.log 0:0:2:46.939
```
我們可以看到第三行最前面的[0]代表著第0個

當我們要刪除(停止)該程式在forever中運行時要運行
```forever stop 0```
而後面的0即和前面所提到的[0]有關
運行後會呈現
```info:No forever processes running```

而在此運行的test.js檔會輸出到/home/pi/.forever/1ZVb.log

# 三.用node呼叫cmd
首先需要先安裝child_process
```sudo npm install child_process```
接著將test.js改為下列
```shell
const exec = require('child_process').exec
const build = exec('sudo ntpdate time.google.com') 
var sec=1000; 
setInterval(function() 
{ 
    build.stdout.on('data', data => console.log('stdout: ', data)) 
},sec);
```
# 四.開機自動開始執行
編輯rc.local會出現以下畫面

```shell
# /etc/rc.local
IP=$(hostname -I)||true
if["$_IP"]; then
    printf "My IP address is %s\n" "$_IP"
fi
echo "HI">/tmp/hello
node /home/pi/test.js &
exit 0
```
以上區域所輸入的程式碼皆會在開機時自動執行
因此這時請也在
```
exit 0
```
上方輸入：
```
forever start /home/pi/test/cmd.js
```
儲存退出後，reboot即可執行
