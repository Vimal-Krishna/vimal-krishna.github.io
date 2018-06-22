---
title: How to restrict WiFi on Android phones (NO ROOT)
date: 2018-06-19 15:26:00 +0530
layout: post
image: '../images/thumbs/thumb_Automate-Workflow-WiFiOff.png'
category: 'blog'
introduction: Effectively disable WiFi without root
description: Learn how to restrict WiFi on Android phones using a password (No rooting required)
published: true
---



This is a solution to disable access to WiFi on an Android device without requiring root privileges. Access to the WiFi settings on the phone can be  controlled via password or secret patterns.
We make use of applications that are available for free on the Play Store to achieve this - [AppLock](https://play.google.com/store/apps/details?id=com.domobile.applock&hl=en_IN) and [Automate](https://play.google.com/store/apps/details?id=com.llamalab.automate&hl=en).

The first app we use is called [AppLock](https://play.google.com/store/apps/details?id=com.domobile.applock&hl=en_IN). This is an app which restricts access to any other installed app on the phone. The user will have to provide a **password** in order to gain access. 

<p align="center">
	<img align="center" src="/images/AppLock-password.png" width="300"/>
</p>

We can successfully block access to the **Settings** screen of the Android OS using just this app.

<p align="center">
	<img align="center" src="/images/AppLock-Settings.png" width="300"/>
</p>

However, during my testing, I discovered the **quick toggle tiles** are not blocked by this method (the ones which can be pulled down from the top of the screen). This is probably because it is not part of the Android Settings app.

<p align="center">
	<img align="center" src="/images/Android-QuickTiles.png" width="300"/>
</p>

The second app that we will use, to complement AppLock is [Automate](https://play.google.com/store/apps/details?id=com.llamalab.automate&hl=en). Automate is like Tasker, but with a more intuitive interface. It allows you to automate actions on the phone which can be trigerred by various events. 

In simple terms, we will create a workflow with an action - **disable WiFi** for the event - **enable WiFi**.
This effectively keeps the WiFi turned off at all times! 

<p align="center">
	<img align="center" src="/images/Automate-Workflow-WiFiOff.png" width="300"/>
</p>

The advantage of using Automate to control WiFi access is that it only depends on the **current state** of the WiFi, and not the app which requested the state change. So, it wouldn't matter if you tried to enable WiFi using **quick tiles** or **network settings** or another completely independent **app**. In contrast, with AppLock we will have to **lock every single app** which could possibly control the WiFi state.

Now once this is done, we use AppLock again to restrict access to Automate, so that the user cannot modify the settings created in Automate.

<p align="center">
	<img align="center" src="/images/AppLock-Automate.png" width="300"/>
</p>

We can simplify the entire solution and purely depend on Automate's workflow to keep the WiFi disabled, as it covers all scenarios in which the WiFi can move to the enabled state. AppLock can be purely used to restrict access to Automate.

What if the apps AppLock and Automate were to be **uninstalled** by the user? Yes, once these apps are uninstalled, the restriction on WiFi is removed, but there is a way to **prevent the apps from being uninstalled**.

**Android Device Administrator** - I discovered, while doing this research that there is a concept called device administrator apps, built right into Android. What this does is that it gives some apps special permissions to perform admin like tasks on the device. As a side effect of being device admin app, those apps **cannot be uninstalled**. So how do we register our apps as device admins? Well, that's upto the app developers actually, and luckily both AppLock and Automate **have the option** of becoming device admin apps. 

<p float="left">
<img src="/images/Android-Device-Admin.png" width="300"/>

<img src="/images/Automate-Cant-Uninstall.png" width="300"/>
</p>

After setting up these apps in this way, WiFi is effectively disabled unless the user opens Automate (via AppLock password) and stops the workflow that turns the WiFi off. Otherwise, every other action that causes the WiFi to turn **ON**, would trigger the Automate workflow which will turn the WiFi back **OFF**.

-Vimal Krishna
