Type

tzsetup
Select Country
date

bsdconfig
Select network interface
Select network card 
DHCP enable

Example date 2610261120

Pkg
Pkg update-f && pkg upgrade

Freebsd-update fetch install

When see (END) type Q

After build freebsd router and to update pkg, disable pf 
Check date see it is up to date
Yymmddhhmm 
2610311250
Check date
service pf stop
pkg bootstrap -f
Pkg update -f && pkg upgrade

nano /boot/loader.conf
if_axge_loader="YES"
# kernel.com.boot_delay="10000"
sctp_load="YES"


nano /etc/rc.conf
host name="generic"
ifconfig_genet0="DHCP"
ifconfig_ue0="inet 192.168.4.1 netmask 255.255.255.0"
defaultrouter="192.168.4.1"
getway_enable="YES"
dhcpd_enable="YES"
dhcpd_ifaces="ue0"
pf_enable="YES"
pf_rules="/etc/pf.conf"
pflog_enable="YES"
pf log_logfile="/var/log/pflog"
ntpd_enable="YES"
ntpd_sync_on_start="YES"
kern_secure_level_enable="YES"
local_unbound_enable="YES"
ifconfig_DEFAULT="DHCP inet6 accept_rtadv"
sshd_enable="NO"
growfs_enable="YES"
powerd_enable="YES"
# Set dumpdev to "AUTO" to enable crash dumps, "NO" to disable
dumpdev="AUTO"

# Check current status
sysctl net.inet.ip.forwarding

# Enable temporarily (until reboot)
sysctl net.inet.ip.forwarding=1

# Permanent way (preferred)
sysrc gateway_enable="YES"

nano /etc/pf.conf

ext_if="genet0" # Wan
int_if="ue0" # Lan

set block-policy drop
set skip on lo0

scrub in all fragment reassemble

nat on $ext_if from $int_if:network to any -> ($ext_if)

block in all

antispoof quick for $ext_if

table <bruteforce> persist 
block in quick from <bruteforce> to any

# block in on $ext_if proto {tcp udp} to any port {22,3283,3389,5900,5937,1900,5000}
# block out on $ext_if proto {tcp udp} from $int_if:network to any port {22,3283,3389,5900,5937,1900,5000}

pass out on $ext_if from $int_if:network to any keep state
pass in on $int_if from $int_if:network to any keep state

block in on $ext_if all

service pf start
pfctl-f /etc/pf.conf
pfctl -si
pfctl-sr
pfctl-sn 
pfctl-e

pkg install isc-dhcpd44-server

nano /usr/local/etc/dhcpd.conf

subnet 192.168.4.0 netmask 255.255.255.0 {
    range 192.168.4.10 192.168.4.20;
    option routers 192.168.4.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}

sysrc dhcpd_enable="YES"
sysrc dhcpd_ifaces="genet0"
service isc-dhcpd start

service net if start
service routing start

nano /etc/susctl.conf

security.bsd.see_other_uids=0
security.bsd.see_other_gids=0
security.bsd.unprivileged_proc_debug=0
security.bsd.unprivileged_read_msgbuf=0

kern.elf64.alsr.enable=1
kern.elf64.alsr.pie_enable=1
kern.elf32.alsr.enable=1
kern.elf32.aslr.pie.enable=1
kern.elf64.nxstack=1
kern.elf32.nxstack=1

net.inet.tcp.blackhole=2
net.inet.udp.blackhole=1
net.inet.sctp.blackhole=1

net.inet.tcp_syncookies=1

kldload sctp

sysctl net.inet.sctp.blackhole=1



# Edit the file
ee /etc/sysctl.conf

# Apply immediately
sysctl -f /etc/sysctl.conf

pkg install unbound
pkg search unbound

nano /usr/local/etc/unbound/unbound.conf

server:
interface: 127.0.0.1
interface: ::1
access-control: 127.0.0.0/8 allow
access-control: ::1 allow
access-control: 192.168.0.0/16 allow
access-control: 10.0.0.0/8 allow

hide-identity: yes
hide-version: yes
harden-glue: yes
harden-dnssec-stripped: yes
use-caps-for-id: yes
qname-minimisation: yes
aggressive-nsec: yes
prefetch: yes

tls-cert-bundle: "/etc/ssl/cert.pem"

forward-zone:
name: "."
forward-tls-upstream: yes
forward-addr: 8.8.8.8@853#dns.google
forward-addr: 8.8.4.4@853#dns.google

unbound-checkconf
service local_unbound restart

nano /etc/resolv,conf

Or

we /etc/resolv.conf

nameserver 8.8.8.8
nameserver 8.8.4.4
options edns0

chflags schg /etc/resolv.conf

To unlock or revert use this command

chflags noschg /etc/resolv.conf
