# PCAP
A place for me to store CLI shortcuts for TShark, TCPDump and Wireshark.
Tips collected from around the internet, classes, webinars and personal troubleshooting. 


## Dumpcap

#### Save PCAP via CLI using a rolling buffer.

```
dumpcap -i 1 -w pcaptest.pcapng -b filesize:100000 -b files:10

-i    interface
-w    write filename
-b    (filesize) Kilobytes
-b    (files) Number of files to save
```

## Wireshark

#### Save PCAP via CLI using a rolling buffer.

```
"C:\Program Files\Wireshark\Wireshark.exe" -i 1 -w pcaptest.pcapng -b filesize:100000 -b files:10
```

## Wireshark Display Filters

#### Offset Filtering
```
# Look for '77:77:77' starting at hex offset 0x37 in the following 3 bytes

eth[0x37:3] == 77:77:77
```

#### Using the `in` operator:
```
# Show packets where the ICMP Type is "0" or "8" (Echo Reply or Echo Request)

icmp.type in {0 8}
```

## tcpdump

Capture for a certain amount of time, keeping a certain amount of files using a rolling buffer regardless of file size.

```
sudo tcpdump -i ens160 -w four_hours.pcap -W 1 -G 14400

-w four_hours.pcap: output file name
-W 1: File count
-G 14400: How many seconds to keep capturing
```


## tshark

#### What are all the DNS servers we are sending requests to, and how many requests are going to each DNS server?
```
tshark -r onehour.pcap -Y 'dns.flags.response == 0' -T fields -e ip.dst | sort | uniq -c | sort -rn

 -r onehour.pcap: read in the file "onehour.pcap"
 -Y 'dns.flags.response == 0': Use the packet filter to single out only packets with the DNS Response flag det to "0"
 -T fields -e ip.dst: Show only the IP Destination field
 | sort: sort the output numerically
 | uniq -c: count how many occurances and reduce to a single line for repeated items
 | sort -rn: sort the results in reverse-numerical order
```


#### What is the count of DNS requests by Domain they are requesting:
```
tshark -r onehour.pcap -Y 'dns.flags.response == 0' -T fields -e dns.qry.name | sort | uniq -c | sort -rn

 -r onehour.pcap: read in the file "onehour.pcap"
 -Y 'dns.flags.response == 0': Use the packet filter to single out only packets with the DNS Response flag det to "0"
 -T fields -e dns.qry.name: Show only the DNS Query Name
 | sort: sort the output numerically
 | uniq -c: count how many occurances and reduce to a single line for repeated items
 | sort -rn: sort the results in reverse-numerical order
```


#### Show the TLD and sort & count by occurance (CAVEAT: only looks at the last 1 to 5 characters)
```
tshark -r onehour.pcap -Y 'dns.flags.response == 0' -T fields -e dns.qry.name | egrep -o '\..{1,5}$' | sort | uniq -c | sort -rn
   1064 .net
    750 .com
     80 .org
     56 .info
     28 .co
     13 .local
     12 .tv
     12 .io
      5 .lab
      2 .nt.vc
      2 .fi
      2 .arpa
      1 .w.org
      1 .tcs
      1 .fr
      1 .ai
```


#### Match only domains not ending in \*.com, \*.net or \*.org, and count and sort them:
```
tshark -r onehour.pcap -Y 'dns.flags.response == 0' -T fields -e dns.qry.name | grep -v -E 'net$|org$|com$' | sort | uniq -c | sort -rn
```

#### Similarly, on Windows 10 you can do:
```
"C:\Program Files\Wireshark\tshark.exe" -r onehour.pcap -Y "dns.flags.response == 0" -T fields -e dns.qry.name | sort /unique
```

#### Dump only the payload from a specific host as HEX
```
c:\Program Files\Wireshark>tshark.exe -r "testtest.pcapng" -Y "ip.src==192.168.1.133" -T fields -e data.data > "C:\Users\TonyE\PCAP\src_payload.txt"

# -r "testtest.pcapng": read in the file
# -Y "ip.src==192.168.1.133": Use Wireshark Display Filtering to find the host
# -T fields: format of output (this case I only want to see a field)
# -e data.data: this is the field I want to dump
# > "C:\Users\TonyE\PCAP\src_payload.txt": send output to specified file
```

## Change the Timestamp of packets
**Scenario:** You have an old PCAP from 2012 and you want to modify the timestamps of each packet to make it look like it was captured today.

1. Find the current timestamp of the first and last packet in epoch: 
 - **Windows:** `capinfos.exe -eS [filename.pcap]`
 - **Linux:** `capinfos -eS [filename.pcap]`
 > `-e` display the capture end time  
 > `-S` display start and end times as seconds

2. Determine your current time in epoch:
  - **Windows:** `powershell -command "(New-TimeSpan -Start (Get-Date "01/01/1970") -End (Get-Date)).TotalSeconds"`
  - **Linux:** `date +%s` 
4. Subtract the last packet in the pcap epoch time from the current epoch time. This becomes your offset.
5. Using `editcap`, add the offset to every timestamp for every packet in the pcap: 
```
editcap.exe -t [offset] -F [pcap|pcapng] <in-file.pcap> <out-file.pcap>
```




