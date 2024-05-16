# Homelab Inventory Base Commands

## Network

###  Network Switches
I use Ubiquiti switches. These support connections via SSH and it's possible to get switch information from them. Mostly this is good to get MAC addresses.

```ssh admin@10.0.0.2 'swctrl mac show' | sort -n```



### Firewall
I use pfSense. The easiest thing to do is to backup your configuration and parse the file out. But that won't show hosts that you don't have static leases configured for. 

I have also installed the pfSense API and use the following aliases:

```
pharmdevices() {  curl -H 'Authorization: SHORT-HEX LONG-HEX' -sk 'http://10.0.0.1/api/v1/services/dhcpd/static_mapping?interface=lan' | jq -c '.data[] | [ .ipaddr, .hostname, .mac ]' ; }
pharmleases() {  curl -H 'Authorization: SHORT-HEX LONG-HEX' -sk 'http://10.0.0.1/api/v1/services/dhcpd/lease' | jq -c '.data[]' ; }
pharmarp() {  curl -H 'Authorization: SHORT-HEX LONG-HEX' -sk 'http://10.0.0.1/api/v1/system/arp' | jq -c '.data[] | [.interface, .mac, .ip]'  ; }
```

### Scanning
nmap is the tool of choice. 

# Hosts Commands

## TrueNAS / FreeBSD
This is a little less comprehensive than Linux.

### Disks
```camcontrol devlist``` followed by ```smartctl -i /dev/da0```.

## Linux Hosts
### General
```hwinfo --short``` is good. 

### Hard Disks
```lshw -class disk``` shows good information.

A better command is ```lsblk -io KNAME,TYPE,SIZE,MODEL``` 

For more details about individual devices use something like ```smartctl -i /dev/sda``` which will include serial numbers, RPMs, etc..

You can also run fdisk on each one, which it looks like ```sfdisk -l``` does for you. Allegedly it includes non-mounted disks which seems nice. 


### PCI Cards
```lspci``` shows good information

### Memory
```free``` is quick. ```dmidecode``` is more comprehensive. 
