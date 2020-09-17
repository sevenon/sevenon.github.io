
# testing toyrouter in vmware

## components

- **toyrouter**
- **vmware player 15.5.6**
- **ubuntu server 20.04.1**

## test bed

- **wan internet** - vmware vmnet1 provides dhcp and nat beween host and vm, no configuration required
- **router** - toyrouter im ubuntu vm, two network interfaces, one serial port
- **lan client** - ubuntu server in vm, used as a client to test the router

## router vm
- create new vm
- cd-rom image as ubuntu server iso
- first network card as NAT (wan side)
- add second network card, configure as vmnet2 (lan side)
- add serial port, named pipe, \\.\pipe\toyrouter-serial, this end server, other end app
- install ubuntu, enable ssh, disable luk volumes

## serial port test in router vm
- create session in PuTTY, serial, 115200, \\.\pipe\toyrouter-serial
- start serial login shell in ubuntu, sudo getty -L 115200 ttyS0  (or ttyS1)
- login with PuTTY
- ... diagnose any issues

## toyrouter install
- git clone "https://github.com/sevenon/toyrouter"
- cd toyrouter
- (check if ttyS? is correct in ubuntu-preinit.sh)
- sudo sh ubuntu-install.sh
- sudo update-grub
- reboot, choose toyrouter from boot menu
- login to router using PuTTY
- wan0 should acquire an IP address automatically
- ... diagnose any issues

## lan client vm
- create new vm
- cd-rom image as ubuntu server iso
- configure network card as vmnet2
- install ubuntu
- ubuntu should acquire an IP address from toyrouter
- ... diagnose any issues


