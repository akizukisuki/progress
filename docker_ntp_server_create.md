# 安裝NTP-SERVER in DOCKER
1.首先安裝資源包
 (以下安裝方法參考：https://github.com/cturra/docker-ntp)

```shell
 git clone https://github.com/akiicat/ecg-receiver
cd ecg-receiver
npm install
```
從docker pull相關images
```shell
root@raspberrypi:/home/pi/docker-ntp# docker pull cturra/ntp
Using default tag: latest
latest: Pulling from cturra/ntp
fb5dbe6f601f: Pull complete 
3e3f42a98e9e: Pull complete 
b9b11971468b: Pull complete 
b0a0ba8c5b3b: Pull complete 
98499cef5a73: Pull complete 
9e8a9c65b732: Pull complete 
Digest: sha256:f7fc025d729090fb9c8a6b2c9e6c04f3dda6543699c56dfa663827f0f2542409
Status: Downloaded newer image for cturra/ntp:latest
```

在docker執行剛剛pull的東西
```shell
root@raspberrypi:/home/pi/docker-ntp# docker run --name=ntp             \
>               --restart=always       \
>               --detach=true          \
>               --publish=123:123/udp  \
>               --cap-add=SYS_NICE     \
>               --cap-add=SYS_RESOURCE \
>               --cap-add=SYS_TIME     \
>               cturra/ntp
WARNING: Your kernel does not support memory swappiness capabilities, memory swappiness discarded.
7519c167f62081fd72e9f13d213ff7db1af26a835dfd7807780b9b82557a004b
Error response from daemon: Cannot start container 7519c167f62081fd72e9f13d213ff7db1af26a835dfd7807780b9b82557a004b: Error starting userland proxy: listen udp 0.0.0.0:123: bind: address already in use
```
用了以下辦法處理swap memory 的問題  參考： https://github.com/moby/moby/issues/4250
```shell
TL;DR: add cgroup_enable=memory swapaccount=1 to kernel boot options to enable both swap and memory accounting
/etc/default/grub:

GRUB_CMDLINE_LINUX_DEFAULT="cgroup_enable=memory swapaccount=1"
then sudo grub-update && sudo reboot
```
發現grub並不存在，因此在此安裝  參考： https://www.gnu.org/software/grub/grub-download.html
(grub主要是用來讓同一台電腦可以處理多種系統，算是一種啟動器)
```shell
pi@raspberrypi:~ $ git clone git://git.savannah.gnu.org/grub.git
Cloning into 'grub'...
remote: Counting objects: 92806, done.
remote: Compressing objects: 100% (21328/21328), done.
remote: Total 92806 (delta 69176), reused 92806 (delta 69176)
Receiving objects: 100% (92806/92806), 69.78 MiB | 349.00 KiB/s, done.
Resolving deltas: 100% (69176/69176), done.

pi@raspberrypi:~/grub $ npm install
npm WARN @google-cloud/logging-winston@0.9.0 requires a peer of winston@^2.x but none is installed. You must install peer dependencies yourself.
npm WARN pi@1.0.0 No description
npm WARN pi@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: xpc-connection@0.1.4 (node_modules/xpc-connection):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for xpc-connection@0.1.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"arm"})

audited 3389 packages in 20.446s
found 1 low severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
pi@raspberrypi:~/grub $ npm audit fix
npm WARN @google-cloud/logging-winston@0.9.0 requires a peer of winston@^2.x but none is installed. You must install peer dependencies yourself.
npm WARN pi@1.0.0 No description
npm WARN pi@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: xpc-connection@0.1.4 (node_modules/xpc-connection):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for xpc-connection@0.1.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"arm"})

up to date in 15.264s
fixed 0 of 1 vulnerability in 3389 scanned packages
  1 vulnerability required manual review and could not be updated

```
由於上述可發現會有warning的產生，因此參考http://cjworld1208.pixnet.net/blog/post/9567741-grub%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E8%AA%AA%E6%98%8E
的安裝模式，然而資源取的方法
```wget ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz```
```wget ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz.sig```
after entering the grub file
```shell
pi@raspberrypi:/usr/local/src/grub-2.02 $ ./configure
checking build system type... armv7l-unknown-linux-gnueabihf
checking host system type... armv7l-unknown-linux-gnueabihf
checking target system type... armv7l-unknown-linux-gnueabihf
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking for cmp... cmp
checking for bison... no
configure: error: bison is not found
```
error: bison is not found
解決辦法：
參考：https://geeksww.com/tutorials/miscellaneous/bison_gnu_parser_generator/installation/installing_bison_gnu_parser_generator_ubuntu_linux.php
```shell
pi@raspberrypi:~ $ wget http://ftp.gnu.org/gnu/bison/bison-2.3.tar.gz
--2018-11-04 17:47:58--  http://ftp.gnu.org/gnu/bison/bison-2.3.tar.gz
正在查找主機 ftp.gnu.org (ftp.gnu.org)... 208.118.235.20, 2001:4830:134:3::b
正在連接 ftp.gnu.org (ftp.gnu.org)|208.118.235.20|:80... 連上了。
已送出 HTTP 要求，正在等候回應... 200 OK
長度: 1386694 (1.3M) [application/x-gzip]
Saving to: ‘bison-2.3.tar.gz’

bison-2.3.tar.gz    100%[===================>]   1.32M   237KB/s    in 7.2s    

2018-11-04 17:48:09 (188 KB/s) - ‘bison-2.3.tar.gz’ saved [1386694/1386694]

pi@raspberrypi:~ $ tar -xvzf bison-2.3.tar.gz
bison-2.3/
bison-2.3/djgpp/
bison-2.3/djgpp/Makefile.maint
bison-2.3/djgpp/README.in

bison-2.3/examples/calc++/calc++-parser.yy
bison-2.3/examples/calc++/test
pi@raspberrypi:~ $ cd bison-2.3
pi@raspberrypi:~/bison-2.3 $ PATH=$PATH:/usr/local/m4/bin/
pi@raspberrypi:~/bison-2.3 $ ./configure --prefix=/usr/local/bison --with-libiconv-prefix=/usr/local/libiconv/
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking for style of include used by make... GNU
checking for gcc... gcc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
checking for suffix of executables... 
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ANSI C... none needed
checking dependency style of gcc... gcc3
checking how to run the C preprocessor... gcc -E
checking for egrep... grep -E
checking for AIX... no
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking minix/config.h usability... no
checking minix/config.h presence... no
checking for minix/config.h... no
checking whether it is safe to define __EXTENSIONS__... yes
checking for gcc... (cached) gcc
checking whether we are using the GNU C compiler... (cached) yes
checking whether gcc accepts -g... (cached) yes
checking for gcc option to accept ANSI C... (cached) none needed
checking dependency style of gcc... (cached) gcc3
checking for gcc... (cached) gcc
checking whether we are using the GNU C compiler... (cached) yes
checking whether gcc accepts -g... (cached) yes
checking for gcc option to accept ANSI C... (cached) none needed
checking dependency style of gcc... (cached) gcc3
checking for flex... no
checking for lex... no
checking for yywrap in -lfl... no
checking for yywrap in -ll... no
checking for bison... no
checking for byacc... no
checking for ranlib... ranlib
checking for gm4... no
checking for gnum4... no
checking for m4... no
checking whether m4 supports frozen files... no
configure: error: GNU M4 1.4 is required

```
因為error: GNU M4 1.4 is required
因此需要安裝GMU M4
參考：http://www.gnu.org/software/m4/m4.html
```shell
pi@raspberrypi:~ $ git clone git://git.sv.gnu.org/m4
Cloning into 'm4'...
remote: Counting objects: 18370, done.
remote: Compressing objects: 100% (3213/3213), done.
remote: Total 18370 (delta 14924), reused 18370 (delta 14924)
Receiving objects: 100% (18370/18370), 7.01 MiB | 603.00 KiB/s, done.
Resolving deltas: 100% (14924/14924), done.

git checkout -b branch-1.4 origin/branch-1.4
fatal: Not a git repository (or any of the parent directories): .git

```
