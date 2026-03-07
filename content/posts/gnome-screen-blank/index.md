+++
title = "GNOME Power Daemon Bug Blacks Screen After 30 Seconds"
date = "2026-03-07T07:57:03-05:00"
author = "Braden"
cover = ""
tags = ["gnome", "opensuse"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
draft = false
+++

# Symptoms
As of March 2026 (time of writing), a bug has existed in GNOME for a few years that causes the display to go black after 30 seconds of inactivity (not suspended or locked, just goes dark). This behavior has been consistent when using OpenSUSE Leap 15.x and 16.x connected to an external display. When resuming from sleep, the screen will go black after just 30 seconds of inactivity regardless of the configured power settings. A system reboot resolves the issue until the next time the system resumes from sleep/suspend.

# Cause
After dealing with this for a long time and finding nothing about it other than forum posts with "yeah, I have this too" but no solution, I found the following issue in the GNOME settings-daemon GitLab that finally shed some light on the issue!

https://gitlab.gnome.org/GNOME/gnome-settings-daemon/-/issues/687

# Workaround
TL;DR: no fix has been pushed yet.
The workaround is to restart gsd-power with systemctl.

You can either run the following command whenever the issue occurs, or setup a systemd service that restarts gsd-power after suspend (see GNOME issue above for more information).

```bash
systemctl restart --user org.gnome.SettingsDaemon.Power.target
```

After running the above command, my system screen stopped going black after 30 seconds. No more coworkers saying "What did you do to your computer...that's annoying."
