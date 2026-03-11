+++
title = "Proxying Immich"
date = "2026-03-10T23:13:50-04:00"
author = "Braden"
cover = ""
tags = ["immich", "pangolin", "homelab", "reverse proxy"]
keywords = ["immich", "pangolin", "homelab", "reverse proxy"]
description = "Using Pangolin as a reverse proxy to access Immich from outside a private network."
showFullContent = false
readingTime = true
hideComments = false
draft = false
+++

One of my recent projects was setting up Immich for a local FIRST Robotics team. For the past 10 years, they have used 2 Western Digital USB 3.0 HDDs to save all photos and videos for their business subteam. While I was shocked the 2 USB HDDs had survived 10 years of teenagers (nice job WD), it was time for something a bit newer.

Enter [Immich](https://immich.app), a "self-hosted photo and video management solution". I have used Immich personally for about a year and it felt like a great fit. Immich also went stable on October 1st, 2025 which made me feel much more comfortable using it in production. 

One problem I needed to solve with Immich was public access. The robotics team was using various Google Drive shared folders to allow photo upload from parents and whatnot after events, but that meant it was common for the photo folders on the USB HDDs and Google Drive to fall out of sync (a manual process). I wanted Immich to be the source of truth, no more Google Drive or other places that we needed to import pictures from.

# Solution 1 - Cloudflare Tunnel

The first option I tried was [Cloudflare Tunnel](https://developers.cloudflare.com/tunnel/). This setup just required a DNS zone at Cloudflare and the lightweight *cloudflared* daemon running on the local network to connect Cloudflare to Immich. The setup took about 15 minutes and everything worked like a charm. The only problem was the 100MB file limit and heavy rate limiting by Cloudflare. Cloudflare Tunnels are not supposed to be used to stream videos, so the bursty yet heavy media upload/download traffic of Immich was pushing the limits of the TOS.

While the Cloudflare Tunnel worked fine for a few months, we started running into issues during competition season when more parents were trying to upload content from their mobile phones with spotty cellular coverage. The uploads could take well over 10 minutes and Cloudflare was killing off the long running HTTP connections before the uploads could finish. I tried tweaking various settings but was not able to fix the timeout issue. Bandwidth was also restricted, and even with a decent connection, uploads were very slow and video uploads would usually time out.

# Solution 2 - Pangolin

Enter [Pangolin](https://pangolin.net), an open source "identity-aware VPN and proxy for remote access to anything, anywhere." While self-hosting on a VPS is an option, their very generous [Pangolin Cloud Basic Free Tier](https://pangolin.net/pricing) did everything I needed. A few sites said it has a 25GB per month bandwidth limit, but the pricing page mentioned nothing about it. Either way, 25GB was more than enough for my needs.

Within 15 minutes, I had a *fosrl/newt* site connector running and Immich proxied through Pangolin Cloud. I won't write my exact steps here as they will likely change. Pangolin is a relatively new offering in the tunnel/proxy world. The basic steps were:

1. Install a Site Connector (running in Docker for me)
2. Setup Domains (CNAME records pointing to Pangolin Cloud)
3. Create a Public Resource linking a FQDN to Immich through the site connector (http://myimmichapp:2283)
4. Setup Resource Rules to block by country (not a bulletproof security measure, but no reason anyone outside the US needs to be accessing my Immich)

# The Results

So far, I have not run into bandwidth issues and I was able to upload a 50MB video file on a spotty cellular connection through my phone's web browser. The upload took about 10 minutes and never timed out. Even larger videos (100+ MB) on WiFi upload in seconds. A significant improvement from Solution 1. Granted, it is well known in the Immich community that Cloudflare allows, but significantly restricts, media traffic over Cloudflare Tunnels. For what I need, Pangolin does the job and the free tier appears to be all I need. If I do ever need to upgrade, Pangolin [partners with RackNerd](https://docs.pangolin.net/self-host/choosing-a-vps) and offers hosting and 2TB of monthly bandwidth for less than a $1 a month.
