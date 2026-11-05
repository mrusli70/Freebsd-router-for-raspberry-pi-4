FreeBSD router for raspberry pi 4
=================================
How to build a FreeBSD Router for the raspberry pi 4. Dual ethernet port.

Download the FreeBSD raspberry pi image from freebsd.org and flash with raspberry pi 4 imager from raspberry pi os.

Example:-

https://download.freebsd.org/releases/arm64/aarch64/ISO-IMAGES/15.1/FreeBSD-15.1-RELEASE-arm64-aarch64-RPI.img.xz

follow the instructions steps given by the build. 

You can use ee command to edit or install nano to use nano command to edit.

packages to download:-

pkg install nano

pkg install isc-dhcp44-server

pkg install unbound

tools for troubleshooting:-

pkg install ldns

pkg install nmap

pkg install bind-tools

to firstly setup make sure the wan is the main built-in ethernet port of the raspberry pi 4.

Use J5 Create USB 3.0 Ethernet for asix 88179 usb ethernet adapter. (It works out of the box)

https://en.j5create.com/collections/ethernet-adapters/products/jue130

Take note the Tp-link UE306 v2 does not work on freebsd. Due to latest revision and chipset 88179A or 88179B. (Only works on openbsd 8.0 for raspberry pi 4 -set tty fb0 and boot to install openbsd)(type sh /etc/netstart axen0 up to enable dhcp on openbsd)(openbsd 8.0 router for raspberry pi 4 is working so i decide to use freebsd router for raspberry pi 4 instead)(Avoid using realtek usb ethernet adapter it does not work although it detected usbdevs -v) (Even test it with Openwrt raspberry pi 4 the UE306 version 2 does not detect only J5 create USB 3.0 Ethernet adapter works!)

https://www.tp-link.com/sg/home-networking/adapter/ue306/

tzsetup command to setup your country. select the list of the country according to your locations.
type date command to update the date and time. 
yymmddhhmm (year,month,date,hour,minute)

type bsdconfig command to setup your network interface and select dhcp.

pkg bootstrap -f

pkg update -f && pkg upgrade -y

freebsd-update fetch install and when you see (END) press q or Q to update and install the patches. 

follow the instructions steps as shown by build to setup, install and configure the router. 

WAN is built in Raspberry Pi 4 Ethernet.
LAN is Asix 88179 USB Ethernet adapter.

Test Steve Gibson's GRC Sheildup stealth mode passed!

https://www.grc.com/x/ne.dll?bh0bkyd2

Other tests links

https://www.speedtest.net/

https://www.waveform.com/tools/bufferbloat

https://speed.cloudflare.com/

https://wander.science/projects/dns/dnssec-resolver-test/

https://www.dnsleaktest.com/

https://browserleaks.com/webrtc

http://nmap.online-domain-tools.com/

check security updates, first disable pf firewall to the security patches and updates and once completed turn back on pf firewall. 

check security updates via 

https://www.freebsd.org/security/advisories/
