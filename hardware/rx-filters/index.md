---
title: Rx Filter Board
---

# {{page.title}}

We have developed a prototype of a pre-selector using 12 independent filters controlled by Power SDR.

This Pre-Selector board was demonstrated in Erwins Transceiver on our booth at the HAM Radio.

It is not clear currently if/when a commercial version will be available. The problem is that high performance filters like the ones used in the prototype require an enormous amount of tweaking during production. This would make the finished product pretty expensive.

For those of you who want to build one yourself, enclosed please find the standard schematics.

I have also created an Excel sheet to calculate them; this spreadsheet is available [here](/assets/download/filter-calculator.xls). A web-based version is also available [here](calculator). The ones I tested and downloaded from the internet were faulty and I wasted long weeks of prototype building to find out. You can either do a Chebyshev or a Butterworth Design using the Spreadsheet.

[![Butterworth Filter Example](/assets/img/hardware/rx-butterworth-filter-example.png)](/assets/img/hardware/rx-butterworth-filter-example.png)

This might give you an idea how the finished filter bank looks like:

![Rx Filter Board with Filters](/assets/img/hardware/rx-filters.jpg)