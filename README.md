

# RTL8188eu on RHEL / CentOS 8


This is a clone of the https://github.com/lwfinger/rtl8188eu

Specifically, it is a clone of the v4.1.8\_9499 branch

It only contains a simple hack of KERNEL\_VERSION to make it work on
RHEL/CentOS 8 kernels.

The upstream maintainer refuses to support kernels with backports, even
common ones like RHEL, CentOS and Ubuntu kernels.

It took me a couple hours to make it all work, so this is to help people
save time. In the future, I might add it as a kmod rpm to the libreswan
repository at https://download.libreswan.org/binaries/rhel/8/

To make this work on RHEL/CentOS 8:

```
make
sudo make install
sudo modprobe r8188eu
```

Running dmesg, will show you something like:

```
SupportPlatform 0x4
usbcore: registered new interface driver rtl8188eu
rtl8188eu 1-1.1:1.0 wlp0s26u1u1: renamed from wlan0
R8188EU: Loaded firmware file rtlwifi/rtl8188eufw.bin
R8188EU: Firmware Version 11, SubVersion 1, Signature 0x88e1
```

Configure the device with Network Manager:

```
sudo dnf install NetworkManager-wifi
sudo nmcli d
    [...]
    wlp0s26u1u1          wifi      disconnected  --          
    p2p-dev-wlp0s26u1u1  wifi-p2p  disconnected  --          

sudo nmcli d wifi list
    IN-USE  BSSID              SSID                              MODE   CHAN  RATE        SIGNAL  BARS  SECURITY  
        XX:XX:XX:XX:XX:XX  NoHatsEconomy                     Infra  7     11 Mbit/s   92      ▂▄▆█  WPA2      
        [...]        

sudo nmcli d wifi connect NoHatsEconomy password yourpassword
    Device 'wlp0s26u1u1' successfully activated with 'ce16b71c-c572-454d-aef7-1264b53c8b7a'.

ifconfig
    [...]
wlp0s26u1u1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1440
        inet 76.XX.XXX.XX  netmask 255.255.255.240  broadcast 76.XX.XXX.XX
        inet6 fe80::b852:c8b:5c31:2689  prefixlen 64  scopeid 0x20<link>
        ether xx:xx:xx:xx:xx:xx  txqueuelen 1000  (Ethernet)
        RX packets 17  bytes 2258 (2.2 KiB)
        RX errors 0  dropped 1  overruns 0  frame 0
        TX packets 34  bytes 5859 (5.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
