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
```

