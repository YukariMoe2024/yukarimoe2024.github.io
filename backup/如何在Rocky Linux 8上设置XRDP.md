## 软件包预备  
以下将以Xorg XRDP为例  
> [!TIP]  
> 你将需要安装以下软件包：xorgxrdp xrdp,以及一个可以正常工作的桌面环境。  
> 本文将以KDE Plasma作文桌面环境,原因见上一个文章  

运行命令`sudo dnf install xorgxrdp xrdp`安装XRDP以及XRDP的Xorg附属（是不是这么叫？）  
并运行命令`sudo dnf groupinstall "KDE Plasma Workspaces"`安装KDE Plasma桌面环境  
## 设置XRDP  
安装完XRDP后还不够,你还需要设置一下XRDP,否则连接体验**非常卡顿**  
打开`/etc/xrdp/xrdp.ini`文件,找到`tcp_send_buffer_bytes`和`tcp_recv_buffer_bytes`两项,将其改为适合你服务器带宽的数字（本人设置是1048756）  
然后找到项`max_bpp`,将其改为16  
然后将`[Xorg]`一直到`code=20`的所有行取消注释（如果有）,保存并关闭文件。  
然后打开`/etc/sysctl.conf`文件,添加以下两项：  
```Conf
net.core.rmem_max=1048576  
net.core.wmem_max=1048576
```
保存关闭文件,运行命令`sudo sysctl -p`应用设置。  
如果你对安全性没啥要求,现在就可以去最后一步了。  
**如果你需要安全性,继续下一步。**  
## 保护XRDP会话
实际上,RDP是不太安全的协议,但是我们可以通过证书文件来提高一点安全性  
cd到`/etc/xrdp`并创建文件夹`certs`,运行命令`openssl req -x509 -newkey rsa:2048 -nodes -keyout key.pem -out cert.pem -days 3650`,按照说明设定证书。  
然后输入以下两条命令,让XRDP可以读取这两个证书文件：  
```Bash
sudo chmod 0644 /etc/xrdp/certs/cert.pem
sudo chmod 0600 /etc/xrdp/certs/key.pem
```
打开`xrdp.ini`,对照以下文本设定配置文件：  
```ini
security_layer=tls
certificate=/etc/xrdp/certs/cert.pem
key_file=/etc/xrdp/certs/key.pem
ssl_protocols=TLSv1.2, TLSv1.3
```
保存关闭文件,证书也就设置完毕了。  
## 启动并连接XRDP服务器  
### 开放防火墙端口  
> [!TIP]
> 如果你系统上没有防火墙或者防火墙已关闭,那可以跳过这一步,但我还是建议开启防火墙以保护你服务器的安全性。

我们假设你使用的是FirewallD,输入以下命令：
```Bash
sudo firewall-cmd --add-port=3389/tcp --permanent
sudo firewall-cmd --reload
```
如果两个命令都返回success,继续下一步,如果你在`xrdp.ini`设置的是别的端口,将3389改为你设置的端口。  
输入以下命令开启XRDP服务并使其在开机时启动：
```Bash
sudo systemctl enable xrdp --now
```
> [!NOTE]  
> 如果你使用的是手机,我极力推荐RD Client 8,使用最新的RD Client会导致你的RDP会话极度卡顿  

现在打开你的RDP客户端,连接你的服务器,登录,如果不出意外,你将会看到一个KDE Plasma桌面。  
**享受你的远程桌面体验吧！**  