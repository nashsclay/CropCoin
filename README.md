# MasterNodeMasterScript
Shell script to install a number of Masternodes! Here is the list of current supported Masternodes. 

Please note to run this script you MUST:  

1) **SUBJECT TO CHANGE** Be root (for intial install) - better to login as root then use the sudo command  
2) Ubuntu 16.04 - I only tested this on that version but I'm sure others may work.  
3) A VPS server - You can use my referal link here --> https://www.vultr.com/?ref=7415368  
4) Digital Ocean is another provider you can use instead of Vultr. There are many more but these are very common to use. You can use any size due to the swap file that will be created if your server is below 2GB of memory.  
5) Optional - There may be other types of servers you can use but not listed here.  

***

## Installation for Masternode Master Script:
```
wget -N https://raw.githubusercontent.com/nashsclay/MasterNodeMasterScript/master/MasterNodeMasterScript.sh
chmod +x MasterNodeMasterScript.sh
./MasterNodeMasterScript.sh
```
***

## Multiple MN on one VPS:

It is now possible to run multiple Master Nodes on the same VPS! Each MN will run under a different user you will choose during installation. You will also need a different IP address for each MN. To set this up on vultr, use these directions. **LINK HERE**  
***

## Usage:

For security reasons all Masternodes are installed under a normal user, usually the coin name is the username, hence you need to **su - coinname** before checking:  
```
CROPUSER=cropcoin #replace cropcoin with the MN username you want to check  
su - $CROPUSER
cropcoind masternode status  
cropcoind getinfo
```
Also, if you want to check/start/stop **cropcoin** daemon for a particular MN, run one of the following commands as **root**:
```
CROPUSER=cropcoin  #replace cropcoin with the MN username you want to check  
systemctl status $CROPUSER #To check the service is running  
systemctl start $CROPUSER #To start cropcoind service  
systemctl stop $CROPUSER #To stop cropcpoind service  
systemctl is-enabled $CROPUSER #To check cropcoind service is enabled on boot  
```
***

## Wallet re-sync

If you need to resync the wallet, run the following commands as **root**:
```
CROPUSER=cropcoin  #replace cropcoin with the MN username you want to resync
systemctl stop $CROPUSER
rm -r /home/$CROPUSER/.cropcoin/{banlist.dat,blk0001.dat,database,db.log,mncache.dat,peers.dat,smsgDB,smsg.ini,txleveldb}
systemctl start $CROPUSER
```
***

## Wallet update to 1.1.0.4
Run the following commands as **root** to update **CropCoin** to version **1.1.0.4**
```
cd /tmp
for crop in $(grep -l cropcoind /etc/systemd/system/*.service | awk -F"/" '{print $NF}'); do systemctl stop $crop; done
rm cropcoind cropcoind.gz
wget -N https://github.com/zoldur/CropCoin/releases/download/v.1.1.0.4/cropcoind.gz
gunzip cropcoind.gz
chmod +x cropcoind
mv cropcoind /usr/local/bin
for crop in $(grep -l cropcoind /etc/systemd/system/*.service | awk -F"/" '{print $NF}'); do systemctl start $crop; done
```

## Acknowledgements:

Script base code forked from Zoldur: https://github.com/zoldur/
Any questions? You may contact me on Discord. nashsclay#6809
Please make sure you mention what coin/script you are messaging me about. Thank you.
