---
title: Quisk for the Charly25 SDR
---

# {{page.title}}

[QUISK](http://james.ahlstrom.name/quisk/) by James Ahlstrom, N2ADR, isn't only an SDR application, but essentially a framework for building your own. As opposed to almost all other HPSDR-compatible SDR applications, it ships its own DSP code instead of using WDSP. For this reason, it is uncertain if/when some "advanced" SDR features like predistortion or diversity reception will come to QUISK, as the focus appears to be on diverse hardware support instead.

Nevertheless, some small changes have been made to the existing files used for HPSDR support, allowing to control the C25 attenuator and preamplifier. Using [those files](https://github.com/markusgrosser/c25-quisk) instead of the provided `quisk_hardware.py` and `quisk_widgets.py` in the `hermes` directory suffices to get the appropriate sliders into QUISK. Other HPSDR displays like e.g. PA temperature have been left in and currently display rather random numbers, leaving the option open for future changes to the hardware or RedPitaya software to support this functionality.