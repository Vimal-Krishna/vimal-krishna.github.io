---
layout: post
title: An alternative to Putty and Vi
date: '2017-10-23 15:26:00 +0530'
layout: post
description: Exploring Netbeans IDE as a remote development tool
image: 'https://vimal-krishna.github.io/images/thumbs/thumb_netbeans-ide-remote.png'
category: 'blog'
introduction: Exploring Netbeans IDE as a remote development tool
published: true
---

We are most productive when we are able to create things as fast as we can visualize them in our minds.

For years, I’ve used a combination of Putty and Vi editor (or variants of these) to write C++ code for Linux targets. Of course, this was the most popular choice among developers, and most were very comfortable with it too, me included. The corporate laptops would mostly be running Windows, and the development machines would be running Linux, likely on a Virtual Machine, tucked away in a lab far away.

The first thing we do when we get our corporate laptops is to download Putty and then login into the development machine. This development machine has all the tools configured to help us write our code. We often use Vi to edit code and some other command line utilities to find methods, classes and files.

I understand that it is important for developers to not become overly dependent on visual IDEs, because it may not be available at their next project or company. Having your own collection of free tools that improve your productivity is very important.

I did try hard to make Vi much more usable by adding many plugins, even one that made autocomplete a possibility on Vi, but something was truly amiss.

Enter Netbeans IDE and its Remote Development features. This really is the one tool that solves all my problems. Set it up once, and you get all the cool features that the Windows developers get with Visual Studio. Easy now, its not the same as Visual Studio, but hey, this is a huge step up from Vi. I now enjoy code completion, tabbed interface, a GUI and most importantly – visual debugging with GDB.  If you think about it, the entire Netbeans GUI is like a fancy SSH terminal like Putty with multiple tabs of Vi inside it, with all its fancy plugins.

Writing code with a GUI tool like Netbeans, certainly helps that train of thought to continue without being interrupted. No! Coffee breaks are not interruptions! For some, using Vi may be doing this already, but I for one, ain’t going back.

Here’s the clincher. Since, Netbeans is free, you don’t have to worry about it not being available at your next project. It can be used for development on Windows as well as Linux!
