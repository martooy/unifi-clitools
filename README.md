# Homelab Inventory Base Commands

## Network

### Get a key or password for your devices

You can setup a username/password but doing key based auth seems wise:

`ssh-keygen -t rsa -f ~/.ssh/unifi-apikey`

Then copy the public key from `cat ~/.ssh/unifi-apikey.pub` over to Unifi under System > Advanced > Device Authentication.

For the examples below we will assume you're doing key auth to a user named admin and the key file is in an environment variable like this:

`export PFKEY=~/.ssh/unifi-apikey`

###  Network Switches
I use Ubiquiti switches. These support connections via SSH and it's possible to get switch information from them. Mostly this is good to get MAC addresses.

```for i in 10.0.0.2 10.0.0.7 10.0.0.8 ; do ssh -i $PFKEY admin@$i 'swctrl mac show' | sort -n ; done```

Show some port stuff:

`for i in 10.0.0.2 10.0.0.7 10.0.0.8 ; do ssh -i $PFKEY admin@$i 'swctrl  port show' | sort -n ; done`
