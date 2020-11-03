---
title: Charly 25 RADIOlab
---

# {{page.title}}

Our Charly 25 RADIOlab is meant to be built by the experienced Home Brewer using our C25 modules and mechanical kits. However, completely pre-configured versions are available as well.

The following pptions are available:
- Chassis/mechanical kit with all plugs and loudspeaker etc installed (all modules to be ordered separately&mdash;wiring done by the end user)
- C25 RADIOlab including: C25 Mainboard, C25 Codec, C25 Power Distribution Board (C25 preselector optional) and Red Pitaya STEMlab 16&mdash;fully preconfigured an cabled
- C25 RADIOlab as above but with additional Win10 PC built in (Intel Core i3/i5 with 8GB of Memory and 480GB SSD&mdash;bigger SSD/2xSSD is available on request)

# Technical Specs

## Rx

- 2x Rx channels with 55MHz low pass receiving range 50KHz to 55MHz
- 2x preamplifier (each 17dB +/-1dB) OIP3 about 43dBm (each Rx channel has 2x preamplifier)
- 2x attenuator 12dB/24dB (each channel has 12dB/24dB attenuators)
- Sensitivity about -139dBm / 500Hz bandwidth per channel
- Diversity reception with automatic optimization in Power SDR (plus Thetis in beta)
- All switching done with high performance signal relays to avoid IMD issues common in many semiconductor switches (especially at lower frequencies)
- Software controlled attenuation +/-36dB in 6dB steps
- S-meter calibrated via PowerSDR or Thetis

## Tx

- Output 20W from 160m to 6m (limited output at 600m possible but not guaranteed)
- Push-pull operation, quiescent current approx. 300mA each Transistor
- Harmonic suppression in line with commercial requirements (> -50dBc on HF and > -60dBc on 6m)
- Robust VHF transistor also withstands bad SWR (no problems with short/open etc.)
- An overcurrent protection circuit monitors the current of the PA
- Integrated coupler for Predistortion
- Software controlled attenuator (0 to 31dB in 1dB steps) for automatic predistortion setup

## Power

- 13,8V/8A max depending on hardware configuration
- Overvoltage protection
- Overcurrent protection
- Reverse polarity protection
- Low voltage shut down to avoid battery damage in case of battery operation
- 5V/5A DC/DC converter integrated

# Overview of Modules, Preconfigured RADIOlab & Pricing:

## STEMlab 16

STEMlab 16 from Red Pitaya is at the core of our Trx: https://www.redpitaya.com/Catalog/p52/sdrlab-122-16-standard-kit?cat=c99

It has been renamed by Red Pitaya to SDRlab.

Technical data from an amateurs point of view:
- MDS on the HF Bands -122dBm @ 500Hz Bandwidth
- Phase Noise is given as -155dBc @ 10KHZ – I have measured it with better then -150dBc (was limited by my equipment)
- Because both the ADC and the DAC are transformer coupled with no Nyquist filter you can use higher order Nyquist Zones
- At 145MHz MDS is -120dBm@500Hz Bandwidth
- You can view our presentation (in English) on this subject here: https://www.youtube.com/watch?v=PI_ROLXqO_Q
- it is in English
- Price is 499,-€ excluding VAT

## C25 Mainboard

To get as much out of the STEMlab as possible we developed a board which we call C25 mainboard:
- please see the specs above – it provides everything to make STEMlab a full blown Trx
- including predistortion (automated via software)
- including diversity Rx (automated vie software)
- Connection for additional VHF/UHF frontends
- Power control functions for the rest of the Trx
- board can be jumpered to either support STEMlab 14 or STEMlab 16
- Price is 738,70€ excluding VAT + 22,69€ for the copper heat spreader

## C25 Codec

There are cheap Chinese codecs available on the internet, however we were not satisfied with their performance and we did have additional requirements like:
- reduction of noise
- 2x 7W Audio Amplifier
- additional I2C buffer because STEMlab is sensitive to massive I2C loads
- please see spec sheet above
- Price is 80,00€ excluding VAT (new version is slightly modified against the one used so far – we implemented new cable connections for easier cabling of a complete device)

## Power Distribution Board

- is used for easier power distribution within the Trx and provides decoupling of all modules
- in addition it provides 8V,6V,5V,3,3V switchable to support an electret microphone
- is not priced yet as we have only sold it with complete Trx – will probably be around 35,00€ excluding VAT

## C25 Rx Preselector

We have developed a very powerful preselector with 11 Amateur Bands (+ passthrough for broadcast listening)
- Far off attenuation has been improved to around 100 dB – we don’t think any other device on the Market is matching these specs
- Price: it comes in 2 Versions:
    1. pre populated with all SMD parts (among them are 1% tolerated NP0 C’s) the user has to add the coils, the relays etc and do the final adjustment; Price is 263,90€ excluding VAT
    2. fully populated, adjusted and tested; Price is 502,50€ excluding VAT

## Chassis / Mechanical Kit

You can find some pictures here: https://smartradioconcepts.com/p/charly-25-radiolab-mechanik-satz-komplett
- it comes with all plugs, distance rolls etc. but no cables

Price is 442,00€ excluding VAT

## Cabling

We use RG316/SMA cabling for all cables inside our Trx – cables are available in the shop in length of 10cm, 20cm, 30cm, 40cm and 50cm.

Currently power cables, audio cables and I2C cables are to be built by the builder&mdash;a complete cable kit offer might be available in future.

Multi media cabling will also be available (2x USB + 2x HDMI specially built to our specs).

## PC Board

People who want a PC built in, can use our 100mm x 100mm PC Board. Currently we are using an Intel core i3 CPU with Windows 10.

You can find some details here including a handbook for download:

https://smartradioconcepts.com/c/charly-25-boards-and-zubehoer/pc-board-mit-core-i3-fuer-w10-oder-linux

Price is 300,-€ excluding VAT and excluding Memory & SSD (we will charge day prices for memories and SSD as these are fluctuating very much)

In future we will move to Intel core i5 because of higher CPU demands from Thetis. Price not fixed yet but will probably in the range of 380,-€ excluding VAT..

## Complete Devices (Preconfigured Trx Kits)

We call it RADIOlab because STEMlab is still accessible from outside so that you can still use all apps like signal generator, VNA etc.

### Minimum Version includes

- Mechanical kit/case
- STEMlab 16
- C25 mainboard
- C25 Codec
- C25 Power Distribution Board
- Cabling completed
- Device tested for functionality
- We will unplug STEMlab 16 to make it a kit; user will have to put plugs back (colour coded so no big deal)

Price 2113,-€ excluding VAT

### Standard Version

Includes all of above plus Preselector

Price: 2628,50€ excluding VAT

### Standalone Trx with built in PC

Includes all of above plus

- PC with Intel core i3 and Windows 10, 8GB Memory, 480GB SSD
- Multi media cabling (2× USB + 2× HDMI)

Price 3096,82€ excluding VAT

You can find a lot of pictures here&mdash;done by the guy who has written the article (at the end of the article): https://saure.org/cq-nrw/2020/08/23/18989/

### Standalone Trx with PC and Screen

And finally our latest device includes all of above plus:

- 7" screen 1280×800
- 12 free configurable push buttons (configuration done in PowerSDR or in Thetis)
- 4 free configurable Rotary buttons with push button
- One optical encoder for VFO
- Socket for microphone, key, headphone
- 4x SMA to connect to the STEMlab

Price is not fixed yet; we are working on some final aspects of it but I guess it will be in the range of 3450,-€ to 3550,-€ when finished

[![Picture of RADIOlab with screen and PC](/assets/img/full-transceivers/radiolab.jpg)](/assets/img/full-transceivers/radiolab.jpg)

We will also sell the front panel with display as a complete module – so anyone who has purchase a RADIOlab before can upgrade later on to make it a complete Trx.

# Software is Free and Open Source

- the firmware for STEMlab 16 is based on work from Pavel Demin, we have modified it to support our hardware (everything controlled by I2C)
- User interface is either PowerSDR or Thetis
- Both versions are modified by us to include extra features and meet our needs
