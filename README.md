# To run this multimasternode setup You need to be familiar with the network settings on ubuntu 20.04+.

 - before start this setup You should configure all necessary IP address for masternodes on Your ubuntu server

---
Manual to setup masternode on Ubuntu 20.04+ system VPS


Minimal settings for VPS for each MN on server , when You want to have more masternodes You need to multiply this value by quantity of planned masternodes:

- 1 core on each 4 masternode minimum on start 4 core because of system
- 0.7 GB RAM on each masternode minimum on start 2GB 
- 2 GB SWAP on each masternode 
- 15 GB HDD on each masternode 
- extra 25 GB one time for system 
- addresses ipv4 or ipv6 one for each masternode 

</h>

Creating masternode on main wallet

If You use MiracleBox Wallet:

You can download it from : https://gemlink.org/miraclebox </br>
1. Create new t-address or use empty t-address - give it label the same as planed masternode alias - it will be easier to find mn rewards.
2. Transfer to this t-address in one transaction 20000 coin.
3. After this you need wait about 2 confirmations.
4. Next go to masternode setup on GemCore:
	- press button `Get Private Key`
	- pres button "Generate Masternode Data"
	- you will see below `mndata` - check the box on the left , data will be automatically move to free fields
	- copy to clipboard mn prv key generated with transaction ID and index, it will be necessary to vps script
	- in `Alias name` field write the name you want to give to your masternode - the same as you add for t-address label
	- in `VPS IP` - ip address of Your VPS - where will be mn set 

### DON'T start the masternode You need to configure first all on your vps.
	
---
## configure masternode management system on VPS (AMI-CLI , GLINK.NODE , ALIS-CLI)

1. Update Your ubuntu and install necessary packages.


```
cd
sudo su
apt update 
apt upgrade -y
apt install wget zip unzip gpw curl libgomp1 git -y
```

2.Now create new directory for your application:

```
mkdir /mns
```

3.Download and install necessary files:

```
wget -q https://raw.githubusercontent.com/alis-is/alis-cli/master/install.sh -O /tmp/install.sh && sh /tmp/install.sh
```

4.Next You need to create place for config file and config file:

```
mkdir /etc/alis-cli
touch /etc/alis-cli/alis.json
cd /etc/alis-cli
```
  
5.Next You need to prepare config file:

```
nano ./alis.hjson
```

6. Copy to alis.hjson : 

```
{
  "global": {
    "user": "glink",
    "appDirectory": "/mns/",
    "configuration": {
      "DAEMON_CONFIGURATION": {
        "port": 16113,
        "txindex": 1,
        "masternode": 1,
        "rpcallowip": "127.0.0.0/8"
        "addnode": "15.235.142.201"
        "addnode": "193.25.2.237"
      }
    }
  },
  "maintenance": {
    "updateAlis": false,
    "updateApps": false
  },
  "apps": [
    {
      "id": "[masternodealias]",
      "type": "glink.node",
      "configuration": {
        "DAEMON_CONFIGURATION": {
          "bind": "[Your MN ip address for first masternode]",
          "rpcbind": "127.0.0.1",
          "masternodeprivkey": "[your_mn_key for first masternode]"
        }
      }
    }
#<------- above here add next section about next masternode configuration,  if You want to have more than one masternode 
]
}
```

If You want to run only one masternode, thats all. You can save file with "ctrl-x" ,save data and go to next point. If You want to add next masternode You need to
add next part of configuration. Each next masternode need in alis.json this part added in this place "#<----------" in alis.json. You can always add new part of    alis.json file with new masternode after, when You will configure new masternode on gemcore. 

```
     ,{
      "id": "[masternodealias]",
      "type": "glink.node",
      "configuration": {
        "DAEMON_CONFIGURATION": {
          "bind": "[Your MN ip address for second masternode]",
          "rpcbind": "127.0.0.2",
          "masternodeprivkey": "[your_mn_key for second masternode]"
         }
       }
      }
```

# It's very important to have configured in linux network settings all of Your masternode IPs

7.Next you need to setup Your masternode:

```
alis-cli all setup
```

of course you can setup only selected masternode with :

```
alis-cli [mnalias] setup
```

After this You can download bootstrap for all configured masternodes:

```
 alis-cli all bootstrap
```

or if you will and new masternode You can add bootstrap only for new one with :

```
alis-cli [mnalias] bootstrap
```

8.You can start Your masternode: 

first time for all configured masternodes:
```
  alis-cli all start
```
or if You will add new masternode , You can start only new one with
```
  alis-cli [mnalias] start
```

9.Last command where You can check all mn status: 
```
alis-cli all info
```
10. After this command You should see info about mn status.
---

### Example commands

Setup all apps: </br>
```alis-cli all setup```

Setup app with id node1: </br>
```alis-cli node1 setup```

Get info of all apps: </br>
```alis-cli all info```

For all alis-cli specific options run: </br>
```alis-cli --help```

Clear ami cache: </br>
```ami --erase-cache```

---
### Special thanks for help: 

- [cryi] (https://github.com/cryi)
- [ciripel] (https://github.com/ciripel)

thanks guys

---

If You want to know more look here:

[alis.is](https://github.com/alis-is)



