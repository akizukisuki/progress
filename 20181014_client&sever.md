# 一.建立 Node.js NTP Client 端
# 二.Docker
為何使用doctor
1.	為因應不同的系統，且同時可以在不同系統運行及嘗試
2.	未給大量人使用，簡化安裝執行
關於doctor
1.	Docker 的底層是使用 Linux Kernel的 Namespace 和 Cgroup 組合而成的。因此在安裝 Docker 之前要先確認 Linux Kernel的版本是否有支援被 Docker 使用。
2.	Docker 的系統的架構主要是 Client-Server 的架構
3.	Docker image、Docker Container、Docker Hub 
(所謂的 image 就是一個已經打包好的環境，譬如說 Debian 或是 Ubuntu + Node.js 等等，需要這個環境時就可以直接把這個 image pull 下來用)

安裝doctor
1.	Docker 使用的作業系統是 CentOS 7.3， Kernel 使用預設的 3.10.0-514
2.	首先需要先安裝yum套件``apt-get install yum``
3.	在root下執行``yum install -y docker``來安裝doctor
4.	systemctl start docker來執行doctor
5.	docker version看版本
6.	如果要安裝最新的yum install https://yum.dockerproject.org/repo/main/centos/7/Packages/docker-engine-17.05.0.ce-1.el7.centos.x86_64.rpm
7.	systemctl enable docker可以讓下次開機自動執行
8.	使用 root 使用者權限時可以順利的執行 ``docker images``,如果有問題請使用以下方法處理
修改 ``/etc/docker/daemon.json`` 的檔案，如果沒有此檔案直接建立新的檔案，指令如下：
``Vi /etc/docker/daemon.json``
檔案內容如下：
``
{
"live-restore": true,
"group": "dockerroot"
}
``
JSON的逗號要記得加不然重新啟動 Docker Service 會啟動不起來
使用 root 權限把 user1 指令加入到 dockerroot 的 group，指令如下
`` usermod -aG dockerroot user1``
重新啟動 docker service，指令如下
``systemctl restart docker``
再一次使用一般使用者(user1)執行 docker images 指令，確認是否解決了問題
 
之後我所介紹使用的 docker 版本如果沒有特別說明，都是使用 1.12.6。今天已經把docker 安裝完成了，明天就可以開始把 docker image pull 下來使用

