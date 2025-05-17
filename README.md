# 随身WIFI Debian系统的无线网卡驱动

本项目包含三个无线网卡的驱动编译结果，仅适用于js大佬的5.15内核版本，具体版本号为：5.15.0-jsbsbxjxh66+

- AIC8800
- RTL8811CU/RTL8821CU
- RTL88X2BU

## 使用方法：

1. 把所需驱动对应文件夹直接放到"**/lib/modules/5.15.0-jsbsbxjxh66+/kernel/drivers/net/wireless**"目录下
2. 使用**insmod**命令对单个文件夹里的ko文件进行加载驱动，例如：**insmod /lib/modules/5.15.0-jsbsbxjxh66+/kernel/drivers/net/wireless/rtl8811cu/8821cu.ko**
3. 加载完成后使用**lsmod**命令查看是否成功被加载

## 注意事项：

一般加载完驱动后如果**ifconfig**里面没有网口出现，那就说明还要进行USB的模式切换（有的话就不用），具体步骤为：

1. 使用**lsusb**查看接入的usb设备（需要先设置为**host**模式），记录厂商id和设备id，例如：

   > root@4G-wifi:~# lsusb
   > Bus 001 Device 005: ID 0781:5595 SanDisk Corp. Ultra USB 3.0
   > Bus 001 Device 004: ID 0bda:c811 Realtek Semiconductor Corp. 802.11ac NIC

   这里记住0bda和c811，是我们无线网卡Realtek Semiconductor Corp. 802.11ac NIC的id

2. 使用**usb-modeswitch**命令进行usb设备的模式切换（没有这个命令的话需要先**apt-get install usb-modeswitch**一下），

   例如：**usb-modeswitch -KW -v 0bda -p c811**

   这里的-v后面的参数和-p后面的参数分别对应厂商id和设备id

   切换完成后一般ifconfig里面就会有网口了。

## 另外

后面自己设置下开机自动加载驱动和用udev设置一下连接usb自动转换模式就能直接稳定使用了，热插拔都可以。

**如果帮到了你，麻烦还请点个Star，谢谢！！！**
