+++
title = "GNOME Power Deaemon Bug Blacks Screen after 30 Seconds"
date = "2026-03-07T07:57:03-05:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = ""
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
As of March 2026 (time of writing), a bug has existed in GNOME for a few years that causes the display to go black after 30 seconds of inactivity. This behavior has been consistent for me when using OpenSUSE Leap 15.x and 16.x when connected to an external display and the laptop goes to sleep. When resuming from sleep, the screen will go black after just 30 seconds of inactivity regardless of whatever power settings are configured.

# Issue
https://gitlab.gnome.org/GNOME/gnome-settings-daemon/-/issues/687

# Workaround
```bash
systemctl restart --user org.gnome.SettingsDaemon.Power.target
```
