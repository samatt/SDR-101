#Running Yate BTS with bladeRF on Ubuntu

To run Yate BTS with Blade RF on Ubuntu you need the following things:

1. [BladeRF](https://github.com/Nuand/bladeRF)
2. [Yate](http://yate.null.ro/) 
3. [Yate BTS](http://wiki.yatebts.com/index.php/Main_Page)
4. [Yate BTS Network In The Box](http://wiki.yatebts.com/index.php/Network_in_a_Box#Web_UI_for_NIB_Management)

###Note
There is a difference between `Yate` and `yatesBTS`. `Yate` stands for *Yet Another Telephony Engine*, and like the name states it is mainly a telephony engine; while currently focused on Voice over Internet Protocol (VoIP) and PSTN, its power lies in its ability to be easily extended. Voice, video, data and instant messenging can all be unified under Yate's flexible routing engine, maximizing communications efficiency and minimizing infrastructure costs for businesses.

##OpenBTS vs YateBTS
The image below is from their website. They provide more information [here](http://wiki.yatebts.com/index.php/File:Yatebts_vs_openbts1.png)

![OpenBts vs Yates](http://wiki.yatebts.com/images/7/7b/Yatebts_vs_openbts1.png)

##bladeRF

To get the bladeRF set up you need to install the [bladeRF-cli](https://github.com/Nuand/bladeRF/tree/master/host/utilities/bladeRF-cli) ,firmware updates and an image fot the Cyclone FPGA board.

If you are running **Ubuntu 14.04** follow the [easy installation](https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Linux#Easy_installation_for_Ubuntu_The_bladeRF_PPA).

***They dont seem to have src for Ubuntu 12.04***.

Or at least I didnt find it, for other distros [here](https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Linux#Building_bladeRF_libraries_and_tools_from_source) is a good resource for compiling it yourself.


They also describe building [GNU Radio](https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Linux#building-gnu-radio-and-gr-osmosdr) but not required if you're using it with Yate.

[Good guide for basic device operation](https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Verifying-Basic-Device-Operation´


##Yate

I tried building it from source from svn however I kept running into a *gpg public key missing error* when i tried this so I ended up installing it from source. You can get the source from [here](http://yate.null.ro/pmwiki/index.php?n=Main.Download)
###Note
if you get this error

```
yate: error while loading shared libraries: libyate.so.5.2.0: cannot open shared object file: No such file or directory
```

Then run

```
export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
```
You could now make sure you have installed a valid version of Yate by running these commands:

```
updatedb
locate libyate
```

##Yate-BTS

Now you're ready to install yate BTS. For a sanity check [here](http://wiki.yatebts.com/index.php/Installing) is what they say about installing it. Basically covers the above two sections.

So to get it up and running all thats left is to set-up a few config files. And in essence that is what 'installing Yate-BTS' is. Its actually already installed when you install Yate.
So are the config files, if you did an install with default paths that should be available at `/usr/local/etc/yate/`


 
So to set-up a [minimum config to start Yate BTS](http://wiki.yatebts.com/index.php/Minimum_configuration_to_start_your_YateBTS) we need to add a few parameters to some config files, namely `ybts.conf`.  Add the following two sections:


Since we're using a bladeRF we need to till Yate thats our tranceiver.

```
[transceiver]
;Path to the transceiver relative to where MBTS is started.
;Should be one of: ./transceiver-bladerf ./transceiver-rad1 ./transceiver-usrp1 ./transceiver-uhd.
;Defaults to ./transceiver
Path=./transceiver-bladerf
```


```
[gsm]
Radio.Band=850
Radio.C0=50
```

For more details on what you can set in the ybts.conf file [this](http://wiki.yatebts.com/index.php/Ybts.conf) is a good reference. Now that Yate BTS is configured we need to run an application to actually run. This is the Yate BTS Network In a Box (NITB).


##Yates BTS NITB

Details [here](http://wiki.yatebts.com/index.php/Network_in_a_Box)

Prerequesites for NITB:

 * YateBTS
 * PHP
 * Apache
 * pySIM(not essential)


We have already covered YateBTS. You can install PHP and Apache with 

`sudo apt-get install apache2`

`sudo apt-get install php5`

`sudo apt-get install libapache2-mod-php5`

`sudo /etc/init.d/apache2 restart`

if you installed apache correctly you should see a splash page when you got to 127.0.0.1 on the browser

If that worked you can now copy the NITB Web Gui into that folder:

```
ln -s /usr/local/share/yate/nib_web nib
```

###Configuration of the Web GUI

Before using the interface, the Web GUI must be given write right to Yate's configuration directory.
The command that has to be run as root is:

`chmod a+rw /etc/yate (If yatebts is installed from package.)`

or  

`chmod a+rw /usr/local/etc/yate/ (If yatebts is installed from sources.)`



##Run it!

```
yate -sd -vvvvv -l /var/log/yate.log
```

This will start Yate as a supervised (-s) daemon (-d) , with maximum debug level (-vvvvv) and logs saved in /var/log/yate.log.

```
telnet 127.0.0.1 5038
```

To see it running open a web browser and navigate to:

```
127.0.0.1/nib
```
 
 Detailed info on running [here](http://wiki.yatebts.com/index.php/Running)
###Further Info

Good breakdown of [GSM](http://wiki.yatebts.com/index.php/GSM_Concepts) and [Radio](http://wiki.yatebts.com/index.php/GSM_Concepts) concepts.