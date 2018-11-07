 關於ntp.conf檔中的資料
 ```shell
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
 ```
 
 ```shell
 \     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 0.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 1.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 2.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 3.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
+ntp.univ-angers 145.238.203.14   2 u  246  256  375  303.571   96.521  30.219
*ntp.maillink.ch .PPS.            1 u   54  256  377  310.632  105.637  60.069
+103-18-128-60.i 216.239.35.12    2 u  235  256  377   16.291   93.959  60.820
+nettuno.ntp.irh 193.204.114.232  2 u  249  256  375  303.751   96.011 109.958
+81.2.248.189 (h 195.113.144.238  2 u  204  256  377  310.422   98.813  56.245
+server.spnr.de  192.53.103.108   2 u   17  256  277  287.366  110.257  18.139
+ns3.stoneartpro 193.52.184.106   2 u  209  256  377  289.414  104.630  42.912

``` 
 
 ```shell
 #
# NTP configuration file (ntp.conf)
#
# Server From UTC(TL)。使用標準時間信號，和國家標準時間差不超過5毫秒，
# 另外附加美國NIST及USNO的NTP Server，若您同時連上這些server，ntpd 
# 會自動選擇最佳的server校時，並防止任一Server當機造成損失
#
server tick.stdtime.gov.tw prefer
server tock.stdtime.gov.tw prefer
server time.stdtime.gov.tw prefer
server clock.stdtime.gov.tw
server watch.stdtime.gov.tw


# Server From NIST and USNO。這是NIST（美國國家標準局）及USNO
# （美國海軍天文台）的Server。由於server設在美國，網路延遲並不固定
# 一般設定上述Server已足夠。
#
server 192.43.244.18 # time.nist.gov (ACTS)
server 132.163.135.130 # time-A.timefreq.bldrdoc.gov (ACTS)
server 192.5.41.40 # tick.usno.navy.mil
#
# Stratum-2 peers. Each server should chime all of the others in this
# 這部份可以設定您自己local的機器，在lan間可互相比較並作監測。
#
peer peer.address.1
peer peer.address.2
#
# Miscellaneous stuff 可用可不用
#
enable auth monitor # 允許monitor
driftfile \etc\ntp.drift # 記載您機器system clock的性能
statsdir \home\ntp\status\ # 記錄連線狀態
filegen peerstats file peerstats type day enable
filegen loopstats file loopstats type day enable
filegen clockstats file clockstats type day enable
#
# Authentication stuff. Note the different authentication delay on
# VAX and SPARC.
#
#keys /etc/ntp.keys # path for key file
#trustedkey 1 2 15 # define trusted keys
#requestkey 15 # key (7) for accessing server variables
#controlkey 15 # key (6) for accessing server variables
#authdelay 0.001501 # authentication delay (VAX)
#authdelay 0.000073 # authentication delay (SPARC)
 ```
 ```shell
      remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 0.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 1.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 2.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
 3.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
+ntp.univ-angers 145.238.203.14   2 u   64  256  373  303.571   96.521  30.228
*212.51.144.44   .PPS.            1 u  148  256  377  310.632  105.637  60.069
+103.18.128.60   216.239.35.12    2 u   80  256  377   11.724   98.090  63.526
+nettuno.ntp.irh 193.204.114.233  2 u   93  256  373  303.751   96.011 109.951
+81.2.248.189 (h 195.113.144.238  2 u   61  256  377  310.422   98.813  56.590
+server.spnr.de  192.53.103.108   2 u  131  256  277  287.366  110.257  18.139
+ns3.stoneartpro 193.52.184.106   2 u   64  256  377  289.414  104.630  43.014

 ```
 
 
 
 
 
 
