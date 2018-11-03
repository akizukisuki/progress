 server tick.stdtime.gov.tw            //設定 Time Server
 server tock.stdtime.gov.tw
 server clock.stdtime.gov.tw
 server watch.stdtime.gov.tw
 server time.stdtime.gov.tw       
 server ntp.ntu.edu.tw
 server time.chttl.com.tw
 server 0.asia.pool.ntp.org
 server 1.asia.pool.ntp.org
 server 2.asia.pool.ntp.org
 broadcast 192.168.10.255 key 1        //設定區網內的 Broadcast 資訊
 driftfile /var/db/ntp.drift           //記載 NTP Server 的 System Clock 性能
 keys /etc/ntp.keys                    //指定存放 Broadcast Key Files 路徑
 trustedkey 1 2
 requestkey 2
 controlkey 2
 
 
 
