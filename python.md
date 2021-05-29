# Python

### Virtual Environment (`venv`)
```bash
# Install Python, pip & venv
user@host:$ sudo apt install python3 python3-pip python3-venv

# Create a directory and cd into it
user@host:~/myapp$ mkdir myapp && cd myapp

# Tell python to create a Virtual Environment here
user@host:~/myapp$ python3 -m venv .

# Use `source` and activate the venv from the current directory
user@host:~/myapp$ source ./bin/activate

# You're now in your Virtual Environment
(myapp) user@host:~/myapp$

# To leave your Virtual Environment
(myapp) user@host:~/myapp$ deactivate
user@host:~/myapp$
```

### IP Addresses

```
import ipaddress

for subnet in list(ipaddress.ip_network('192.168.0.0/22').subnets(new_prefix=24)):
    print(subnet)
    
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```
