### 主思路  
主要参考链接
 [解放你的双手 — 让普通家电也能被Siri召唤](http://post.smzdm.com/p/532100/?be_invited_by=3724913200)  
Siri <—> homeKit <—> homebridge <—> Domoticz <—> RMbridge <—>broadlinkRM <—> 受控设备  
 Siri接收语音信号控制Homekit，homekit通过homebridge控制Domoticz,Domoticz再通过RMbridge控制RMpro,RMpro接收网络信号发出红外或者射频信号，完成对设备的控制。broadlinkRM前面一直是网络信号的传递，broadlinkRM负责把接收到的遥控码用红外或者射频信号发到受控设备上。

### 树莓派安装配置
#### 树莓派系统安装

#### 设置国内源
>sudo nano /etc/apt/sources.list    

用 # 注释掉原有的 source, 输入阿里云镜像.    
>deb http://mirrors.aliyun.com/raspbian/raspbian/ jessie main non-free contrib    
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ jessie main non-free contrib

#### 安装nodejs
> sudo nano /etc/profile  

After you install it, edit your path by modifying /etc/profile  
> sudo nano /etc/profile  

Go ahead and add these two lines before “export $PATH”
>NODE_JS_HOME="/opt/node"  
NODE_PATH="/opt/node"  
PATH="$NODE_JS_HOME/bin/:$PATH"  

Log out and back in for it to take effect.
#### 安装 Homebridge 及相关依赖
>sudo apt-get install   libavahi-compat-libdnssd-dev  
apt-get install libavahi-compat-libdnssd-dev  
apt-get install build-essential g++  
npm install -g --unsafe-perm homebridge  
npm install -g --unsafe-perm homebridge-edomoticz

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


#### 设置开机启动
 方式1  安装cron（失败）
>sudo apt-get install gnome-schedule    
>crontab -e    
> @reboot homebridge &  

 This will run your Python script every time the Raspberry Pi reboots. If you want your command to be run in the background while the Raspberry Pi continues starting up, add a space and & at the end of the line
#### 開機時自動啟動VNCserver
> sudo nano /etc/init.d/tightvncserver

我們將會建立一個文字檔案，再來需打入下列程式碼

>#!/bin/sh
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

之後在LXTerminal視窗中繼續打入下列指令

>sudo chmod 777 /etc/init.d/tightvncserver

在繼續打入下面這一行

>sudo update-rc.d tightvncserver defaults

之後將Raspberry Pi重開機後就會自動執行VNC Server了

#### 安装RM Bridge替代方案
通过url发送控制请求
#### Installing and running Domoticz on a Raspberry PI,用Domoticz控制自动化,
If you are running Debian-based flavours of Linux on your Pi, like Raspbian and Ubuntu, installing Domoticz is easy. Just open a terminal window and execute this command.

> sudo curl -L install.domoticz.com | sudo bash  

Congratulations, you're done!  
Just point your browser to the IP address of your Raspberry Pi, and use port 8080. From your Pi's browser you could surf to http://127.0.0.1:8080.
