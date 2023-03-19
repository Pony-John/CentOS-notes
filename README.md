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
Note: Python2不要随意删除，因为yum依赖于Python2解释。  
**Python2升级到Python3，需要重新配置yum**
[Linux笔记-Centos7将python2升级为python3（及修改yum配置防报错）-CSDN](https://blog.csdn.net/qq78442761/article/details/123470711)  
```
vi /usr/bin/yum
vi /usr/libexec/urlgrabber-ext-down
```
将以上两个文件中的`/usr/bin/python`修改为`/usr/bin/python2`，防止yum误调用Python3而报错。

## 2.4 安装clash for windows
[Linux科学上网 Ubuntu20.04LTS 配置科学上网环境-YouTube](https://www.youtube.com/watch?v=pTlso8m_iRk&t=182s)  
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
### 2.5 开启全局代理
全局代理指令（其中http_proxy、https_proxy为两个系统环境变量）
```
export http_proxy=http://127.0.0.1:7890   
export https_proxy=http://127.0.0.1:7890  # 注意后面是http不是https
```
也可以将如下指令文本
```
export http_proxy="http://localhost:port"
export https_proxy="http://localhost:port"
```
写入如下文件中
```
/etc/profile                  # 写在此文件中的代码会在开机时被执行
/etc/profile.d/ClashProxy.sh  # 存放在/etc/profile.d/路径下的.sh脚本会在开机时被执行
/root/.bashrc                 # 为shell启动文件(在MacOS中为.zshrc)
```
修改.bashrc后需要`source ~/.bashrc`来使之生效。

### 2.6 安装eg插件
eg插件是github上的开源项目，旨在提供linux指令的用法示例。
[eg-Useful examples at the command line.-GitHub](https://github.com/srsudar/eg)
使用pip3安装：
```
pip3 install eg
```
找到安装目录：
```
find / -name eg
返回结果：
/usr/local/python3/bin/eg
```
建立软链接：
```
ln -s /usr/local/python3/bin/eg /usr/bin/eg
```
使用方法：在想寻求示例的指令前键入eg即可
```
eg tar
```


# 3、处理问题记录
## 3.1 Python调用SSL失效
Caused by SSLError(“Can‘t connect to HTTPS URL because the SSL module is not available.“)   
[Caused by SSLError详解-CSDN](https://blog.csdn.net/bo_self_effacing/article/details/123628224)  
[Python解决SSL不可用问题-CSDN](https://blog.csdn.net/weixin_44894162/article/details/126342591)  
## 3.2 OpenSSL编译报错
Can‘t locate IPC/Cmd.pm in @INC  
[Can‘t locate IPC/Cmd.pm in @INC-CSDN](https://blog.csdn.net/sd4493091/article/details/122220902)
