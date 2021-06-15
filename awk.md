# awk

### Extract a string when the field seperator is multiple special characters
If from the below sample you're trying to extract only the portion: `[1:2021701:1] ET GAMES MINECRAFT Server response inbound` so you can sort/uniq/sort them you can use `awk`.
This example will grab the second *field* using the double-escaped field-seperator.
```
user@host:~/suricata/logs$ tail fast.log
06/15/2021-21:07:12.992287  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 172.65.196.232:25565 -> 192.168.1.144:54558
06/15/2021-21:07:13.256219  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.173:25565 -> 192.168.1.144:54560
06/15/2021-21:07:13.256219  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.173:25565 -> 192.168.1.144:54560
06/15/2021-21:07:13.355813  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.173:25565 -> 192.168.1.144:54560
06/15/2021-21:17:19.676883  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.172:25565 -> 192.168.1.144:54663
06/15/2021-21:17:19.676883  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.172:25565 -> 192.168.1.144:54663
06/15/2021-21:17:19.774643  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 185.116.156.172:25565 -> 192.168.1.144:54663
06/15/2021-21:17:23.704600  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 194.213.3.206:25565 -> 192.168.1.144:54665
06/15/2021-21:17:23.793534  [**] [1:2021701:1] ET GAMES MINECRAFT Server response inbound [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 194.213.3.206:25565 -> 192.168.1.144:54665
06/15/2021-21:17:27.995217  [**] [1:2028651:2] ET USER_AGENTS Steam HTTP Client User-Agent [**] [Classification: Potential Corporate Privacy Violation] [Priority: 1] {TCP} 192.168.1.144:54671 -> 104.96.220.123:80

user@host:~/suricata/logs$ awk  'BEGIN { FS = "\\[\\*\\*\\]" }; { print $2 }' ./fast.log | sort | uniq -c | sort -n -r
  11122  [1:2033078:2] ET INFO Session Traversal Utilities for NAT (STUN Binding Request On Non-Standard High Port)
    849  [1:2013031:9] ET POLICY Python-urllib/ Suspicious User Agent
    184  [1:2016150:2] ET INFO Session Traversal Utilities for NAT (STUN Binding Response)
    184  [1:2016149:2] ET INFO Session Traversal Utilities for NAT (STUN Binding Request)
    135  [1:2021701:1] ET GAMES MINECRAFT Server response inbound
     43  [1:2013504:6] ET POLICY GNU/Linux APT User-Agent Outbound likely related to package management
     31  [1:2028651:2] ET USER_AGENTS Steam HTTP Client User-Agent
     26  [1:2230010:1] SURICATA TLS invalid record/traffic
     26  [1:2230002:1] SURICATA TLS invalid record type
      7  [1:2014799:5] ET POLICY OpenVPN Update Check
      5  [1:2100498:7] GPL ATTACK_RESPONSE id check returned root
      4  [1:2029709:2] ET HUNTING Suspicious Domain Request for Possible COVID-19 Domain M1
      3  [1:2027870:4] ET INFO Observed DNS Query to .world TLD
      2  [1:2029707:7] ET HUNTING Suspicious TLS SNI Request for Possible COVID-19 Domain M1
      1  [1:2260002:1] SURICATA Applayer Detect protocol only one direction
      1  [1:2221010:1] SURICATA HTTP unable to match response to request
```
