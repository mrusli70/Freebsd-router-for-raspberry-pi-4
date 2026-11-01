FreeBSD router for raspberry pi 4
=================================

Download the FreeBSD raspberry pi image from freebsd.org and flash with raspberry pi 4 imager from raspberry pi os.

Example:-

https://download.freebsd.org/releases/arm64/aarch64/ISO-IMAGES/15.1/FreeBSD-15.1-RELEASE-arm64-aarch64-RPI.img.xz

follow the instructions steps given by the build. 

You can use ee command to edit or download nano to use nano command. 

packages to download:-

pkg install nano
pkg install isc-dhcp44-server
pkg install unbound

tools for troubleshooting:-

pkg install ldns
pkg install nmap
pkg install bind-tools

to firstly setup make sure the wan is the main ethernet port of the raspberry pi 4.

Use J5 Create USB 3.0 Ethernet for asix 88179 usb ethernet adapter. 
Take note the Tp-link UE306 v2 does not work. (Only works on openbsd)

tzsetup command to setup your country. select the list of the country according to your locations.
type date command to update the date and time. 
yymmddhhmm (year,month,date,hour,minute)

type bsdconfig command to setup your network interface and select dhcp.

pkg bootstrap -f
pkg update -f && pkg upgrade -y

freebsd-update fetch install and when you (END) press q or Q and update and install the patches. 

follow the instructions steps as shown by build to setup, install and configure the router. 

WAN is build in Raspberry Pi 4 Ethernet.
LAN is Asix 88179 USB Ethernet.
