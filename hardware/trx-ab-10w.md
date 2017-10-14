---
title: Allband 10W Trx board
---

# {{page.title}}

Some people in our club build this first version and it turned out to be pretty useful. 

However as you might guess people came up with additional wishes:
* All band TX capability 
* A pre-selector

And of course as we moved to an all SMD design not everyone was happy to do the soldering by himself and people asked us for finished boards, something which was completely out of range for us (but we started to search for a solution).

Okay, so we added the all band capability by adding more filters, 6m turned out to be a problem, our one transistor design did not provide enough gain to get us from about 6dBm from the Red Pitaya to 40dBm of output.

In addition it became clear that we would need more filtering for 6m to get some spurs and the alias frequencies under control. This all required extra gain and hence we added a driver to the board.

The input matching of the transistor also became a problem and was improved for 6m.

No other changes were made so the all band board is more or less the same design as the original C25LC board. Of course the SW had to be adopted for the additional bands.

And as we had to add more tracks - the design was not possible on double sided PCB material anymore we had to move to a 4layer PCB design. 

In addition the 4 layer design and some smaller SMD parts make it less attractive to build this board by hand now – but in  the meantime we have solved the production problem by teaming up with Red Pitaya and the boards are now available at the RP shop and from CQDL. 

## More wishes …

Of course the wish list never ends, Power SDR and RP provides us with wonderful features which we would like to include in our design.

This is:
* Pre-Distortion – requires an output coupler on the board
* Diversity for RX – requires improved x-talk values on the RP receiver inputs
* A bit more output to drive older PA’s
* Usage of 2nd and 3rd Nyquist Zone?