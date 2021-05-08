---
layout: default
title: pi case 40
permalink: /pi-case-40
---

# Enable Cooler Master Pi Case 40 shutdown button without using Pi-Tool

Cooler Master's [Pi Case 40](https://www.coolermaster.com/catalog/cases/raspberry-pi/pi-case-40/) is a relatively nice case for
the Raspberry Pi 4. It would've been nicer if they hadn't made it impossible to use HATs by swapping the GPIO header rows &
misleading backers, stating the pinout would be fixed during the campaign & changing their tune after funding closed, but it's
pretty nice otherwise.

It has a built-in GPIO soft power button, for which they have a GUI tool available [on GitHub](https://github.com/CoolerMasterTechnology/Pi-Tool)
that is pretty horrifically bloated. It lets you set actions to trigger from a few different press events, apply some pretty
mediocre preconfigured overclocking profiles, and monitor frequency/temperature. 

It also happens to take up almost **half a gigabyte** of disk space, a ludicrous amount of CPU/RAM, and button triggers only work when it's actively running in the GUI. This makes the shutdown button of questionable use, since you can't use it on a headless Pi without installing the GUI and connecting over VNC.

Fortunately, Pi Case 40's shutdown button is connected to the standard Pi shutdown pin, GPIO3, so we can skip their tool
entirely by adding a single line to `/boot/config.txt`:

```
# Enable shutdown button on GPIO3
dtoverlay=gpio-shutdown
```

Yeah, yeah, it's two lines with the comment. Details. 

If you'd like to tweak settings, here's the relevant section of [`/boot/overlays/README`](https://github.com/raspberrypi/firmware/master/boot/overlays/README) at [line 1015](https://github.com/raspberrypi/firmware/blob/d9c382e0f3a546e9da153673dce5dd4ba1200994/boot/overlays/README#L1015) (as of this writing):

```
Name:   gpio-shutdown
Info:   Initiates a shutdown when GPIO pin changes. The given GPIO pin
        is configured as an input key that generates KEY_POWER events.

        This event is handled by systemd-logind by initiating a
        shutdown. Systemd versions older than 225 need an udev rule
        enable listening to the input device:

                ACTION!="REMOVE", SUBSYSTEM=="input", KERNEL=="event*", \
                        SUBSYSTEMS=="platform", DRIVERS=="gpio-keys", \
                        ATTRS{keys}=="116", TAG+="power-switch"

        Alternatively this event can be handled also on systems without
        systemd, just by traditional SysV init daemon. KEY_POWER event
        (keycode 116) needs to be mapped to KeyboardSignal on console
        and then kb::kbrequest inittab action which is triggered by
        KeyboardSignal from console can be configured to issue system
        shutdown. Steps for this configuration are:

            Add following lines to the /etc/console-setup/remap.inc file:

                # Key Power as special keypress
                keycode 116 = KeyboardSignal

            Then add following lines to /etc/inittab file:

                # Action on special keypress (Key Power)
                kb::kbrequest:/sbin/shutdown -t1 -a -h -P now

            And finally reload configuration by calling following commands:

                # dpkg-reconfigure console-setup
                # service console-setup reload
                # init q

        This overlay only handles shutdown. After shutdown, the system
        can be powered up again by driving GPIO3 low. The default
        configuration uses GPIO3 with a pullup, so if you connect a
        button between GPIO3 and GND (pin 5 and 6 on the 40-pin header),
        you get a shutdown and power-up button. Please note that
        Raspberry Pi 1 Model B rev 1 uses GPIO1 instead of GPIO3.
Load:   dtoverlay=gpio-shutdown,<param>=<val>
Params: gpio_pin                GPIO pin to trigger on (default 3)
                                For Raspberry Pi 1 Model B rev 1 set this
                                explicitly to value 1, e.g.:

                                    dtoverlay=gpio-shutdown,gpio_pin=1

        active_low              When this is 1 (active low), a falling
                                edge generates a key down event and a
                                rising edge generates a key up event.
                                When this is 0 (active high), this is
                                reversed. The default is 1 (active low).

        gpio_pull               Desired pull-up/down state (off, down, up)
                                Default is "up".

                                Note that the default pin (GPIO3) has an
                                external pullup. Same applies for GPIO1
                                on Raspberry Pi 1 Model B rev 1.

        debounce                Specify the debounce interval in milliseconds
                                (default 100)

```