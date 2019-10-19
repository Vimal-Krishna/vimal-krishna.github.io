---
layout: post
title: "Intel Wireless Adapter 9650 Issue and Solution"
date: 2019-10-19
image: '/images/thumbs/thumb_intel-wireless-adapter-issue-and-solution.png'
description:
category: 'blog'
tags:
twitter_text:
introduction: Streaming Problems
published: true
---

This is a description of an issue that I faced on my **Acer Nitro 5** laptop, and how I solved it.

My laptop had developed an issue while **streaming videos** on the browser - Prime, Netflix, Vimeo etc.
The video would stop to buffer very frequently even on the lowest quality settings, sometimes every minute.

It was interesting that I had not faced the same issue on my work laptop, or my phone or my home desktop.

In order to isolate the issue, I tried a couple of tests. The important test was the use of a USB WLAN . I borrowed the USB WLAN from my home desktop, configured it on my Acer Nitro, and disabled the built-in Wireless Adapter.
This instantly solved the problem!

While I wondered whether my Acer laptop had a hardware issue, I though it may have been a driver update that caused the problem because the issue wasn't seen when the laptop was new.

I did find an updated driver for **Intel Wireless-AC 9650** that fixed the issue.

The version that had the issue was: **21.0.0.5**.<br/>
The version that fixed the issue is: **21.40.2.2**.

![_config.yml]({{ site.baseurl }}/images/Intel-Driver-Updates-Page.png)