---
layout: post
title: "Zoom G1X MIDI Commands"
date: 2024-05-16
image: '/images/thumbs/thumb_Zoom-Midi-Config.png'
description:
category: ''
tags:
twitter_text:
introduction: An unofficial guide!
published: true
---

**This page will serve as an archive for all the different MIDI commands that I have discovered so far that works with the ZOOM G1X Four.**
 
I will add more details and explanation as I find time. 

**Bank Selection**
Switch to Bank 1:
````bash
CC 0 0 CC 32 0
````

Switch to Bank 2:
````bash
CC 0 0 CC 32 1
````

Switch to Bank 3:
````bash
CC 0 0 CC 32 2
````

Switch to Bank 4:
````bash
CC 0 0 CC 32 3
````

Switch to Bank 5:
````bash
CC 0 0 CC 32 4
````

**Patch Selection with the Selected Bank**
Switch to Patch 0
````bash
PC 0 0 
````

Switch to Patch 1
````bash
PC 1 0
````

Switch to Patch 2
````bash
PC 2 0
````

Switch to Patch 3
````bash
PC 3 0
````

Switch to Patch 4
````bash
PC 4 0
````

Switch to Patch 5
````bash
PC 5 0
````

Switch to Patch 6
````bash
PC 6 0
````

Switch to Patch 7
````bash
PC 7 0
````

Switch to Patch 8
````bash
PC 8 0
````

Switch to Patch 9
````bash
PC 9 0
````

**Effect Slot Toggle**
Toggle Slot 1
````bash
f0 52 00 6e 64 03 00 0a 01 00 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````

Toggle Slot 2
````bash
f0 52 00 6e 64 03 00 0a 01 01 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````

Toggle Slot 3
````bash
f0 52 00 6e 64 03 00 0a 01 02 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````

Toggle Slot 4
````bash
f0 52 00 6e 64 03 00 0a 01 03 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````

Toggle Slot 5
````bash
f0 52 00 6e 64 03 00 0a 01 04 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````

Toggle Slot 6
````
f0 52 00 6e 64 03 00 0a 01 05 00 00 00 00 f7 f0 52 00 6e 64 03 00 0a 03 01 00 00 00 00 f7
````
