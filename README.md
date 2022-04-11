# Tools
一些好用的工具的记录
tools_file文件存放一些需要的安装文件
## 工具说明
### 编写自己的启动服务开机启动
1.新建服务

        sudo nano /etc/systemd/system/npsc.service  
2.编辑  
>[Unit]  
Description=nps Client  
After=network.target  
Wants=network.target  
[Service]  
Type=simple  
ExecStart=/home/pi/homeassistant/config/nps_client/ npc -server=IP:8012 -vkey=123456asdkj  
[Install]  
WantedBy=multi-user.target  

3.开机启动服务，启动服务  

    sudo systemctl  enable npsc.service  #开机自动运行
     
    sudo systemctl start npsc.service  
### 内网穿透  
项目地址：https://github.com/ehang-io/nps/releases  
1.服务端安装  

        tar -zxvf linux_amd64_server.tar.gz
        cd nps
        nohup ./nps start/ &  
之后访问 [服务器 IP]:8080，登录 web 页面。默认用户名 admin，密码 123  
2.客户端安装，树莓派看系统是多少位的，就用多少位的nps  

        tar -zxvf linux_amd64_client.tar.gz
        cd nps
        ./npc -server=xxx.xxx.xxx.xxx:8024 -vkey=hxxxxx4hzc -type=tcp  

登陆服务端的web管理就可以看到你的vkey是多少以及快捷连接命令了
### webssh网页ssh
项目地址：https://github.com/billchurch/webssh2  
因为可能不同的架构，所以自己构建容器镜像，进入tools_file/webssh2文件夹下执行命令  

1.      docker build -t webssh2 .
2.      docker run --name webssh2 -d -p 2222:2222 webssh2
容器开启后，你就可以访问页面了：http://192.168.0.101:2222/ssh/host/192.168.0.101

### 网易云音乐API
项目地址：https://github.com/Binaryify/NeteaseCloudMusicApi  
文档：https://binaryify.github.io/NeteaseCloudMusicApi/#/?id=neteasecloudmusicapi  
由于我是放在树莓派上架构不同，所以选择自定义镜像的形式  

                git clone https://github.com/Binaryify/NeteaseCloudMusicApi && cd NeteaseCloudMusicApi
                sudo docker build . -t netease-music-api
                sudo docker run -d -p 3000:3000 netease-music-api
### 检测蓝牙设备距离锁定解锁mac
项目地址：https://github.com/ts1/BLEUnlock  
### docker管理的UI界面  

    docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --name portainer  docker.io/portainer/portainer
现在你就可以访问 http://localhost:9000 了，第一次进入会自动创建密码
## 技巧知识 
### Pycharm绘图
[csdn参考链接](https://blog.csdn.net/weixin_51740371/article/details/109555345)        
绘图文件夹  
demo1.py是绘图demo代码，simhei.zip是中文包。
Mac端会出现中文乱码问题，解决如下  
>import matplotlib  
>print (matplotlib.matplotlib_fname()) # 将会获得matplotlib配置文件  
>\#我的是在"/Library/Python/3.8/site-packages/matplotlib/mpl-data/matplotlibrc  
> 解压中文包得到一个ttf文件,复制文件到"/Library/Python/3.8/site-packages/matplotlib/mpl-data/fonts/ttf
文件夹里面   
> cd ~   
> ls -a  
> cd .matplotlib  && rm fontlist-v330.json  
> 重启pycharm搞定   
> 
> test