---
title: Allband 20W Trx board
---

# {{page.title}}

So we have started to work on these new functionalities.

People who have visited us at the booth on Ham Radio or joined us during the presentation in the SDR Academy, know that we have started to work on some of the requests.

## Pre-Distortion

Will be included in our next design.

We will build in a coupler to provide
* -32dB RF forward
* -32dB RF reverse
* A DC output for Power forward
* A DC output for Power reverse

The DC outputs will be used to integrate a power and SWR measurement functionality in Power SDR

## Diversity

Red Pitaya has built a special version of Red Pitaya for us. This is, all Analog Frontend HW is taken out, and both ADC and DAC are Transformer coupled. 

I have demonstrated the results of this change during my presentation at the SDR Academy.

It results in massively improved x-talk between channel 1 and Channel 2 (around 85dB now)

The 2nd RX is fully usable now including Diversity.

## More Power

We will deliver 20W in future using a Push Pull design.

## Usage of 2nd and 3rd Nyquist Zone

This is not part of our current design work. This idea will be parked for a while but the aim is to use RP on 70MHz, 145MHz and 220MHz. Will be picked up again once all the other work is done.

Some measurements done on this subject look pretty promising especially as Pavel has done some magic again and the signal is cleaner than ever …