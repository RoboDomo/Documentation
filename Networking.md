# RoboDomo: Networking and DNS
_Dynamic Name System_ (DNS) is the white pages of the Internet.  Every device has an IP address, but humans don't easily
remember IP addresses in general, nor what device is at a specific address.  For these reasons (and more), we set up a
server that you configure the binding between human friendly text names to IP addresses.  For our purposes, we need to
set up DNS for all the devices behind our router/firewall.  From outside, or the other side of the router, or from the
public internet, the names will not be visible (nor the devices).

The reason we set up a local DNS server is that it would be expensive and more work to set up a DNS server on the
Internet, purchase and manage a domain name, and configure and update the DNS entries as you ad devices to your home.
There really isn't a point to having your names available anywhere but on your LAN/WiFi.

## Routing
Routing is the means by which packets to or from a device are delivered to or sent from your devices.  The most
basic setup is to have a netmask of 255.255.255.0, which allows you to use a range of 254 IP addresses - 192.168.0.1
through 192.168.0.254, for example.  This is how your consumer grade router (given to you by your ISP) is likely to be
set up by default.

We suggest the use of a netmask of 255.255.0.0.  This allows a large address space of 192.168.0.1 through 192.168.0.254 
AND 192.168.1.1 through 192.168.1.254 AND 192.168.2.1 through 192.168.2.254, ... 192.168.254.1 through 192.168.254.254.

This scheme allows for an IP allocation scheme that is comfortable and like devices have similar addresses.  For
example, you can set up 192.168.0.* subnet for your routers and switches, the 192.168.1.* subnet for your computers, the
192.168.2.* for your tablets and phones, the 192.168.3.* subnet for your home theater devices (Rokus, AppleTVs, TiVo,
set top boxes, DVR, audio amplifiers, etc.).  You will be able to add up to 254 devices to each subnet, which is
probably plenty of headroom.

Every device that is connected to a network via WiFi or Ethernet has a unique 48-bit ID number assigned.  This number is
known as a MAC address.  You typically set up the IP address for a device as either static or DHCP.  If you set up a
static IP, you will need to specify the netmask (255.255.0.0, remember?) the DNS server, and so on.  If you use DHCP,
the device broadcasts a request to have an IP assigned.  The DHCP server keeps track of which  IPs are assigned which
devices, but does not know about static IPs you have assigned to devices.  The DHCP request for IP address includes the
device's MAC address.  The DHCP server can be configured to return the same (well known) IP for a device every time,
effectively giving your devices static IPs.  

When you have visitors to your home, your DHCP server should assign a truly dynamic IP.  When your guest leaves and
another guest visits, that same IP can be reassigned.

It's a bit of work to initially set up your DHCP server - MAC address <-> static IP.  Once that is done, you only need
to add entries for new devices you want to permanently add to your network (a new TV or stremaing player, for example).

When you add a device to your DHCP configuration, you will also want to add a name <-> IP address entry in your DNS
server.  Getting static IPs is less useful than having a static IP as well as a name you can use to refer to the device.

Most consumer grade routers, like the ones you get from your ISP when you sign up for service, have a static IP, like
192.168.1.1.  You can point your browser at http://192.168.1.1/ to access the router administration or setup pages.  You
set up the DNS server and DHCP server using the admin pages.  Instructions for each individual router brand will not be
presented here - google/search for your router to find instrucitons for setting up DHCP and DNS servers.

Why are static IPs useful?  

You cannot have a mnemonic name to IP address mapping with dynamic IPs.  Today your device gets assigned to it 
192.168.1.100, and tomorrow it gets 192.168.1.200.  Which IP should mydevice.home return?

When you are home and your phone is connected to WiFi, we can detect that and deduce that you are in fact home.  If your
phone is not connected to your home WiFi, we can deduce you are away.

For many devices, you can ping by name to see if the device is online.  

Many of the microservices need to control a device via IP address/network connection.  Without a static IP for that
device, how is the microservice supposed to know what IP to connect to?

## DNS
DNS is a bit more complicated to set up.  If you want to have local names for your devices and computers, you have a
couple of choices: edit /etc/hosts files on all your computers, or set up a DNS server for your LAN.

Editing /etc/hosts and keeping them all in sync on your SOHO/home computers is a bit of work and troublesome because you
have to keep all those files in sync.  If you add a device and IP address to your LAN, you have to edit all the
/etc/hosts on all your systems.  

Setting up a single DNS server to handle DHCP and DNS is ideal.  We recommend using dnsmasq running on the same machine
as you run the microservices/Docker containers.

Documentation for dnsmasq can be found here:
* http://thekelleys.org.uk/dnsmasq/doc.html

When you set up dnsmasq, you will then disable DHCP in your router and set the IP address of the DNS server in your
router to point to your dnsmasq server.  

You will set up a static IP for your dnsmasq server on the server.  It will not have DHCP available at boot time to
assign itself an IP!  However, once it is booted, every other device on your network will be able to get its appropritae
IP from your dnsmasq server.

## Advanced Setup
Ubiquitiy sells a small router for about $50 US:
* https://www.amazon.com/Ubiquiti-EdgeRouter-Advanced-Gigabit-Ethernet/dp/B00YFJT29C

This router is compact and can be configured to load balance between 2 internet connections or simply act like your
ISP's router.  You may or may not need to use your ISP router in bridge mode, though the EdgeRouter X (ERX) can connect
directly to your ISP's handoff if it is ethernet (in some cases).

The ERX runs Linux and has a limited amount of storage.  What you can do is set up dnsmasq on it and it will both assign
IP addresses via DHCP and act as your DNS server.

The ERX does not do WiFi.  For that, you will need to purchase one or more Access Points.
* https://www.amazon.com/Ubiquiti-UAP-AC-LITE-802-11ac-Gigabit-Dual-Radio/dp/B01DRM6MLI/

If you use more than one of these access points (in different rooms), the router automatically sets up a mesh WiFi
network.  As you walk around your home/office with your phone connected to WiFi, it will automatically switch its
connection to the access point with the strongest signal.  This is ideal for a home/office large enough that WiFi signal
is weak far from the main access point.

The ERX is not for novices at networking.  Setting up dnsmasq on a server is relatively easy in comparison.

## Continue Reading

* [Hardware](./Hardware.md)
* [DNS and Routing](./Networking.md)
* [Docker](./Docker.md)
* [MQTT](./MQTT.md)
* [SmartThings MQTT Bridge](./MQTTBridge.md)
* [Speaker / Text to Speech](./RoboSpeak.md)
* [Developing](./Developing.md)

