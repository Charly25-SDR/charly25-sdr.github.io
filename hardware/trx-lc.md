---
title: Low-Cost 10W Trx board
---

# {{page.title}}

Charly 25 LC (for low cost) was drafted and prototyped and the idea was to create an easy path for newcomers to get into the 4 Ham Bands allowed  for Class E in Germany.

Hence we built a first board with the following ideas in mind:
* As non destroyable as possible, allowing for experimentation without damaging anything
  + Over voltage protected
  + Reverse polarity protected
  + Immune to high SWR, short and open lines
* Simple and easy to build
* 10W on 4 Bands
* Sensitive receiver with
  + 2 pre-amplifiers
  + Switchable attenuators 12dB/24dB

RP delivers about 9dBm of output power on the TX output (more like 6dBm on 6m) so in order to achieve 10W of output we would need roughly 30dB of power gain.

We decided to test a Mosfet designed for VHF use to do the task, this would save a lot of space on the board. The only drawback was on fixing and cooling – this small Transistor is designed to be soldered on a thermally conductive PCB, something not feasible for us in a simple and cost efficient way.

We decided to use a copper block 40x50x10mm as heat spreader and press the transistor on it. It turned out that this method works pretty well. It also makes it very easy to change the transistor. 

![Copper block heat spreader](/assets/img/hardware/lc-heatspreader.png)\\
The heat spreader….

The idea was then tested in a little amplifier originally supposed to be 5W

![5W PA](/assets/img/hardware/lc-5w-pa.jpg)

The final board including the amplifier above would fit into a Schubert case of 100x160x30mm

![Empty board in casing](/assets/img/hardware/lc-board-in-case.jpg)

The final prototype then looked like this:

![Final LC Prototype](/assets/img/hardware/lc-final-prototype.jpg)

And here are the schematics to give you an idea of what we did, it covers 10,20,40,80m:

[![LC board schematic](/assets/img/hardware/lc-schematic.png)](/assets/img/hardware/lc-schematic.png)

If you want to build your own version of this device, feel free to do so, a Target 3001 file is available [here](/assets/download/charly25-lc.t3001c).

However, be aware of restrictions please:

**As this is an early prototype it might still contain some bugs so this is for the experienced Experimenter only!\\
We will not provide any support on it!\\
Any commercial reproduction, even partial is prohibited without prior written permission!**