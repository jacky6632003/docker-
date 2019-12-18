docker基礎


**第一步: 環境 VMware 15 + centos 7.x + xshell 5 + winscp(傳文件用)**

**第二步: 安裝docker**


1. 安裝docker-ce, (ce是社區版免費的, ee是企業版收費的)


```

sudo yum install docker-ce

```



2. 查看當前docker版本



```

docker -v

```



3. 設置docker的鏡像為ustc, (ustc docker mirror是老牌子的linux鏡像提供者了, 不需要註冊, 公共服務)

（默認是沒有該文件的，請先建一個）

ustc官方介紹:&nbsp; http://mirrors.ustc.edu.cn/help/dockerhub.html?highlight=docker




```

mkdir -p /etc/docker
cd /etc/docker
touch daemon.json
vi /etc/docker/daemon.json

```


4. 啟動與停止docker



```

systemctl start docker   # 啟動
systemctl status docker  # 查看當前docker狀態  "Active: active (running) 代表運行中" "Active: inactive (dead)&nbsp;代表關閉"
systemctl stop docker    # 停止
_<em><em>systemctl restart docker # 重啟
_</em></em>_systemctl enable docker  # 設置成開機自動啟動
_docker info              # 查看詳細信息
docker --help            # 查看具體有哪些命令,以及怎麽用</pre>


```


**第三步: 鏡像命令**<

1. 查看鏡像


```
docker images
```



&nbsp;鏡像名稱是可以相同的, tag經常被當做版本號來使用

Active: active (running) 代表運行中

2. 網上搜索鏡像


```
docker search centos
```




3. 從中央倉庫拉取鏡像(如果卡主了,就ctrl+c停止, 再重新拉取)


```

docker pull tutum/centos  # 默認是直接拉取該倉庫的最新版本, 可以用 docker pull tutum/centos:14.04 來指定版本
docker pull centos  # 這個官方的當前是必備

```


4. 刪除鏡像


```

docker rmi 鏡像ID   #需要當前鏡像下的所有容器都停止或全被刪除,才能刪除鏡像

```



**第五步: 容器命令**

1. 查看容器(一個鏡像可以創建多個容器)


```

docker ps　　   #  查看正在運行的容器
docker ps -a   # 查看所有容器,包括開機的和關機的
docker ps -l   # 查看最後一次運行的容器

```


2. 創建容器


(1) 交互式方式創建容器


```

docker run -it --name=容器名稱 鏡像名稱:標簽 /bin/bash  #  容器名稱必須唯一,如果標簽是lastest就可以省略不寫,/ban/bash的作用是因為容器的運行至少需要一個進程, 這裏就指定用命令行作為啟動命令
docker run -it --name=mycentos centos /bin/bash  # 這裏我們用官方的centos來創建沒有centos容器,回車後會自動進行該容器,用ls命令查看便知, 再使用docker ps就能看到現在正在運行的該容器
exit  # 退出當前容器環境,docker ps裏面會顯示Exited

```


(2) 守護式方式創建容器


```

docker run -id --name=容器名稱 鏡像名稱:標簽  # 守護式不需要/bin/bash
docker run -id --name=mycentos2 centos     #  容器名稱必須唯一,創建後不會自動進入,但其實後台在運行,返回一長串字符串代表創建成功
docker exec -it mycentos2 /bin/bash   # 需要手動進入該容器
_<em><em><em>exit  # 退出當前容器環境, 但不會真正退出,後台還是在運行, _</em></em></em>docker ps裏面會顯示Up

```


(3) 停止與啟動


```


docker stop mycentos2  # 這才真正停止了這個容器, 也可以用 docker stop cd6c52a90b1b&nbsp;這種方式
docker start mycentos2  # 啟動容器(必須要啟動容器後,才能夠進入容器)


```


(4) 往容器中拷貝文件


```

docker cp /root/anaconda-ks.cfg mycentos2:/usr/local  # 往容器裏拷貝文件, 需要退出容器在宿主機上才能執行這個命令
docker cp mycentos2:/usr/local/anaconda-ks.cfg /root/anaconda-ks22.cfg  # 從容器裏面往外拷貝文件,需要退出容器在宿主機上才能執行這個命令

```


(5) 目錄掛載: 方便往容器中拷貝文件( -v 宿主機目錄:容器目錄, 如果目錄不存在會自動創建)


```

docker run -id --name=mycentos3 -v /usr/local/mycentos3Files:/usr/local/myfiles centos # 只要修改了宿主機/usr/local/mycentos3Files裏面的內容,那麽容器mycentos3的/usr/local/myfiles會相應的改變

```


如果共享的是多級的目錄, 可能會出現權限不足的提示

因為Centos7的安全模塊selinux把權限禁掉了, 我們需要添加參數 --privileged=true 來解決掛載目錄沒有權限的問題

(6) 查看容器ip


```

docker inspect mycentos3 # 查看容器mycentos3的所有信息
docker inspect --format='{{.NetworkSettings.IPAddress}}' mycentos3   # 只看ip的內容

```


(7) 刪除容器


```

docker rm mycentos # 需要改容器處於停止狀態

```


&nbsp;

**第六步: 常用應用部署**

**1. mysql部署**

(1) 拉取mysql鏡像


```
docker pull centos/mysql-57-centos7
```


(2) 創建容器( 端口映射: -p 宿主機端口:容器端口, MYSQL_ROOT_PASSWORD設置密碼)


```

docker run -id --name=test_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456 centos/mysql-57-centos7

```



**2. tomcat部署**

(1) 拉取鏡像


```
docker pull tomcat:7-jre7
```


(2) 創建容器


```

docker run -id --name=mytomcat -p 9000:8080 -v /usr/local/webapps:/usr/local/tomcat/webapps tomcat:7-jre7

```


(3) 下載別人已經部署好的cas.war, 下載地址:&nbsp; https://github.com/hjzgg/cas-overlay-template-.2.7/blob/master/target/cas.war

用xshell傳到/root目錄下後轉移他, 然後在瀏覽器中輸入&nbsp; ip:9000/cas去訪問


```
mv cas.war /usr/local/webapps
```




**3. Nginx部署**

(1) 拉取鏡像


```
docker pull nginx
```


(2) 創建容器



```
docker run -id --name=mynginx -p 80:80 nginx
```





(3) 進入容器並查看nginx的html放在哪兒的


```

docker exec -it mynginx /bin/bash  # 進入容器
cat /etc/nginx/nginx.conf          # 查看nginx配置, 可以發現 ''include /etc/nginx/conf.d/*.conf''
cat /etc/nginx/conf.d/default.conf # 進入這個配置文件,可發現  "root&nbsp;/usr/share/nginx/html;" 說明nginx的主頁就放在這兒的
cd /usr/share/nginx/html           # 進入這個文件夾就可以發現, 50x.html&nbsp; index.html 這兩個文件,說明找對了

```


(4) 接下來自己就可以把自己想用的靜態文件夾改名為html,去覆蓋nginx的html文件夾就ok了

&nbsp;

4**. Nginx部署**

(1) 拉取鏡像


```
docker pull redis
```


(2) 創建容器


```
docker run -id --name=myredis -p 6379:6379 redis
```


(3) 用windows的redis連接虛擬機上的redis容器, 以下示例說明連接成功




**第七步: 備份與遷移**

1. 容器備份


```
docker commit mynginx mynginx_i   # 把容器備份成鏡像
docker images　　　　　　　　　　　　 # 可以看到多了一個mynginx_i鏡像
docker run -id --name=mynginx2 -p 81:80 mynginx_i  # 再用這個鏡像來創建容器
接下裏在windows瀏覽器裏輸入"宿主機ip:81"     # 訪問成功, 說明這個容器備份成功
```


2 鏡像導入導出

```
docker save -o mynginx.tar mynginx_i   # 把鏡像導出成文件(不指定路徑的話,就在當前文件夾生成)
docker stop mynginx2　　　　　　　　　　　 # 把剛剛開啟的容器停止掉
docker rm mynginx2   　　　　　　　　　　 # 刪除這個容器
docker rmi mynginx_i   　　　　　　　　　# 刪除這個鏡像
docker images  　　　　　　　　　　　　　 # 可見,mynginx_i這個鏡像不在了
docker load -i mynginx.tar  　　　　　　# 導入鏡像
docker images  　　　　　　　　　　　　　　# 可見,mynginx_i這個鏡像恢覆了(說明是可以帶名字恢覆的)
```


**第八步: Dockerfile**

<span style="font-size: 14px; font-family: 宋體;">什麽是Dockerfile? 轉載&nbsp;[https://www.cnblogs.com/edisonchou/p/dockerfile_inside_introduction.html](https://www.cnblogs.com/edisonchou/p/dockerfile_inside_introduction.html)</span>


1. 基於 centos 7 創建 jdk8 鏡像

jdk8 官網下載地址:&nbsp;https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

```
mkdir -p /usr/local/dockerjdk8  # 創建一個文件夾, 並把下載的jdk8-linux移動到裏面
cd /usr/local/dockerjdk8   # 進入到該目錄
vi Dockerfile   # 創建Dockerfile文件,

　　FROM centos:7&nbsp; # 指定基礎鏡像,如果沒有centos:7, 那麽它就會自動下載centos7
　　MAINTAINER me&nbsp; # 聲明鏡像創建者
　　RUN mkdir /usr/local/java&nbsp; # 創建一個目錄
　　ADD jdk-8u121-linux-x64.tar.gz /usr/local/java&nbsp; # 自動把該文件解壓到指定目錄下

　　ENV JAVA_HOME=/usr/local/java/jdk1.8.0_121&nbsp; # 創建環境變量,讓我們在任何地方都能使用jdk命令,jdk1.8.0_121是解壓後的文件夾的名字,不要輸錯
　　ENV JRE_HOME $JAVA_HOME/jre
　　ENV CLASSPATH $JAVA_HOME/bin/dt/jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
　　ENV PATH $JAVA_HOME/bin:$PATH

　　--- wq 保存退出 ---

docker build -t='jdk1.8' .&nbsp; &nbsp;# 創建 jdk1.8 鏡像,指定Dockfile的文件路徑時當前文件夾, <span style="color: #ff0000;">**有個點 .&nbsp;**</span> 別忘記

docker images&nbsp; &nbsp;# 可以看到多了一個 jdk1.8 鏡像, 說明創建成功

```


**第九步: Docker私有倉庫**

<span style="font-family: 宋體;">掃盲轉載:&nbsp;[https://www.cnblogs.com/jaazz/p/9334183.html](https://www.cnblogs.com/jaazz/p/9334183.html)</span>

1. 下載registry


```
docker pull registry   # 下載
docker run -id --name=registry -p 5000:5000 registry　 # 創建容器,映射端口為5000
接下來去測試一下是否搭建成功
```




接下來去配置daimon.json文件, 讓docker信任私有地址, 我們才能上傳鏡像到私有倉庫

```
vi /etc/docker/daemon.json  
　　{
　　"registry-mirrors": ["https://9cpn8tt6.mirror.aliyuncs.com"],
　　"insecure-registries":["192.168.101.189:5000"]  # 把自己的宿主機的ip和5000端口加進去
　　}
　　--- wq 保存退出 ---

systemctl restart docker # 需要重啟docker, 重啟成功則說明配置成功,否則去檢查insecure是否輸錯
```

接下來去上傳鏡像

tag標簽詳細用法, 轉載:&nbsp;[https://www.cnblogs.com/pzk7788/p/10180919.html](https://www.cnblogs.com/pzk7788/p/10180919.html)


```
docker start registry  # 首先需要開啟私服
docker tag jdk1.8 192.168.101.189:5000/jdk1.8  # 標記此鏡像為私有倉庫的鏡像
docker images　　　　　# 可以看到多了一個 192.168.101.189:5000/jdk1.8 鏡像, 但id其實是一樣的
docker push 192.168.101.189:5000/jdk1.8  # 上傳到私有倉庫
```



&nbsp;最後再從私有倉庫下載鏡像,


```
docker pull 192.168.101.189:5000/jdk1.8  # 回車, 發現提示如下
```



&nbsp;意思是說, 已經存在該鏡像了, 所以我們可以先刪掉它,再下載試試,來驗證命令是否可用


```
docker rmi 192.168.101.189:5000/jdk1.8  # 先刪掉它
docker images  # 確認不存在這個鏡像
docker pull 192.168.101.189:5000/jdk1.8  # 可以發現,下載成功了,說明下載指令沒有問題
