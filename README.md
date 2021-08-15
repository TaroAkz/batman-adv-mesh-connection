# batman-adv-mesh-connection
## Installing BATMAN-ADV

```
sudo apt-get install -y batctl
```
* Have batman-adv startup automatically on boot  
```
echo 'batman-adv'
sudo tee --append /etc/modules
echo 'denyinterfaces wlan0' | sudo tee --append /etc/dhcpcd.conf
echo "$(pwd)/start-batman-adv.sh" >> ~/.bashrc 
echo "Reboot to complete install
```
## Enabling the Necessary Interfaces

First create a script named  *start-batman-adv.sh*  in your home directory and make it executable. This will be used to enable the interfaces on boot as these settings do not stick after you reset your Pi. A way to do this is:
```
sudo nano start-batman-adv.sh 
sudo chmod +x start-batman-adv.sh
```
After you create this file, open it up and add the following to it:
```
#!/bin/bash
# batman-adv interface to use
sudo batctl if add wlan0
sudo ifconfig bat0 mtu 1468

# Tell batman-adv this is a gateway server
# sudo batctl gw_mode server

# Tell batman-adv this is a gateway client
sudo batctl gw_mode client

# Activates batman-adv interfaces
sudo ifconfig wlan0 up
sudo ifconfig bat0 up

sudo ifconfig bat0 192.168.199.2/24