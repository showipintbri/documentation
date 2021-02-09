# Zeek

## Install
Choose which version you want from the links below. 

Instructions/downloads are available for: CentOS, Debian, Fedora, openSUSE, SLE & Ubuntu
- Latest Feature Releases: [https://software.opensuse.org//download.html?project=security%3Azeek&package=zeek](https://software.opensuse.org//download.html?project=security%3Azeek&package=zeek)
- Latest LTS Release: [https://software.opensuse.org//download.html?project=security%3Azeek&package=zeek-lts](https://software.opensuse.org//download.html?project=security%3Azeek&package=zeek-lts)

#### Example
```bash
 echo 'deb http://download.opensuse.org/repositories/security:/zeek/xUbuntu_20.04/ /' | sudo tee /etc/apt/sources.list.d/security:zeek.list
 curl -fsSL https://download.opensuse.org/repositories/security:zeek/xUbuntu_20.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/security_zeek.gpg > /dev/null
 sudo apt update
 sudo apt install zeek
```

**NOTE:** Zeek installes to: `/opt/zeek/`

Add this to your common path's:
```bash
 export PATH=$PATH:/opt/zeek/bin
 echo $PATH
 
 # Make changes permanent:
 echo "export PATH=$PATH:/usr/local/zeek/bin" >> .bashrc
```

## Configuration Files:
#### `/opt/zeek/etc/node.cfg`
```bash
# Example ZeekControl node configuration.
#
# This example has a standalone node ready to go except for possibly changing
# the sniffing interface.

# This is a complete standalone configuration.  Most likely you will
# only need to change the interface.
[zeek]
type=standalone
host=localhost
interface=eth0

## Below is an example clustered configuration. If you use this,
## remove the [zeek] node above.

#[logger-1]
#type=logger
#host=localhost
#
#[manager]
#type=manager
#host=localhost
#
#[proxy-1]
#type=proxy
#host=localhost
#
#[worker-1]
#type=worker
#host=localhost
#interface=eth0
#
#[worker-2]
#type=worker
#host=localhost
#interface=eth0
```

#### `/opt/zeek/etc/networks.cfg`
```bash
# List of local networks in CIDR notation, optionally followed by a
# descriptive tag.
# For example, "10.0.0.0/8" or "fe80::/64" are valid prefixes.

10.0.0.0/8          Private IP space
172.16.0.0/12       Private IP space
192.168.0.0/16      Private IP space
```

## Zeek Package Manager

#### Install `zkg`
```
apt install python3-pip
pip3 install zkg

# After installation:
zkg autoconfig





