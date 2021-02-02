# Juniper/JunOS
Quick reference of Juniper/JunOS configs for those of us who don't do it everyday.

If you have a favorite command/commands edit the document and create a pull request.


## First Boot

### Factory Reset or Change `root` Password:

**Juniper ex2300-c:**

1. Connect console cable
2. Power on switch
3. When asked `[press any key to stop boot]` , press space bar. (For me this happened in the first 3 seconds from power on, very quick)
4. When presented with the menu, press `Ctrl + C`, to stop auto boot. (You need to be very quick)
```
FreeBSD/x86 bootstrap loader, Revision 1.1
(builder@feyrith.juniper.net, Sun Feb  4 13:06:24 PST 2018)
/
Autoboot in 1 seconds... (press Ctrl-C to interrupt)
```
5. Press `5` for "More Options":
```
1.  Boot [J]unos volume
2.  Boot Junos volume in [S]afe mode

3.  [R]eboot

4.  [B]oot menu
5.  [M]ore options
```
6. Press `c` to boot into the "Recovery mode - CLI":
```
1.  Recover [J]unos volume
2.  Recovery mode - [C]LI

3.  Check [F]ile system

4.  Enable [V]erbose boot

5.  [B]oot prompt

6.  [M]ain menu
```


Once in the CLI you can:
- Factory Reset
- Set/change `root` password

**Factory Reset:**

This take about 10 minutes or more to complete.
```
set system zeroize
```

### Set Root Password:
<pre>
root# <b>set system root-authentication plain-text-password</b>

New password:
Retype new password:
root# commit
</pre>

### Turn off "Auto Image Upgrade":
```
delete chassis auto-image-upgrade
commit
```

### Turn off DHCP on irb.0 and vme.0:
```
delete interfaces irb unit 0 familty inet dhcp
delete interfaces vme unit 0 familty inet dhcp
commit
```

### Reboot:
```
root> request system reboot
```

### Power off the unit:
<pre>
root# <b>run request system power-off</b>
warning: This command will halt all the members.
If planning to halt only one member use the member option
Power Off the system ? [yes,no] (no)
</pre>

### Turn on SSH, allow root login
```
set system services ssh root-login allow
commit
```

### Set DNS Name Server
```
set system name-server x.x.x.x
commit
```

### Set Hostname & Domain Name
```
set system host-name jex4300
set system domain-name domain.tld
commit
```

### Set Date
Set the date to: November 27, 2020 10:04:00pm
```
date 202011272204.00
```

### Upgrade JunOS
Upload the JunOS image to `/var/tmp/`:
```
root> request system software add /var/tmp/junos-arm-32-20.2R2.11.tgz no-validate no-copy
```

### Configure a Virtual-Chassis
```

```

**NOTE:** After a Virtual-Chassis is configured use the `commit synchronize` command.

### Setting Analyzer Port
**SPAN Entire VLAN**

Output interface must have `family ethernet-switching` activated.
```
set interface ge-0/0/47 unit 0 family ethernet-switching

set forwarding-options analyzer SPAN-TAP input ingress vlan 1
set forwarding-options analyzer SPAN-TAP output interface ge-0/0/47.0

commit
```
<pre>
root@jex4300> <b>show forwarding-options analyzer</b>
  Analyzer name                    : SPAN-TAP
  Mirror rate                      : 1
  Maximum packet length            : 0
  State                            : up
  Ingress monitored VLANs          : default-switch/default
  Output interface                 : ge-0/0/47.0
</pre>


**SPAN Interface (ingress/egress)**

Output interface must have `family ethernet-switching` activated.
```
set interfaces ge-0/1/1 unit 0 family ethernet-switching 

set forwarding-options analyzer SPAN-TAP input ingress interface ge-0/0/4.0
set forwarding-options analyzer SPAN-TAP input egress interface ge-0/0/4.0

set forwarding-options analyzer SPAN-TAP output interface ge-0/1/1.0

commit
```
<pre>
root@jex2300-c-12p-a> <b>show forwarding-options analyzer</b>
  Analyzer name                    : SPAN-TAP
  Mirror rate                      : 1
  Maximum packet length            : 0
  State                            : up
  Ingress monitored interfaces     : ge-0/0/4.0
  Egress monitored interfaces      : ge-0/0/4.0
  Output interface                 : ge-0/1/1.0
</pre>
