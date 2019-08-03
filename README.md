# googlecloud-Ubuntu-SSR安装配置终极教程

google cloud-Ubuntu 18-SSR安装配置终极教程

# 命令步骤

## 1：切换到ROOT用户

```
sudo su
```

# 2：SSR一键安装脚本

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

# 3：安装BBR加速

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

# 4：检测BBR是否开启成功
## 执行命令
```
sysctl net.ipv4.tcp_available_congestion_control
```

出现 net.ipv4.tcp_available_congestion_control = bbr cubic reno
类似含有bbr字样即成功。
返回值一般为：
net.ipv4.tcp_available_congestion_control = bbr cubic reno
或者为：
net.ipv4.tcp_available_congestion_control = reno cubic bbr

# 5：如果没有开启BBR，则执行以下步骤，否则忽略

## 5.1 升级内核

### 5.1.1 更新包管理器

```
sudo apt update
```

### 5.1.2 查看可用的Linux内核版本

```
sudo apt-cache showpkg linux-image
```

### 5.1.3 找到一个你想要升级的Linux内核版本，如“linux-image-4.10.0-22-generic”

```
sudo apt install linux-image-4.10.0-22-generic
```

### 5.1.4 等待安装完成后重启服务器

```
sudo reboot
```

### 5.1.5 删除老的Linux内核

```
sudo purge-old-kernels
```

# SSR操作命令

```
以下是几个常用的SSR操作命令：
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
```

# SSR配置多端口  修改 /etc/shadowsocks.json文件

```json
{
	"server": "0.0.0.0",
	"server_ipv6": "[::]",
	"local_address": "127.0.0.1",
	"local_port": 1080,
	"port_password":
	{
		"8090":"password", 
		"8089":"password",
		"459":"password",
		"456":"password"
	},
	"timeout": 120,
	"udp_timeout": 60,
	"method": "aes-256-cfb",
	"protocol": "auth_aes128_md5",
	"protocol_param": "",
	"obfs": "http_simple",
	"obfs_param": "",
	"dns_ipv6": false,
	"connect_verbose_info": 1,
	"redirect": "",
	"fast_open": false,
	"workers": 1
}
```

