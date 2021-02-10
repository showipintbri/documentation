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
 
 # For the latest feature release:
 sudo apt install zeek
 
 # For the latest Release Candidate:
 sudo apt install zeek-rc
 
 # For the recent LTS:
 sudo apt install zeek-lts
```

**NOTE:** Zeek installs to: `/opt/zeek/`. For other version change `zeek` --> `zeek-rc` or `zeek-lts`.

Add this to your common path:
```bash
# This setting is per session.
 export PATH=$PATH:/opt/zeek/bin
 echo $PATH
 
 # Make changes permanent:
 echo "export PATH=$PATH:/opt/zeek/bin" >> .bashrc
 ```

## Configuration Files
#### `/opt/zeek/etc/node.cfg`:
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

#### `/opt/zeek/etc/networks.cfg`:
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

**NOTE:** Zeek 4.0 comes with zkg already packaged. The below is for Zeek < 4.0
```bash
apt install python3-pip
pip3 install zkg

# After installation:
zkg autoconfig
```

#### Config File
The Zeek Package Manager config file is in: `$HOME/.zkg/config`

Be sure the `script_dir` & `plugin_dir` are correct for your installation.

**NOTE:** `state_dir` should reflect `$HOME/.zkg`
```bash
[sources]
zeek = https://github.com/zeek/packages

[paths]
state_dir = /root/.zkg
script_dir = /opt/zeek/share/zeek/site
plugin_dir = /opt/zeek/lib/zeek/plugins
zeek_dist =
```

#### Example: Install a package
```bash
# Search for package
zkg search geoip

# Install a package
zkg install geoip-conn

## EXAMPLE:
root@zeek:/pcaps/4/a# zkg install geoip-conn
The following packages will be INSTALLED:
  zeek/brimsec/geoip-conn (master)

Proceed? [Y/n] Y
Installing "zeek/brimsec/geoip-conn"......
Installed "zeek/brimsec/geoip-conn" (master)
Loaded "zeek/brimsec/geoip-conn"
root@zeek:/pcaps/4/a#
```

#### Tell Zeek to load installed 'packages'
Edit the `/opt/zeek-rc/share/zeek/site/local.zeek` file:
```bash
# For Zeek <  4.0 add the line
# For zeek >= 4.0 un-comment the line at the bottom
@load packages
```

#### Using the packages
- If you are running zeek collecting packet from a live NIC use the `zeekctl deploy` command.
- If you plan on using the packages with reading in PCAP's you need to invoke the package individually or tell Zeek to load all packages using the `local` script:
```
# Load all installed packages
zeek -C -r filename.pcap local

# Load individual packages
zeek -C -r filename.pcap geoip-conn
```
