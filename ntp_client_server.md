# NTP Client & Server
## 安裝ntp
運行以下指令：
```apt-get install ntp```
## 修改server端(電腦)的 ntp.conf 
因為剛裝好的server端中沒有預設要從何處取得time

因此需要修改這個ntp的核心檔案

```sudo vim  /etc/ntp.conf ```

此次主要加入的指令只有

```server time.google.com```

server後方的可以是其他的時間IP

此處以google來作為example

而此行加在

```#Specify one or more NTP servers```

的下方，大約第四個#處

## 運行ntp

此處在DEMO時有出現一個錯誤(與參考的document出現分歧)

參考資料：

```etc/init.d/ntpd start```

然而在我運行後發現`no such file`

因此實際進入init.d的資料夾中實際查看，發現只有ntp檔

因此改運行

```etc/init.d/ntp start```

會出現以下

```[ok] Starting ntp(via systemctl):ntp.service```

代表執行成功

## 安裝ntpdate

因為ntpdate和ntp不是綁在一起的

因此需要重新安裝

```sudo apt install ntpdate```

## 運行ntpdate

第一次運行的時候出現了

```permission denied```

因此確認每次運行該指令都需要root

```sudo ntpdate 127.0.0.1```

運行完後會出現

```時間 the NTP socket is in use,exiting```

而此部分主要是裝在client端

## 讓開機自動啟動NTP

輸入：

```sudo systemctl enable ntp```

同樣此處與參考資料亦不相符(參考資料：ntpd)

