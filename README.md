# CentOS-notes

# 0、老是记不住的指令
打包/压缩/解压
```
tar [-zxcvf] 压缩后文件名 文件或目录
-c 建立一个压缩文件的参数指令（create），后缀是.tar
-x 解开一个压缩文件的参数指令（extract）
-z 以gzip命令压缩/解压缩
-v 压缩的过程中显示文件（verbose）
-f file 指定文件名,必选项
```
给予全部权限
```
chomd 777 -R dirname    # 给予读写执行权限
chown root -R dirname   # 归属于root用户
chgrp root -R dirname   # 归属于root用户组
```

# 1、系统概述
- 归属：腾讯云-轻量应用服务器
- CPU：2 Core
- 内存：2 Gb
- 带宽：4 Mbps
- 流量：300 Gb/月
- 操作系统：CentOS 7.6

# 2、常用搞机操作
## 2.1 安装宝塔面板
```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```
## 2.2 安装图形界面
[Centos安装图形化界面-CSDN](https://blog.csdn.net/qq_56418482/article/details/127161890)

安装X Windows控制功能
```
yum groupinstall "X Window System"
```
安装GNOME
```
yum groupinstall -y "GNOME Desktop"
```
启动/关闭 图形界面
```
startx  # 启动图形界面
init5   # 切换到命令行界面
init3   # 切换到图形界面
```
## 2.3 安装Python3.11.2
官网下载Python
```
wget https://www.python.org/ftp/python/3.10.5/Python-3.11.2.tgz
```
解压
```
tar -xzvf Python-3.11.2.tgz
```
进入目录
```
cd Python-3.11.2
```
编译并安装
```
./configure --prefix=/usr/local/python3   # --prefix是指定安装位置
make clean    # 清除以前的编译（若无可跳过此步）
make          # 编译
make install  # 编译安装
```
建立软链接
```
ln -s /usr/local/python3/bin/python3.11.2 /usr/bin/python3  # 添加软链接，使得python3指向python3.11.2
ln -s /usr/local/python3/bin/pip3.11 /usr/bin/pip3          # 添加软链接，使得pip3指向/usr/local/python3/bin/pip3.11

# 如果提示软链接已经存在，则可以：

cd /usr/bin/          # 进入指令目录
ls -la | grep python  # 查看关于python的软链接
ls -la | grep pip     # 查看关于pip的软链接
rm -rf python         # 删除关于python的软链接
rm -rf pip            # 删除关于pip的软链接
```
Note: Python2不要随意删除，因为yum依赖于python解释。
## 2.4 安装clash for windows
[Fndroid/clash_for_windows_pkg-GitHub](https://github.com/Fndroid/clash_for_windows_pkg)
我下载的版本是`Clash.for.Windows-0.20.18-x64-linux.tar.gz`  
解压
```
tar -zxvf Clash.for.Windows-0.20.18-x64-linux.tar.gz
```
进入目录
```
cd Clash.for.Windows-0.20.18-x64-linux
```
给予权限
```
chmod +x cfw
```
运行
```
./cfw --no-sandbox  # 不加后面这个参数无法执行
```


# 3、处理问题记录
## 3.1 Python调用SSL失效
Caused by SSLError(“Can‘t connect to HTTPS URL because the SSL module is not available.“)   
[Caused by SSLError详解-CSDN](https://blog.csdn.net/bo_self_effacing/article/details/123628224)  
[Python解决SSL不可用问题-CSDN](https://blog.csdn.net/weixin_44894162/article/details/126342591)  
## 3.2 OpenSSL编译报错
Can‘t locate IPC/Cmd.pm in @INC  
[Can‘t locate IPC/Cmd.pm in @INC-CSDN](https://blog.csdn.net/sd4493091/article/details/122220902)
