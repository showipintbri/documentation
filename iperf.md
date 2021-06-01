# iperf
A collection of references because I always have to look them up.

### Important Notes:
- Server must be running first!
- Default Operation: Client pushes traffic to Server over TCP (simulated upload)
- Simulate Download: Use the `-R` flag on the Client for '*reverse*'

## Download
You must using the same iperf code base on both the client and the server.

For both `iperf` & `iperf3`:
- [https://iperf.fr/iperf-download.php](https://iperf.fr/iperf-download.php)

![](iperf image screenshot)

## Using `iperf3`



## Using `iperf`
#### Server:
```
iperf -s -D

-s      Start iperf as the server
-D      Start iperf as a daemon in the background
```
#### Client:
```
iperf -c xxx.xxx.xxx.xxx -P 8 -t 60 -i 2
  
-c <x...> Start iperf as a Client and connect to the Server: <xxx.xxx.xxx.xxx>
-P [#]    Parallel threads
-t [##]   Time: duration in seconds. Default is 10.
-i [#]    Update console with current summary every [#] seconds.
```

### Additional Flags Worth Noting
```
-u      Use UDP
-d      Bi-directional Testing (iperf ONLY, NOT iperf3)
```
