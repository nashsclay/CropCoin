# MasterNodeMasterScript
Shell script to install a [Cropcoin Masternode](https://bitcointalk.org/index.php?topic=2863802.0) on a Linux server running Ubuntu 16.04. Use it on your own risk.  
***

## Installation for v1.1.0.4:
```
wget -N https://raw.githubusercontent.com/zoldur/CropCoin/master/cropcoin.sh
bash cropcoin.sh
```
***

## Multiple MN on one VPS:

It is now possible to run multiple **CropCoin** Master Nodes on the same VPS. Each MN will run under a different user you will choose during installation.  
***

## Usage:

For security reasons **CropCoin** is installed under a normal user, usually **cropcoin**, hence you need to **su - cropcoin** before checking:  
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
