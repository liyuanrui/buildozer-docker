## 写在前面

感谢面包@nkiiiiid的指引，搭建了docker版本的buildozer打包环境。

python2和python3都可打包，但python3打包numpy和opencv等库会报错(纯py库可以)，待解决。

p4a打包方式其实也可以，但还没解决private.mp3 not found报错问题，待解决。

环境搭建教程来源：https://github.com/nkiiiiid/kivydev-note

文档格式参考来源：https://github.com/nkiiiiid/kivy-apk

## 0x00 系统配置

docker镜像下载地址 https://share.weiyun.com/5MOAnhX

docker安装方式省略

系统 Ubuntu 16.04 64位 amd64架构

用户名 kivydev 密码 kivydev

用户名root 密码 kivydev

vnc端口号5901 密码 kivydev 

启动vnc服务命令 /etc/init.d/vncserver start

镜像有点大，解压后有17G

已配置ssh、vnc, 需要ssh和vnc连接的话新建容器时需要映射22和5901端口.

## 0x01 导入并启动buildozer镜像

```
# 1. 安装p7zip(ubuntu为例)
sudo apt-get install p7zip p7zip-full

# 2. 解压镜像文件
7z e buildozer.docker.7z

# 3. 导入镜像到docker images
docker load --input buildozer.docker

# 4. 新建buildozer容器(映射22端口)
#    使用docker images查看镜像id
docker run -itd -p 30022:22 1196ea15dad6

# 5. 此时ssh -p30022 kivydev@127.0.0.1即可连上buildozer
#    也可以直接进入容器
#    使用docker ps查看容器id
docker exec -it 7c07ba426a27 /bin/bash # 
```
docker使用方法参考: https://shimo.im/docs/GWRh5bTwEAYyhRNE/

## 0x02 打包

系统/home/kivydev/路径下已经有两个测试打包的目录buildozer-py2与buildozer-py3，对应python2打包和python3打包。

python3打包要以buildozer-py3下的buildozer.spec为模板，复制到你的项目目录。

打包命令 在打包前必须把测试打包目录下的.buildozer复制到你的项目目录下。
```
buildozer init      #生成buildozer.spec配置文件，这个配置文件包括各种apk的配置
```

这里我对原始的bulldozer.spec配置文件做了修改,手动指定了sdk,ndk,ant的版本和路径。
```
buildozer android debug     #生成debug版本apk

buildozer android release   #生成release版本apk
```

如果有其他错误或者完善建议欢迎issue
