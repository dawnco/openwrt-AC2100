# 红米路由器AC2100刷不死鸟breed、OpenWRT



### 1.1 登录web后台，获取token, 开启ssh和修改密码
登录成功的  stok=字符串  

开启ssh和修改密码

```
# 开启 ssh

http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=-h%3B%20nvram%20set%20ssh_en%3D1%3B%20nvram%20commit%3B%20sed%20-i%20's%2Fchannel%3D.*%2Fchannel%3D%5C%22debug%5C%22%2Fg'%20%2Fetc%2Finit.d%2Fdropbear%3B%20%2Fetc%2Finit.d%2Fdropbear%20start%3B


# 修改root密码为admin
http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/misystem/set_config_iotdev?bssid=gallifrey&user_id=doctor&ssid=-h%0Aecho%20-e%20%27admin%5Cnadmin%27%20%7C%20passwd%20root%0A

```
响应 {"code":0} 表示成功



### 1.2 刷入breed

ssh到登录路由器 可能支持 https
文件md5值 
```
e65d388129a6d1ac39abf99329f1978b  breed-mt7621-xiaomi-r3g.bin
```

```

cd /tmp
wget https://breed.hackpascal.net/breed-mt7621-xiaomi-r3g.bin
nvram set uart_en=1
nvram set bootdelay=5
nvram set flag_try_sys1_failed=1
nvram commit

mtd -r write breed-mt7621-xiaomi-r3g.bin Bootloader


```


如果路由器在60秒内重启则代表刷BREED成功(灯会从蓝变橘，最终变蓝进入系统)。

### 1.3.刷入成功后拔掉电源，按住reset同时接上电源等10秒即可进入breed。

### 1.4.设置PC为自动获取IP地址，访问http://192.168.1.1 进入Breed Web恢复控制台；

### 1.5. 添加环境变量xiaomi.r3g.bootfw",值设置为 2



### 1.5 下载固件

https://firmware-selector.openwrt.org/?version=24.10.0&target=ramips%2Fmt7621&id=xiaomi_redmi-router-ac2100

```


wget https://downloads.openwrt.org/releases/24.10.0/targets/ramips/mt7621/openwrt-24.10.0-ramips-mt7621-xiaomi_redmi-router-ac2100-initramfs-kernel.bin

wget https://downloads.openwrt.org/releases/24.10.0/targets/ramips/mt7621/openwrt-24.10.0-ramips-mt7621-xiaomi_redmi-router-ac2100-squashfs-kernel1.bin

wget https://downloads.openwrt.org/releases/24.10.0/targets/ramips/mt7621/openwrt-24.10.0-ramips-mt7621-xiaomi_redmi-router-ac2100-squashfs-rootfs0.bin

wget https://downloads.openwrt.org/releases/24.10.0/targets/ramips/mt7621/openwrt-24.10.0-ramips-mt7621-xiaomi_redmi-router-ac2100-squashfs-sysupgrade.bin

```

### 1.6 下载固件
在 breed 控制台刷入   格式是   kernel1

```
openwrt-24.10.0-ramips-mt7621-xiaomi_redmi-router-ac2100-squashfs-kernel1.bin
```