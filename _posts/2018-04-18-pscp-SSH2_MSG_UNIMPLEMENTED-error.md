---
title: pscp and SSH2_MSG_UNIMPLEMENTED error
date: 2018-04-18 15:26:00 +0530
layout: post
image: '../images/thumbs/thumb_GitLab-CI-Python-2018-06-22-16_15_47.png'
category: 'blog'
introduction: SSH2_MSG_UNIMPLEMENTED
description: SSH2_MSG_UNIMPLEMENTED error when using pscp.exe
published: true
---
A description of the SSH2_MSG_UNIMPLEMENTED error when using pscp.exe and how to overcome it.

```bash
C:\> pscp.exe sourceFile.txt root@192.168.168.2:/tmp
Fatal: Disconnected: Server protocol violation: unexpected SSH2_MSG_UNIMPLEMENTED packet
```

The problem seems to be with the method used to perform the key exchange, wherein the server is rejecting the request.

This seems to be a common problem while using Putty as well, and the solution with respect to Putty is to modify the order of the key exchange protocols.
The first one is **"Diffie-Hellman group exchange"**. Move it down to third or fourth in that list.

With pscp, I could not find a way to modify the key exchange protocol in the command line itself. 
The way I got it to work was to first create a session using Putty and then to use that session within pscp.
It worked because the session was saved with the modified key exchange protocol order.
