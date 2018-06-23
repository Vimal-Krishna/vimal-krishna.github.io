---
title: Something in Win10 is eating my RAM
date: 2018-02-26 15:26:00 +0530
layout: post
image: '/images/thumbs/thumb_Win10-RAM-Usage-2018-02-26-17_52_24.png'
category: 'blog'
introduction: When 16GB of RAM is not enough!
description: Something's eating my RAM in Windows 10.
published: true
---

This is **not** a solution. It's only a documentation of the problem I am currently facing.

I discovered that the RAM usage on my Windows 10 Thinkpad laptop has been constantly maxed out. It starts off at around 50%-60% usage and as the OS continues to remain powered on, it reaches 99% in about a day or two, even with almost no applications running. 16GB of RAM is a lot and it's not fun when you don't get to use it.

I discoverd this only because the cooling fan was constantly running at high speed, and sometimes resource hungry applications like Visual Studio would take a long time to load.

I did figure out what was causing the RAM usage though. It is a certain driver that has been installed on my office laptop. 
As seen in the screenshot, under the **'driver locked'** category of RAM usage, there is a whopping **12GB of RAM!**

![_config.yml]({{ site.baseurl }}/images/Win10-RAM-Usage-2018-02-26-17_52_24.png)

There was an article which explained how to determine which exact driver is causing this problem. Unfortunately, the diagnostic tool crashed during the memory test. There's nothing more that I can do now, since this is probably a customized version of Windows 10 that is being used by the IT department.

For now, I reboot the OS every second day and I have automated the programs that need to be launched on each boot. Luckily chrome and firefox can be configured to save their states. Although annoying to do this, it works for me currently.
