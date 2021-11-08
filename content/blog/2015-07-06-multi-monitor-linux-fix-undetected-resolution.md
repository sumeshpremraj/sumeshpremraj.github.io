---
title:  "Fixing undetected resolution in a multi-monitor Linux setup"
date:   2015-07-06 06:40:30 +0530
tags: ["linux"]
---
After upgrading to Fedora, I was unable to run my Dell laptop on a dock connected to two monitors properly. One of the screens would refuse to run at its maximum resolution.

It took me a while, but adding the resolution manually with xrandr and playing around with X configuration files fixed the issue.

### Adding new resolution with xrandr
Run <em>xrandr</em> to identify the screens by name. Note these names, they'll be used in X conf files and for xrandr commands later. In my case, it was DP1, DP1-3 and DP1-2, with the latter not running in native resolution (1920x1080).

Run gtf with the required resolution and refresh rate (1920 x 1080 and 60 Hz here) to generate the <strong>Modeline</strong>.

```bash
$ gtf 1920 1080 60
# 1920x1080 @ 60.00 Hz (GTF) hsync: 67.08 kHz; pclk: 172.80 MHz
  Modeline "1920x1080_60.00"  172.80  1920 2040 2248 2576  1080 1081 1084 1118  -HSync +Vsync
```

Use xrandr to add a new mode like so (named 1080p here, can be anything you want). You have to copy the line starting with Modeline, but omit the word Modeline itself:

```bash
# Create new mode
$ xrandr --newmode "1080p" 172.80  1920 2040 2248 2576  1080 1081 1084 1118  -HSync +Vsync
# Add this mode to our screen
$ xrandr --addmode DP1-2 1080p
# Change the screen to this mode
$ xrandr --output DP1-2 --mode 1080p
```

Once you've tested this, it is time to add this to your startup so that you don't have to do this manually.

```bash
$ cat ~/.xprofile
# Create new mode
xrandr --newmode "1080p" 172.80  1920 2040 2248 2576  1080 1081 1084 1118  -HSync +Vsync
# Add this mode to our screen
xrandr --addmode DP1-2 1080p
# Change the screen to this mode
xrandr --output DP1-2 --mode 1080p
```

### X configuration
X usually works out of the box, but in this case, the native resolution is not being detected. The screens themselves support 1920x1080 resolution, which was tested with xrandr above, so we now specify the resolution explicitly to enable the higher resolution.

Specify your graphics chip and driver.

```bash
$ cat /etc/X11/xorg.conf.d/30-graphic.conf
Section "Device"
    Identifier      "Intel Integrated"
    Driver          "intel"
EndSection
```

Specify the name and Modeline (generated above with <strong>gtf</strong> above) for the undetected screen.

```bash
$ cat /etc/X11/xorg.conf.d/40-monitor.conf
Section "Monitor"
  Identifier  "DP1-2"
  Modeline "1920x1080_60.00"  172.80  1920 2040 2248 2576  1080 1081 1084 1118  -HSync +Vsync
  Option "PreferredMode" "1920x1080_60.00"
EndSection
```

Specify the preferred resolution for the screen.

```bash
$ cat /etc/X11/xorg.conf.d/50-screen.conf
Section "Screen"
    Identifier "DP1-2"
    DefaultDepth 24
    SubSection "Display"
            Depth 24
            Modes "1280x1024"  "1024x768"   "1920x1080"
    EndSubSection
EndSection
```

Restart X. I use GNOME 3 and GDM login manager, so I had to do this:

```bash
$ sudo service gdm restart
```

Restart your login manager if you know it, or alternatively you can restart your system.
