# Scapy
![Scapy Logo](/scapy/assets/scapy-logo.png)

From [Scapy.net](https://scapy.net):

> Scapy is a powerful interactive packet manipulation program. It is able to forge or decode packets of a wide number of protocols, send them on the wire, capture them, match requests and replies, and much more. It can easily handle most classical tasks like scanning, tracerouting, probing, unit tests, attacks or network discovery (it can replace hping, 85% of nmap, arpspoof, arp-sk, arping, tcpdump, tethereal, p0f, etc.). It also performs very well at a lot of other specific tasks that most other tools can’t handle, like sending invalid frames, injecting your own 802.11 frames, combining technics (VLAN hopping+ARP cache poisoning, VOIP decoding on WEP encrypted channel, …), etc.
> 
> Scapy runs natively on Linux, Windows, OSX and on most Unixes with libpcap (see scapy’s installation page). The same code base now runs natively on both Python 2 and Python 3.

#### Resources:
- [Scapy.net](https://scapy.net)
- [Scapy.readthedocs.io](https://scapy.readthedocs.io/en/latest/)
- [Awesome Scapy](https://scapy.net/awesome-scapy/): A curated list of tools, add-ons, articles or cool exploits using Scapy.

This is a place to store all the scapy one-liners I've needed to use. If you have any to add please create a comment or pull request.

## To list your interfaces accroding to how scapy sees them: `ifaces`
```
>>> ifaces
INFO: Table cropped to fit the terminal (conf.auto_crop_tables==True)
INDEX  IFACE                                             IPv4             IPv6                       MAC
20     Surface Ethernet Adapter                          10.123.123.101   fe80::450b:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
1      Software Loopback Interface 1                     127.0.0.1        ::1                        00:00:00:00:00:00
21     TAP Adapter OAS NDIS 6.0                          169.254.10.107   fe80::b007:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
35     Microsoft Wi-Fi Direct Virtual Adapter #6         169.254.129.11   fe80::7423:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
19     Microsoft Wi-Fi Direct Virtual Adapter #5         169.254.174.124  fe80::2d3a:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
9      TAP-Windows Adapter V9                            169.254.83.0     fe80::8dfc:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
29     Marvell AVASTAR Wireless-AC Network Controller _  192.168.1.133    fe80::ec5b:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
10     VMware Virtual Ethernet Adapter for VMnet8        192.168.40.1     fe80::4a1:xxxx:xxxx:xxxx   xx:xx:xx:xx:xx:xx
6      VMware Virtual Ethernet Adapter for VMnet1        192.168.65.1     fe80::addd:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
-1     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
-3     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
-2     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
```

## Windows specific command: `show_interfaces()`
```
>>> show_interfaces()
INFO: Table cropped to fit the terminal (conf.auto_crop_tables==True)
INDEX  IFACE                                             IPv4             IPv6                       MAC
20     Surface Ethernet Adapter                          10.123.123.101   fe80::450b:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
1      Software Loopback Interface 1                     127.0.0.1        ::1                        00:00:00:00:00:00
21     TAP Adapter OAS NDIS 6.0                          169.254.10.107   fe80::b007:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
35     Microsoft Wi-Fi Direct Virtual Adapter #6         169.254.129.11   fe80::7423:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
19     Microsoft Wi-Fi Direct Virtual Adapter #5         169.254.174.124  fe80::2d3a:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
9      TAP-Windows Adapter V9                            169.254.83.0     fe80::8dfc:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
29     Marvell AVASTAR Wireless-AC Network Controller _  192.168.1.133    fe80::ec5b:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
10     VMware Virtual Ethernet Adapter for VMnet8        192.168.40.1     fe80::4a1:xxxx:xxxx:xxxx   xx:xx:xx:xx:xx:xx
6      VMware Virtual Ethernet Adapter for VMnet1        192.168.65.1     fe80::addd:xxxx:xxxx:xxxx  xx:xx:xx:xx:xx:xx
-1     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
-3     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
-2     NdisWan Adapter                                   None             None                       00:00:00:00:00:00
```

## Configure a variable:
```
a = Ether(src="28:xx:xx:xx:xx:xxC",dst="cc:xx:xx:xx:xx:xx")/IP(src="10.123.123.101",dst="192.168.1.251")/TCP(sport=666,dport=777,dataofs=0,flags="S")
```

## Show the stacked layers and configured fields:
```
>>> a
<Ether  dst=cc:xx:xx:xx:xx:xx src=28:xx:xx:xx:xx:xx type=IPv4 |<IP  frag=0 proto=tcp src=10.123.123.101 dst=192.168.1.251 |<TCP  sport=doom dport=777 dataofs=0 flags=S |>>>
```

## Show the full configuration of the variable:
```
>>>ls(a)
dst        : DestMACField                        = 'cc:xx:xx:xx:xx:xx' (None)
src        : SourceMACField                      = '28:xx:xx:xx:xx:xx' (None)
type       : XShortEnumField                     = 2048            (36864)
--
version    : BitField  (4 bits)                  = 4               (4)
ihl        : BitField  (4 bits)                  = None            (None)
tos        : XByteField                          = 0               (0)
len        : ShortField                          = None            (None)
id         : ShortField                          = 1               (1)
flags      : FlagsField  (3 bits)                = <Flag 0 ()>     (<Flag 0 ()>)
frag       : BitField  (13 bits)                 = 0               (0)
ttl        : ByteField                           = 64              (64)
proto      : ByteEnumField                       = 6               (0)
chksum     : XShortField                         = None            (None)
src        : SourceIPField                       = '10.123.123.101' (None)
dst        : DestIPField                         = '192.168.1.251' (None)
options    : PacketListField                     = []              ([])
--
sport      : ShortEnumField                      = 666             (20)
dport      : ShortEnumField                      = 777             (80)
seq        : IntField                            = 0               (0)
ack        : IntField                            = 0               (0)
dataofs    : BitField  (4 bits)                  = 0               (None)
reserved   : BitField  (3 bits)                  = 0               (0)
flags      : FlagsField  (9 bits)                = <Flag 2 (S)>    (<Flag 2 (S)>)
window     : ShortField                          = 8192            (8192)
chksum     : XShortField                         = None            (None)
urgptr     : ShortField                          = 0               (0)
options    : TCPOptionsField                     = []              (b'')
```


## Send to remote LAN

#### Send SYN with INVALID TCP HDR length "8", Receive (if the socket isn't listening on the server) a RST
```
# Worked on Windows 10, Surface Pro 5
a = Ether(src="28:xx:xx:xx:xx:xx",dst="cc:xx:xx:xx:xx:xx")/IP(src="10.123.123.101",dst="192.168.1.251")/TCP(sport=666,dport=777,dataofs=0,flags="S")
sendp(a, iface='Surface Ethernet Adapter')
```

## Send & Receive: Local LAN

#### Send SYN, Receive (if the socket isn't listening on the server) a RST
```python
# TCP SYN Correct Header/Offset
>>> a = IP(dst="192.168.1.251")/TCP(sport=666,dport=777,flags="S")
>>> sr(a)
```

#### Send SYN with INVALID TCP HDR length "8", Receive (if the socket isn't listening on the server) a RST
```python
#Malformed TCP Header (Incorrect Offset/Header Length)
>>> a = IP(dst="192.168.1.251")/TCP(sport=666,dport=777,dataofs=8,flags="S")
>>> sr(a)
```

#### Send SYN with INVALID TCP HDR length "0", Receive (if the socket isn't listening on the server) a RST
```python
# Send a single SYN with a TCP Header/Data Offset of "0"
>>> a = IP(dst="192.168.1.251")/TCP(sport=666,dport=777,dataofs=0,flags="S")
>>> sr(a)
```

## Answering Machine With Incorrect TCP Header Length
Reference: [https://stackoverflow.com/questions/24415464/scapy-sending-receiving-and-responding](https://stackoverflow.com/questions/24415464/scapy-sending-receiving-and-responding)

This would be run on the 'server side' of the connection, to automatically respond to an incoming request from a client. This returns an invalid TCP HDR/Data Offset length by design.
```python
def rst(p):
    ans = IP(src=p[IP].dst, dst=p[IP].src)/TCP(
        flags='RA',
        sport=p[TCP].dport,
        dport=p[TCP].sport,
        seq = 0,
        ack = p[TCP].seq + 1,
        dataofs=8
    )
    send(ans, verbose=False)
    return "%s\n => %s" % (p[IP].summary(), ans.summary())
    
sniff(iface="eth0", filter="tcp and tcp[tcpflags] & tcp-syn == tcp-syn",prn=rst)
```

