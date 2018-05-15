---
title: pihpsdr for the Charly25 SDR
---

{:code-c: .language-c .highlight}

# {{ page.title }}

## General description

G0ORX's [pihpsdr](https://github.com/g0orx/pihpsdr) is a HPSDR client intended for small touchscreens connected to a Raspberry Pi or similar. While it does not (yet) support all features that the hardware supports (missing Diversity reception and Predistortion), it still supports all the features one would usually expect from a software-defined radio, partly thanks to the [WDSP](https://github.com/g0orx/wdsp) library, which is also used by PowerSDR.

In addition to touchscreen controls, pihpsdr also offers support for buttons and rotary encoders, which can be connected via GPIO pins. An example is the Apache Labs [PiHPSDR Controller](https://apache-labs.com/al-products/1043/PiHPSDR-Controller.html), featuring a collection of buttons, rotary encoders, a Raspberry Pi 3 as well as a touchscreen.

## Charly25 adjustments

All C25 hardware features are now supported by pihpsdr by default—although we recommend compiling it yourself, since the binaries in the repository might not always be up to date. The required steps to do so for a Debian-based system can be found in the repository under `release/documentation/pihpsdr-build.pdf`.

## Desktop usage

While the interface was designed for screens with a resolution of 800×480 pixels, it is possible to use pihpsdr on a Desktop PC or Laptop with a bigger screen. This requires a few changes in `main.c`, starting at around line 181: 

* Removing the `display_width=800;`{:code-c} and `display_height=480;`{:code-c} lines enables the window to take an arbitrary size.
* Removing `full_screen=0;`{:code-c} causes the pihpsdr to go into fullscreen mode, even for screen wider than 800 or taller than 480 pixels.
* Removing `gtk_window_set_resizable(GTK_WINDOW(top_window), FALSE);`{:code-c} a few lines further down makes the window resizable.
* If you end up with a bright/white font on a white background, comment all out lines that force a background colour, for example by running `sed -i 's!gtk_widget_override_background_color!//gtk_widget_override_background_color!' *.c` on the command line in the pihpsdr directory.

## STEMLab discovery

By uncommenting the `STEMLAB_DISCOVERY=STEMLAB_DISCOVERY` option in `Makefile`, pihpsdr can be built with support for discovering RedPitaya/STEMlab devices. This means that figuring out its IP address and opening the web interface is no longer necessary in order to use it as an SDR with pihpsdr. This feature requires additional software, which might come standard with your Linux distribution: Avahi and cURL. To install those on Debian-based systems, you can run `sudo apt-get install avahi curl`.
