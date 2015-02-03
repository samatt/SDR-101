#Artsec SDR Meetup


##Getting Started


###What are Software Defined Radios?

Currently most radios you can buy off-the-shelf are designed to do one thing. Like Bluetooth, or Wi-Fi or GSM. However Software Defined Radios allow you to use the same hardware to "program different radio architectures". The analogy I like to use is that current radi chipsets are like calculators and software defined radios are computers.

*"Radio components such as modulators, demodulators and amplifiers are traditionally implemented in hardware components. The advent of modern computing allows these traditionally hardware based components to be implemented into software instead. Hence, the software defined radio. This enables easy signal processing and thus cheap wide band scanner radios to be produced.""*




###What makes them special?

In case you're wondering what makes this possible, its the falling price of hardware. 

With the advent of USB 3.0 it is possible to get really high sampling rates that are usually needed to decode signals at high frequencies.

Powerful microcontrollers that run the board and program the FPGAs are pretty cheap.

Chips known as [FPGA](https://en.wikipedia.org/wiki/Field-programmable_gate_array)s and [CPLDs](https://en.wikipedia.org/wiki/Complex_programmable_logic_device) are basically integrated circuit breadboards that you can program to be whatever you want. These are the boards hardware manufactures use when they are desinging new integrated circuits and have traditionally been frightfully expensive. 


###What are the different boards out there?
 Specs         |RTL SDR        		| Hack RF      		| Blade RF     			|USRP (B100) 		|    
:-----------   |:-----------   		| :----------- 		| :----------- 			| :-----------		|
Price		   | 20$        		| 320$		  		| 420$		 			|~ 600$		 		|
Open Source	   | 	        		| Everything		|HDL + Code Schematics	| Host code		  	|
Radio Spectrum | ~ 50Mhz - 1.5 Ghz 	| 30 MHz – 6 Ghz 	| 300 MHz – 3.8 GHz 	| 50 MHz –2.2 GHz 	|
Bandwidth      | 2.8 Mhz(max stable)| 20 MHZ		  	| 28 MHZ 		 		| 16 MHz 			|
Duplex	       | Half (Rx only)     | Half		  		| Full	 		 		| Full				|
Sample Size	   | 8 - bit        	| 8-bit		  		| 12-bit 		 		| 12/14 12-bit 		|
Sample Rate	   | 2.4 MS/s        	| 20Ms/s		  	| 40 Msps 		 		| 64/128 Msps		|
Interface	   | USB 2.0        	| USB 2.0		  	| USB 3.0 		 		| USB 2.0 			|
Good For       | FM, NOAA Satellites | GSM, A/NTSC      | GSM,ATSC,NTSC,        | GSM             	|

The RTL-SDR is by far the cheapest board out there 






##Art Hack Day TV

For Art Hack Day Jonathan Dahan and I set up a small television broadcast station. We used the Hack RF and modified code we found online to broadcast a slide show of images from the Hack RF to UHF Channel 9

Code from that project is available [here](https://github.com/samatt/WAHD-TV.git)

![WAHD-TV](FullSizeRender.jpg =250x) 



##GSM With SDRs

Disclaimer: GSM is a licensed band and if you transmit and are caught transmitting the FCC will give you trouble. 

Having said that there are a lot of projects out there that allow you to use SDRs  to create a standalone GSM network.

The oldest and most well known is [openBTS](http://openbts.org/). 

Others include [YateBTS](http://www.yatebts.com/) and [OsmoBTS]( http://openbsc.../wiki/OsmoBTS)

For more information on the GSM aspect of SDRs I suggest looking through the materials for [Towers Of Power](https://itp.nyu.edu/classes/towers-spring2014/readings-resources/) a course taught at ITP by the amazing [Benedetta Piantella](http://www.benedetta.cc/bio.html) that aims to teach students about GSM networks. How they work, their politics and how to use them in projects. The link above has a lot of good material for anyone more interested in GSM.


I also wrote a couple of tutorials on setting up gsm networks using Yates and Osmo with the Blade Rf and USRP1 respectively. Linked below

[Setting up Blade RF with Yates BTS](https://github.com/samatt/ArtSec-SDR/blob/master/running_yate_bts_with_bladerf_on_ubuntu_14.04.md)

[Setting up USRP1 with Osmobts](https://github.com/samatt/ArtSec-SDR/blob/master/running_osmobts_with_usrp1_ubuntu_12.04.md)

I have **live images** for these if anyone is wants them let me know.	

##Resources

[SDR Tutorials](https://greatscottgadgets.com/sdr/) by Michael Ossmann 

[SDR Book](http://people.scs.carleton.ca/~barbeau/SDRBook/Book/) for theoretical fundamentals


##References
[Receiving HRPT Weather Satellite](http://www.rtl-sdr.com/hackrf-receiving-hrpt-weather-satellite-images/)

[High Altitude Balloons HAB ](http://www.rtl-sdr.com/hackrf-decoding-pico-high-altitude-balloons-hab/)

Jiao Xianjuns amazing [Git Hub repo](https://github.com/JiaoXianjun/)

Watch TV using the [RTL2832U dongle](https://github.com/kik/sdr-tv/wiki/RTL2832U%E3%83%89%E3%83%B3%E3%82%B0%E3%83%AB%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%83%86%E3%83%AC%E3%83%93%E3%82%92%E8%A6%8B%E3%82%8B%EF%BC%81)