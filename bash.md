# `/bin/bash`


## `for` Loop w/ `case` condition:
This small script is to download the series of \*.pcaps from: [https://www.netresec.com/?page=MACCDC](https://www.netresec.com/?page=MACCDC)

The filenames are as follows, where the "00005" file is named differently:

- maccdc2012_00000.pcap.gz
- maccdc2012_00001.pcap.gz
- maccdc2012_00002.pcap.gz
- maccdc2012_00003.pcap.gz
- maccdc2012_00004.pcap.gz
- maccdc2012_00005_fixed.pcap.gz
- maccdc2012_00006.pcap.gz
- maccdc2012_00007.pcap.gz
- maccdc2012_00008.pcap.gz
- maccdc2012_00009.pcap.gz
- maccdc2012_00010.pcap.gz
- maccdc2012_00011.pcap.gz
- maccdc2012_00012.pcap.gz
- maccdc2012_00013.pcap.gz
- maccdc2012_00014.pcap.gz
- maccdc2012_00015.pcap.gz
- maccdc2012_00016.pcap.gz

```bash
#!/bin/bash

for num in {00..16}
do
  case $num in
    05)
      name=maccdc2012_000$num\_fixed.pcap.gz
      echo $name
      ;;
    *)
      name=maccdc2012_000$num.pcap.gz
      echo $name
      ;;
  esac

  wget https://download.netresec.com/pcap/maccdc-2012/$name

done
exit 0

```


## `for` Loop: Unzip a directory of files
```bash
#!/bin/bash

for filename in ./*.gz
do
  unzip $filename
done
exit 0

```
