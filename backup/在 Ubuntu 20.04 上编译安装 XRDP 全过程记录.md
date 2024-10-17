# 环境准备  

安装软件包`build-essential libx11-dev libpam-dev libxrandr-dev libxfixes-dev autoconf automake libtool-bin pkg-config libfreetype2-dev libimlib2-dev `,以上是 XRDP 的编译安装依赖  
我们还需要以下软件包来编译安装 XorgXRDP：`xserver-xorg-dev`  

# 编译安装 XRDP

以下命令在当时环境中全部在 root 用户下运行。  
```Bash
wget https://github.com/neutrinolabs/xrdp/releases/download/v0.10.1/xrdp-0.10.1.tar.gz
tar xvf xrdp-0.10.1.tar.gz
cd xrdp-0.10.1
./bootstrap
./configure --enable-jpeg --enable-fuse --enable-pixman --with-freetype2=yes --with-imlib2=yes --with-x
make
make install
```

# 编译安装 XorgXRDP  

以下命令在当时环境中全部在 root 用户下运行。  
```Bash
wget https://github.com/neutrinolabs/xorgxrdp/releases/download/v0.10.2/xorgxrdp-0.10.2.tar.gz
tar xvf xorgxrdp-0.10.2.tar.gz
cd xorgxrdp-0.10.2
./bootstrap
./configure
make
make install
```

# 解决 无法启动 X 服务器 问题  

我在安装完毕 XRDP 后,尝试登录时,XRDP一直提示我“无法启动 X 服务器”  
使用`journalctl -xe`查看日志,发现以下报错：`xrdp-sesman[1156]: /usr/lib/xorg/Xorg.wrap: Only console users are allowed to run the X server`  
初步判定是`/etc/X11/Xwrapper.config`阻止了 XRDP 启动 X 服务器  
发现最后一行写着`allowed_users=console`,将其修改为`allowed_users=anybody`,重启 XRDP 服务,恢复正常运行。  