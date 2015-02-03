#Running OsmoBTS with USRP1 Ubuntu 12.04



This document assumes you're running **Ubuntu 12.04**.

##Overview

There are many components that can be split into two sections and should be installed in the following order

### Install
 * Dependencies 
 	* [Ubutu](#Ubuntu)
	* [GNU Radio with libusrp](#GNURadio)

 * Osmo Core
	* [libosmocore](#libosmocore)
	* [libosmo-abis](#libosmo-abis)
	* [OpenBSC](#OpenBSC)
	* [OsmoBTS](#OsmoBTS)
	* [OsmoTRX](#OsmoTRX)
 	
 	
###Setup Configuration files

Once you're done with all of that you need to set up some config files.


###Run
Once everything is installed and the configuration files have been made you can follow the instrcutions in the Running section to set up your network.



* * *
####Important Note

From the USRP2 onwards Ettus combined the library for all their models into a [Universal Hardware Driver (UHD)](http://code.ettus.com/redmine/ettus/projects/uhd/wiki). If you are using a USPR1 you need to install `libusrp` if you are using a newer USRP you will need the UHD.


So the gist of it is **to use the USRP1 we need to install libusrp**.

libusrp is annoying because it **no longer ships with GNU Radio**. The only way to install it is to download the source for **GNU Radio 3.4.2** and compile it for your system. This is a bit of a long winded process and has its own dependencies. 
****

##Install

###Ubuntu

Here are some packages that are required for installing everything.

Before installing make sure to update

```
sudo apt-get update
```

You can get a list of the packages you require from the [GNU Radio website](http://gnuradio.org/redmine/projects/gnuradio/wiki/UbuntuInstall#Install-the-Pre-Requisites)


Since we are running Ubuntu 12.04 (Precise Pangolin) we need to run the following command:

```
sudo apt-get -y install git-core autoconf automake  libtool g++ python-dev swig \
pkg-config libboost1.48-all-dev libfftw3-dev libcppunit-dev libgsl0-dev \
libusb-dev sdcc libsdl1.2-dev python-wxgtk2.8 python-numpy \
python-cheetah python-lxml doxygen python-qt4 python-qwt5-qt4 libxi-dev \
libqt4-opengl-dev libqwt5-qt4-dev libfontconfig1-dev libxrender-dev 
```

We will also need to install these additional packages

```
sudo apt-get install libdbi-dev 
sudo apt-get install libdbd-mysql
sudo apt-get install libdbd-pgsql 
sudo apt-get install libdbd-sqlite3
```

After installing the packages run update again

```
sudo apt-get update
```


###GNURadio

Okay now that you have installed all the dependencies you are ready to install GNU Radio. As mentioned above we are doing this becasue we want to use the USRP1. The USRP1 requires `libusrp` to be installed on your os in order to talk to it. `libusrp` used to come with GNU Radio but they no longer support it. We need to download an old version of GNU Radio and build it. 

Start at the root folder 

```
cd ~
```
Download the GNU Radio tarball

```
wget http://gnuradio.org/releases/gnuradio/gnuradio-3.4.2.tar.gz
```

Untar the file

```
tar -xvzf gnuradio-3.4.2.tar.gz
```

You'll need to download the following things. More info at this [link]( http://sourceforge.net/p/openbts/mailman/message/31110982/)

```
sudo apt-get install swig
sudo apt-get install libusb-1.0-0-dev
sudo apt-get install sdcc
sudo apt-get install libaudio-dev 
```

cd into the previously untarred folder

```
cd gnuradio-3.4.2
```

Run the configure script to install it. 

```
sudo ./configure --enable-usrp --disable-gr-uhd --with-fusb-tech=libusb1
```

Note the flags we set to configure:

 * `--enable-usrp`: This makes sure libusrp is installed with the build
 * `--disable-gr-uhd`: This disables the UHD from installing and is optional. Remove this option if you want to use any USRP device that depends on the UHD.
* `--with-fusb-tech=libusb1`: Requirement because of some weird libusb issue that i don't remember.

after configure run:

```
sudo make
```

Once you have run make the output should inform you whether usrp was built or not.
You can also check the directory to make sure its there (using `ls`).

if its there lets install. 

```
sudo make install
```
This basically just copies it to the right directories so your os knows its there (`/usr/local/lib` by default )

you shoudld be able to confirm its there by running the following command

```
ls /usr/local/lib/ | grep libusrp
```

Once libusrp is installed you need one more package before installing the Osmo system. That is:

```
apt-get install libortp-dev
```

##libosmocore

Go to your root directory

```
cd ~
```

Clone the libosmocore repo


```
git clone git://git.osmocom.org/libosmocore.git
```
 
After cloning cd into the directory and run the configure script


```
cd libosmocore
sudo autoreconf -i
sudo ./configure
```

- - - 

You might need to get libpcslite. If you do run the following command.


```
sudo apt-get install libpcslite-dev
```

- - -



Once configured make, install and update the linker (ldconfig)

```
sudo make && make install && ldconfig
cd ..
```
##libosmo-abis

Go to your root directory

```
cd ~
```

Clone the repo

```
git clone git://git.osmocom.org/libosmo-abis.git

cd libosmo-abis
```

Change the branch from the default master

```
git checkout -b jolly/multi-trx origin/jolly/multi-trx
```
Configure and build

```
sudo autoreconf -i
sudo ./configure
```
* * *
sometimes it is necessary to point to different .../lib/pkgconfig/ path: 

```
PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/ 
./configure 
```
* * * 



```
sudo make && make install && ldconfig
cd ..
```


###OpenBSC
Go to root dir. Clone the repo and cd to openbsc sub-folder

```
cd ~
git clone git://git.osmocom.org/openbsc.git
cd openbsc/openbsc/
```

Change the branch

```
git checkout -b jolly/testing origin/jolly/testing
```


Configure build and install

```
autoreconf -i
./configure
sudo make && make install && ldconfig
```

**If you have trouble make sure you have installed all the packages mentioned in the [Ubuntu](#Ubuntu) section.**

[This](http://www.h-online.com/open/features/Building-a-GSM-network-with-open-source-1476745.html%3Fanchor=openbsc) is a decent explanation for the difference between OpenBTS and OpenBSC.

###OsmoBTS
Go to root dir and clone repo. Cd into repo folder

```
cd ~
git clone git://git.osmocom.org/osmo-bts.git
cd osmo-bts
```
Change the branch

```
sudo git checkout -b jolly/trx origin/jolly/trx
```

Configure,build, install. **Watch out for the extra flag on configure**

```
sudo autoreconf -i
sudo ./configure --enable-trx
sudo make && make install && ldconfig
```

###OsmoTRX

Go to root dir and clone repo. Cd into repo folder

```
cd ~
sudo git clone git://git.osmocom.org/osmo-trx
cd osmo-trx/
```


Configure, build, insta. **Watch out for the USRP flag**

```
sudo autoreconf -i
sudo ./configure --with-usrp1
sudo make 
sudo make install 

```

###Setup Configuration files

You need to setup a few configuration files to tell help osmo configure the USRP and network correctly.

To set up osmoBTS you need to add the osmo-bts.cfg file. 


```
mkdir ~/.osmocom
sudo gedit ~/.osmocom/osmo-bts.cfg
```
and paste this in

```
bts 0
 band DCS1800
 ipa unit-id 1801 0
 oml remote-ip 127.0.0.1
 rtp bind-ip 127.0.0.1
 rtp jitter-buffer 0
 paging lifetime 0
 gsmtap-sapi bcch
 gsmtap-sapi ccch
 gsmtap-sapi rach
 gsmtap-sapi agch
 gsmtap-sapi pch
 gsmtap-sapi sdcch
 gsmtap-sapi pacch
 gsmtap-sapi pdtch
 gsmtap-sapi sacch
 fn-advance 20
 ms-power-loop -10
 timing-advance-loop
 trx 0
  rxgain 0
  power 0
```
save and exit


after osmo BTS you need to add the open-bsc.cfg file.

```
mkdir ~/.osmocom
edit ~/.osmocom/open-bsc.cfg
```
and paste this in 

```
e1_input
 e1_line 0 driver ipa
 e1_line 0 port 0
network
 network country code 262
 mobile network code 42
 short name OpenBSC
 long name OpenBSC
 auth policy accept-all
 location updating reject cause 13
 encryption a5 0
 neci 1
 paging any use tch 0
 rrlp mode ms-based
 mm info 1
 handover 0
 handover window rxlev averaging 10
 handover window rxqual averaging 1
 handover window rxlev neighbor averaging 10
 handover power budget interval 6
 handover power budget hysteresis 3
 handover maximum distance 9999
 timer t3101 10
 timer t3103 0
 timer t3105 0
 timer t3107 0
 timer t3109 0
 timer t3111 0
 timer t3113 60
 timer t3115 0
 timer t3117 0
 timer t3119 0
 timer t3122 10
 timer t3141 0
 dtx-used 0
 subscriber-keep-in-ram 0
 bts 0
  type sysmobts
  band DCS1800
  cell_identity 0
  location_area_code 1
  training_sequence_code 7
  base_station_id_code 63
  ms max power 0
  cell reselection hysteresis 4
  rxlev access min 0
  periodic location update 30
  channel allocator descending
  rach tx integer 9
  rach max transmission 7
  channel-descrption attach 1
  channel-descrption bs-pa-mfrms 5
  channel-descrption bs-ag-blks-res 1
  ip.access unit_id 1801 0
  oml ip.access stream_id 255 line 0
  neighbor-list mode automatic
  trx 0
   rf_locked 0
   arfcn 869
   nominal power 0
   max_power_red 0
   rsl e1 tei 0
    timeslot 0
     phys_chan_config CCCH+SDCCH4
     hopping enabled 0
    timeslot 1
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 2
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 3
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 4
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 5
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 6
     phys_chan_config TCH/F
     hopping enabled 0
    timeslot 7
     phys_chan_config TCH/F
     hopping enabled 0
```

save and exit


###Running 

Almost there. If everything went well you should be ready to run the GSM Network In a Box. 
Power up the  USRP1  and connect it to your computer via USB.

In order to run it you need to run three programs in three seperate terminal windows. 
Execute the following three commands in this order. Each in its own terminal window


Open a shell and start OpenBSC:

```
sudo osmo-nitb -c ~/.osmocom/open-bsc.cfg -l ~/.osmocom/hlr.sqlite3 -P -C --debug=DRLL:DCC:DMM:DRR:DRSL:DNM
```

Open a shell and start OsmoBTS:

```
sudo osmobts-trx -c ~/.osmocom/osmo-bts.cfg
```
 
Open a shell and start OsmoTRX

```
sudo osmo-trx

```
