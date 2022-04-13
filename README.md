# multimasternode-manual

cd

sudo su

apt update 

apt upgrade -y

apt install wget zip unzip gpw curl libgomp1 git php-cli -y

mkdir /mns

wget -q https://raw.githubusercontent.com/alis-is/ami/master/install.sh -O /tmp/install.sh && sh /tmp/install.sh

wget -q https://raw.githubusercontent.com/alis-is/alis-cli/master/install.sh -O /tmp/install.sh && sh /tmp/install.sh


mkdir /etc/alis-cli

touch /etc/alis-cli/alis.json

#Finished instalation

Next You need to prepare config file:

create file in /etc/alis-cli/alis.json

each { after "apps": [ - open new masternode configuration

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
      "id": "gemlink1",
      "type": "glink.node",
      "configuration": {
        "DAEMON_CONFIGURATION": {
          "bind": "Your MN ip address 1",
          "rpcbind": "127.0.0.1",
          "masternodeprivkey": "your_mn_key 1"
        }
      }
    },
    {
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
]
}

Next after configure Your masternode write command:

 alis-cli all bootstrap

this command will download all bc for Your masternode

and last command to start your masternode 

alis-cli all start

after this when You will start masternode You can check status , 


alis-cli all info

You should see info like this :
