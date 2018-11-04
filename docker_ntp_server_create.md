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
用了以下辦法處理swap memory 的問題
```shell
TL;DR: add cgroup_enable=memory swapaccount=1 to kernel boot options to enable both swap and memory accounting
/etc/default/grub:

GRUB_CMDLINE_LINUX_DEFAULT="cgroup_enable=memory swapaccount=1"
then sudo grub-update && sudo reboot
```
