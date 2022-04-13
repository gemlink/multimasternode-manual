# multimasternode-setup

# To run this multimasternode setup You need to familiar with network setting on ubuntu 20.04.

 - before start this setup You should configure all necessary IP address for masternodes on Your ubuntu server

-----------------------------------------------------------------------------------------------------------------------------------------------------------
Manual to setup masternode on Ubuntu 20.04 system VPS


Minimal settings for VPS for each MN on server , when You want to have more masternodes You need to multiply this value by quantity of planned masternodes:

- 1 core on each 4 masternode minimum on start 4 core because of system
- 0.7 GB RAM on wach masternode
- 2 GB SWAP on each masternode
- 15 GB HDD on each masternode

- 25 GB one time for system 

</h>

Creating masternode on main wallet

If You use GemCore Wallet:

You can download it from : https://github.com/gemlink/gemcore/releases/latest </br>

1. Create new t-address or use empty t-address - give him label the same as planed masternode alias - it will be easier to find mn rewards.
2. Transfer to this t-address in one part 20000 coin.
3. After this You need wait about 2 confirmations.
4. Next go to masternode setup on GemCore:
	- press button Get Private Key"
	- pres button "Generate Masternode Data"
	- You will see below mndata - check the box on the left , data will be automaticaly move to free fields
	- copy to clipboard mn prv key generated with transacrion ID and index, it will be necessary to vps script
	- in "Alias name" field write <alias name> - the same as You add for t-address label
	- in "VPS IP" - ip address Your VPS - where will be mn set 

	## DON'T start the masternode You need to run first script on vps.
	
---------------------------------------------------------------------------------------------------------------------
# configure masternode managment system on VPS (AMI-CLI , GLINK.NODE , AMI-ALIS)

1. Update Your ubuntu and install necessary package.


```
cd

sudo su

apt update 

apt upgrade -y

apt install wget zip unzip gpw curl libgomp1 git -y

```

2.Now create new directory for Your application:

```

mkdir /mns

```

3.Download and install necessary files:


```

wget -q https://raw.githubusercontent.com/alis-is/ami/master/install.sh -O /tmp/install.sh && sh /tmp/install.sh

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

nano ./alis.json

```

6. Copy to alis.json : 
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
]
}
    ```

    If You want to run only one masternode You are finished , You can save file with "ctrl-x" and save data. If You want to add next masternode You need to
  add next part of confifuration. Each masternode need in alis.json this part added after "}"  and before last "]" in alis.json. 
  
 ```
    ,{
      "id": "gemlink2",
      "type": "glink.node",
      "configuration": {
        "DAEMON_CONFIGURATION": {
          "bind": "Your MN ip address 2",
          "rpcbind": "127.0.0.1",
          "masternodeprivkey": "your_mn_key 2"
        }
      }
    },
    ...
```

7.Next after configure Your masternode write command:

 alis-cli all bootstrap

this command will download all bc for Your masternode

8.You can start Your masternode: 

```
  alis-cli all start
```

9.Last command when You can check all mn status: 

```

alis-cli all info

```

10. after this command You should see info about mn status.

---------------------------------------------------------------------------------------------------------------------

#Example commands

Setup all apps alis-cli all setup

Setup app with id node1 alis-cli node1 setup

Get info of all apps alis-cli all info

For all alis-cli specific options run alis-cli --help

-----------------------------------------------------------------------------------------------------------------------------




