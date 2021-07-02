# NFDUMP
NFDUMP is a suite of tools for capturing, generating and parsing NetFlow data. [https://github.com/phaag/nfdump](https://github.com/phaag/nfdump)

## Problem:
I wanted to read a PCAP and generate NetFlow. The current `nfdump` package in Ubuntu (nfdump/focal,now 1.6.18-2) isn't compiled with the options to read a PCAP.

## Solution: Compile From Source
### Before you begin install a few dependancies:
```
sudo apt-get update && sudo apt-get install -y \
  wget \
  unzip \
  man \
  apt-utils \
  dialog \
  pkg-config \
  rrdtool \
  librrd-dev \
  libtool \
  autoconf \
  autogen \
  bison \
  byacc \
  flex \
  libbz2-dev \
  libpcap0.8-dev/focal
```
  
### Clone the Repository
```
git clone https://github.com/phaag/nfdump.git
```
  
### Configure / Make
From within in the new Git repository directory:
```
./autogen.sh

./configure --enable-readpcap --enable-nfpcapd
# --enable-readpcap: Enables nfcapd to read a *.pcap
# --enable-nfpcapd:  Created the nfpcapd binary in the bin directory

./make

./make install
```

