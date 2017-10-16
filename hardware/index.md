---
title: A bit of history on our HW developments
---

# {{page.title}}

When we started our discussion about our project back in 2014, we didn’t know much about SDR and of course nothing about Red Pitaya.

The first idea was to build something very simple, may be a 1Band class E Amplifier for CW on 20m and a simple receiver based on an up converter for an RTL Stick. 

## Many projects start like this…..

Over time our wishes grew, the receiver should be more sophisticated and of course we would like to transmit on more bands, may be a transmitter for class E the beginner’s license in Germany.

Starting with Analog Devices and Linear Technology I/Q Chips, we built our first prototype using LT5506 and ADL5368 for signal processing. A lot of time was spent in developing a 48MHz Ceramic filter with 100KHz bandwidth.

Finally the prototype was working from almost DC to about 1GHz.

[![C25 IQ Mixer Proof of Concept](/assets/img/hardware/iq-mixer-poc.png)](/assets/img/hardware/iq-mixer-poc.png)

However as with other projects a lot of issues popped up, production cost was higher than expected, performance was not as good as we had wished. Some more work would need to be done, an alternative chip was selected for RX (IP3 was not good enough) and so on …

## Then Red Pitaya came along …

Or better to say, Red Pitaya was already there but Pavel Demin the SDR Genius came up with his idea of using Red Pitaya for SDR this was around June or July 2015 if I remember right.

First Pavel developed a RX SW and I was so favourite to receive one of his first releases.

So I did some 2-tone measurements on the performance of his SW on RP and the result… a big disappointment - endless amounts of spurs and glitches, the SW was simply not usable.

I shared my results with Pavel, a couple of mails back and force and a very constructive discussion followed.

It was only about a week after which Pavel came up with a solution and the route course of the problem – synchronization issues between the ADC Clock and the FPGA clock.

After the problem was solved Red Pitaya became the beautiful little receiver that many people love.  

And now of course the wish came up to make it a TX as well – a little later this dream became true as well and the rest is history.

A lot of people jumped on the idea and made Red Pitaya and Pavels Software a popular basis of an Amateur TRX. 

## Charly 25

When we successfully tested Red Pitaya as TRX for the first time around August 2015 we were pretty impressed by the performance and the cost/performance ratio of this device.  

It had its drawbacks of course (high impedance input for example) but meanwhile the price had dropped considerable and it became very attractive.

So we skipped our idea of building our own SDR TRX board and continued with Red Pitaya as our main core SDR (the idea of using IQ chips is not completely dead as we still dream about building a fully portable, handheld TRX. This might bring us back to the small QFN chips from Analog and LT one day… power consumption is one of the drawbacks of the RP concept….).  

One of the question to be clarified was the controllability of the HW that we would build. Some people using RP have followed the Hermes path and tried to simulate the Hermes functionality, including the usage of I/O Pins on the Red Pitaya under control of the Hermes SW.

We looked at this approach and also it was an easy path forward, it did not give us the flexibility we would like to see in our future designs.

In addition, we always thought that the HW to be build would have to be useful in a traditional way as well using a microprocessor control and any HF processing unit connected to it.

We did not want to lock ourselves into direct sampling SDR as the only option.

This is the reason why we decided to control all of our HW using I2C.