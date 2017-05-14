### 主思路  

家庭自动化简化模型  
Siri-homeKit-homebridge-Domoticz-RMbridge -broadlinkRM-受控设备  

模型描述  
 Siri接收语音信号控制Homekit，homekit通过homebridge控制Domoticz,Domoticz再通过RMbridge控制RMpro,RMpro接收网络信号发出红外或者射频信号，完成对设备的控制。broadlinkRM前面一直是网络信号的传递，broadlinkRM负责把接收到的遥控码用红外或者射频信号发到受控设备上。  

 主要参考链接  
  [解放你的双手 — 让普通家电也能被Siri召唤](http://post.smzdm.com/p/532100/?be_invited_by=3724913200)  

-----------------------
#### 文章目录
##### 1.树莓派安装和配置
##### 2.安装 Homebridge 及相关依赖
##### 3.Domoticz平台安装配置
##### 4.安装RM Bridge替代方案，控制博联遥控
##### 5.安装其他子配件，如小米系列
##### 6.其他参考链接
##### 8.linux常用指令
##### 8.shairport安装配置(已卸载)


--------------


##### 1 树莓派安装配置
1.1 下载树莓派系统及刷入SD卡
[官方参考链接](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)  
``` sudo dd bs=4m if=2017-04-10-raspbian-jessie.img of=/dev/rdisk2```

1.2 设置国内源
打开源列表进行编辑
>sudo nano /etc/apt/sources.list    

用 # 注释掉原有的 source, 输入阿里云镜像.    
>deb http://mirrors.aliyun.com/raspbian/raspbian/ jessie main non-free contrib    
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ jessie main non-free contrib

1.3 安装nodejs


>wget https://nodejs.org/dist/v4.3.2/node-v4.3.2-linux-armv6l.tar.gz
tar -xvf node-v4.3.2-linux-armv6l.tar.gz
cd node-v4.3.2-linux-armv6l
sudo cp -R * /usr/local/

After you install it, edit your path by modifying /etc/profile  
> sudo nano /etc/profile  

Go ahead and add these two lines before “export $PATH”
>NODE_JS_HOME="/opt/node"  
NODE_PATH="/opt/node"  
PATH="$NODE_JS_HOME/bin/:$PATH"  

1.4 改变环境变量
>sudo su  
PATH=/opt/node/bin/:$PATH  

1.5 開機時自動啟動VNCserver
> sudo nano /etc/init.d/tightvncserver

我們將會建立一個文字檔案，再來需打入下列程式碼
```
#!/bin/sh
#/etc/init.d/tightvncserver
#Carry out specific functions when asked to by the system  
case "$1" in  
  start)  
    su pi -c '/usr/bin/vncserver :1 -geometry 1240x1024'  
    echo "Starting VNC server "  
    ;;  
  stop)  
    pkill vncserver  
    echo "VNC Server has been stopped (didn't double check though)"  
    ;;  
  *)  
    echo "Usage: /etc/init.d/blah {start|stop}"
    exit 1  
    ;;  
esac  
exit 0  
```

之後在LXTerminal視窗中繼續打入下列指令

>sudo chmod 777 /etc/init.d/tightvncserver

在繼續打入下面這一行

>sudo update-rc.d tightvncserver defaults

之後將Raspberry Pi重開機後就會自動執行VNC Server了

Log out and back in for it to take effect.

----------------------
##### 2 安装 Homebridge 及相关依赖
2.1 下载并安装
>sudo apt-get install   libavahi-compat-libdnssd-dev  
apt-get install libavahi-compat-libdnssd-dev  
apt-get install build-essential g++  
npm install -g --unsafe-perm homebridge  
npm install -g --unsafe-perm homebridge-edomoticz

2.2 编辑配置文件

config.json 内容
```
{
    "bridge": {
        "name": "Homebridge",
        "username": "CC:22:3D:E3:CE:30",
        "port": 51826,
        "pin": "031-45-154"
    },
    "description": "configuration file with one fake accessory and one fake platform. You can use this as a template for creating your own configuration file containing devices you actually own.",

    "accessories": [],

    "platforms": [
        {
            "platform" : "eDomoticz",
            "name" : "eDomoticz",
            "server" : "192.168.1.101",
            "port" : "8080",
            "ssl" : 0,
            "roomid" : 0,
            "mqtt" : 1,
        }
    ]
}
```


2.3 设置开机启动  
 2.3.1  方式1  安装cron（失败）
>sudo apt-get install gnome-schedule    
>crontab -e    
> @reboot homebridge &  

 This will run your Python script every time the Raspberry Pi reboots. If you want your command to be run in the background while the Raspberry Pi continues starting up, add a space and & at the end of the line

-----------------------------

##### 3. 安装RM Bridge替代方案
通过url发送控制请求  
未完待续。。。

-------------------------
##### 4. Domoticz平台安装配置
Debian平台下运行以下这一行就好

> sudo curl -L install.domoticz.com | sudo bash  

运行在本地8080端口  
 http://127.0.0.1:8080.

 ----------------------
##### 5. 配件配置小米插座  
[小米插座](https://www.npmjs.com/package/homebridge-miio)

--------------------
##### 6. 其他参考链接
[如何安装在openwrt上](http://bbs.feng.com/forum.php?mod=viewthread&tid=10909315)  
[群晖安装broadlink-http-rest代替RMBridge](http://www.shafa.com/articles/zNzf7HJdfjyeAbGB.html)  
[小米网关接入Homekit完整教程，声控家中设备!](http://bbs.xiaomi.cn/t-13198850)  
[折腾 Raspberry Pi + HomeKit 手记](http://andelf.github.io/blog/2016/09/16/play-homekit-with-ios-10-and-raspberry-pi/)
[树莓派启动](http://elmagnificogi.github.io/2015/11/12/RaspberryStartup-6/)

---------------------
##### 7.linux常用指令
replace(/:/g, '')   
查看日志 tail -F /var/log/daemon.log  
sudo systemctl stop homebridge  
homebridge.service
查找 sudo find / -name 'homebridge.service'

---------------------
##### 8. shairport安装配置(已卸载)
```
删除make install
when you followed the instructions of this blog you can undo your steps like this:

remove shairport from boot scripts:

sudo update-rc.d -f shairport remove
sudo update-rc.d -f tightvncserver remove   /etc/init.d/tightvncserver
remove the init file

sudo rm /etc/init.d/shairport
find the shairport program

sudo find /usr -name 'shairport'

the result will be in /usr/local/bin or /usr/sbin
delete the program

sudo rm /path/to/file/shairport
search for soruces of shairport

sudo find / -name 'shairport'
delete sources

sudo rm -r /path/to/sources
maybe it is not recommented to purge the prereqirements you found in the blog post, becasue they might be used by other

i found the informations in this Blog post:

http://komputermaschine.blogspot.de/2015/01/shairport-vom-system-entfernen.html
```
