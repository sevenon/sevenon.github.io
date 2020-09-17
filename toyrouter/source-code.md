
[ubuntu-install.sh](https://github.com/sevenon/toyrouter/blob/master/ubuntu-install.sh)

- installs in /usr/local/toyrouter
- creates grub boot menu entry

[**ubuntu-pre-init.sh**](https://github.com/sevenon/toyrouter/blob/master/ubuntu-pre-init.sh)

- configured in grub as the first process to execute (replaces /sbin/init)
- renames network interfaces to lan0 and wan0 
- overlays ubuntu's /bin /sbin and /etc, executes toyrouter's /bin/init
- edit to load usb network driver

[**/etc/inittab**](https://github.com/sevenon/toyrouter/blob/master/inittab)

- configuration file for /bin/init (busybox init)
- edit to enable ddns and time services

[**/etc/ddns-updater.sh**](https://github.com/sevenon/toyrouter/blob/master/ddns-updater.sh)

- configured for dns-omatic, most ddns services use a similar format
- edit to change ddns user name and password

[**/etc/iptables-rules.sh**](https://github.com/sevenon/toyrouter/blob/master/iptables-rules.sh)

- configures ip filtering and forwarding
- edit to enable access to telnet, ftp, and http from wan

[**/etc/udhcpd.conf**](https://github.com/sevenon/toyrouter/blob/master/lan-updown-event.sh)

- edit to configure lan ip address and dhcp settings

**/etc/TZ**

- timezone setting, see [/etc/TZ.readme](https://github.com/sevenon/toyrouter/blob/master/TZ.readme)

[/etc/inetd.conf](https://github.com/sevenon/toyrouter/blob/master/inetd.conf)

- configuration file for the intetd service, starts ftp, telnet, and http on demand

[/etc/www/cgi-bin/index.cgi](https://github.com/sevenon/toyrouter/blob/master/index.cgi)

- router web page accessed at the router's ip address

[/etc/wan-updown-event.sh](https://github.com/sevenon/toyrouter/blob/master/wan-updown-event.sh)

- requests new ip address from wan whenever the interace is brought up

[/etc/lan-updown-event.sh](https://github.com/sevenon/toyrouter/blob/master/lan-updown-event.sh)

- configures the router's ip address as specified in /etc/udhcpd.conf

/bin/busybox

- the busybox binary is included with the source, more recent versions may be available from [https://busybox.net/downloads/binaries/](https://busybox.net/downloads/binaries/)




