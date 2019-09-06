# How to build a CVN
In this HOWTO we suppose that you are going to install into the folder /opt/faircoin. You do not need to run any step of this guide as root user except for the first two steps.
```
sudo mkdir /opt/faircoin
sudo chown <insertYourUserName>.<insertYourUserName> /opt/faircoin
```
## 1 Compile the FairCoin wallet
This document assumes that you have all the required development packages already installed on your system.

if you are using Ubuntu (tested with both 16.04 and 18.04), then an easy way to install all required dependencies and avoid using the `--with-incompatible-bdb` parameter in `configure` script is to first add bitcoin's PPA and then install sll required dependencies as follows:

````
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt update 
sudo apt install automake libtool build-essential libtool autotools-dev autoconf pkg-config libssl-dev libboost-all-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libqrencode-dev autoconf openssl libssl-dev libevent-dev libminiupnpc-dev libzmq3-dev qtchooser libdb4.8-dev libdb4.8++-dev
````
after that you can proceed as usual: 

```
cd /opt/faircoin
git clone https://github.com/faircoin/faircoin.git
cd faircoin
./autogen.sh
./configure --disable-tests --disable-bench --with-incompatible-bdb --with-gui=qt5 --with-cvn
make -j`nproc`
```

Note: if compiling on a Raspberry PI execute a plain make else it will run out of memory:  
```
make
```

## 2 Run the FairCoin wallet in CVN mode
Please make sure to start your FairCoin wallet in normal mode first and let it download the complete block chain before restaring it as a CVN.

This is how to start the wallet software as a daemon:  
```/opt/faircoin/faircoin/src/faircoind -daemon ```

This is how to start the wallet software with GUI:  
```/opt/faircoin/faircoin/src/qt/faircoin-qt ```

There are two ways to run a CVN.  
1. By using Fasito (FairCoin signature token) which contains all the information required  
2. By using an x509 Key/certificate pair which containls all the information required (for testing only)  

### 3.1 Using Fasito
The Fasito is provided by the FairCoin development team. Once you have received the token plug it into a USB port and start the wallet using the parameters ```-cvn=fasito -gen=1 ```
### 3.2 Using an x509 Key/certificate pair
The wallet searches for a file named cvn.pem in the FairCoin data directory (in Linux ~/.faircoin2)

Start the wallet with the arguments: ```-cvn=file -gen=1 ```
