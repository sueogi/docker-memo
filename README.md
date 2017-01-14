docker compose 筆記
=======

因為 compose 比較方便，所以先學 compose ......

下面以建立 wordpress 來說明

## 安裝

### Windows

Windows 功能那邊要安裝 Hyper-V，所以 Windows 10 Home 不能用

從官網[下載](https://www.docker.com/products/docker#/windows)

[教學](https://docs.docker.com/docker-for-windows/#/shared-drives)

&nbsp;

如果要玩 Windows Container，可以從[這裡](https://docs.docker.com/docker-for-windows/#/download-docker-for-windows)下載 Beta 版本

微軟官方也有提供教學

[Windows 10 上的 Windows 容器](https://docs.microsoft.com/zh-tw/virtualization/windowscontainers/quick_start/quick_start_windows_10)

### Mac

待續...

### Shared Drive

如果要將本地端檔案 mount 到 container 裡，就要開防火牆及做一些設定

docker settings => Shared Drives

基本上 windows 好像都要開 c:

不過如果有裝防火牆，通常直接勾 c: 就會出現 firewall detected

這邊有[官方的解決方法](https://docs.docker.com/docker-for-windows/#/shared-drives)

不過好像沒不用麻煩

因為 docker 會開一個新網路連線 vEthernet (DockerNAT)

把這個網路連線改成區域網路或受信任的網路就可以了

## docker compose 設定檔
docker compose 設定檔預設命名是 docker-compose.yml (.yaml也可以)

所以建立一個 docker-compose.yml 先

### version

docker compose 的設定檔有分 v1、v2

指定 version: '2' 就會以 v2 的方式讀設定檔

沒指定預設是用 v1

### services

就服務，docker 基本上是建議一個服務建立一個container

所以會分 db、wordpress、phpmyadmin

當然如果要合在一起，例如說 db 跟 phpmyadmin 放在同一個 container

這個就要自己做 image 或看 docker hub 有沒有人做了

services 下一層是服務的名稱，可以任意取名

### image

就是看要抓哪一個 image 來用

:latest 代表最新版，沒寫就預設是 latest

### depends_on

設定這個 service 依賴的 service

不過這跟執行順序無關

就算 depends_on 資料庫，但資料庫未必比 ap 先啟動

不過官方有提供[方法](https://docs.docker.com/compose/startup-order/)

### ports

設定 host 的 port 對應到 container 的 port

## restart

設 always 就 docker 重開後總是自動重啟

### volumes

將本地目錄 mount 到 container 裡面

- ./wordpress:/var/www/html

就是把設定檔目錄底下的 wordpress 目錄 mount 到 container 的 /var/www/html

如果有要用自己的 wordpress 就要這樣做

也是有 run script 用 git clone latest 下來的做法，不過感覺分開來做會比較好

container 也不用安裝 git

### environment

image 的環境變數，這就要看每個 image 的設定了

## 運作

在 docker compose 設定檔目錄下執行

### 啟動

```bash
docker-compose up -d
```

-d 就 run in background

會自動 pull 本機沒有的 image


### 移除

```bash
docker-compose down --volume
```

--volume 代表連 volume 一起清掉

volume 是 mount 在 host 的資料


## docker 其他指令

### docker ps -a

### docker image -a

### docker volume ls

docker volume rm [VOLUME NAME]