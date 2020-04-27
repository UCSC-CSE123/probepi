# probepi
This repository contains Raspberry Pi installation and initialization instructions. For this
project, we are using [Raspian](https://projects.raspberrypi.org/en/projects/noobs-install)
as our operating system on [Raspberry Pi Model B+](https://www.raspberrypi.org/products/raspberry-pi-1-model-b-plus/)
boards.

## External Wifi Adapter
Although the Model B+ includes its own wireless network adapter, it does not support monitor mode
which is necessary in order to intercept wireless packets for the purpose of people counting. In 
addition, an adapter in this mode cannot simultaneously be connected to a network. Thus, a second 
adapter that supports monitoring mode is required for our design. A list of supported chipsets
can be found [here](https://wifivisit.blogspot.com/2019/07/Monitor-Mode-Supported-WiFi-Chipset-Adapter-List.html).

## Enabling Monitor Mode
There are multiple ways to enable monitoring on chipsets, however, many of them require killing
the NetworkManager making it more difficult to maintain a wifi connection while probing.

### Simple Configuration
We can manually modify two config files using the nano text editor in order to connect to our desired
wireless network using **wlan0** (RPI built in adapter) and also enable monitoring on **wlan1**
(external) every time Raspian boots up.
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
At the botton of the file we will add our desired network.
```
network={
 ssid="SSID"
 psk="PASSWORD"
}
```
Next we will modify interfaces to enable monitor mode on our external adapter.
```
sudo nano /etc/network/interfaces
```
```
allow-hotplug wlan1
 iface wlan1 inet manual
 pre-up iw phy phy1 interface add mon1 type monitor
 pre-up iw dev wlan1 del
 pre-up ifconfig mon1 up
```
