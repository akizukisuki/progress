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
