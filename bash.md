# `/bin/bash`


## `for` Loop w/ `case` condition: Download files using wget
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
