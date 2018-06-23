---
layout: post
title: gSoap C++ module in Apache server
date: '2017-12-29 15:26:00 +0530'
layout: post
description: Working instructions to install gSoap as a C++ module in Apache
image: '/images/gSoapCppApache.png'
category: 'blog'
introduction: Working instructions to install gSoap as a C++ module in Apache
published: true
---

## Installing a gSoap C++ module in Apache server

The instructions at the [genivia website](https://www.genivia.com/doc/apache/html/index.html) just did not work while trying to install gSoap as a C++ module.

If you are not familiar with how to write a module for Apache, this [blog post](http://theunixtips.com/howto-develop-apache-module-in-c/) will get you started on it. It takes you through the steps of creating a C module and then a C++ module.

The instructions provided by [www.genivia.com](https://www.genivia.com/doc/apache/html/index.html), ask us to run the following command to build the gSoap module for Apache:  
```bash
bin/apxs -a -c -S CC=c++ calcserver.cpp soapC.cpp 
soapcalcService.cpp stdsoap2.cpp

chmod 755 .lib/calcserver.so
```

There are no errors whatsoever, so it seems like the command worked and all is well.  
However, when trying to make a soap API call to the installed service, Apache throws the following error:  
````bash
gsoap: httpd.conf module mod_gsoap.c SOAPLibrary load "calcserver.so" success,  
but failed to find the "apache_init_soap_interface" function entry point defined by  
IMPLEMENT_GSOAP_SERVER()
````

On closer inspection and by comparison with the "C version" of the gSoap module for the same calulator example, we realized that the **DSO** (Dynamic Shared Object) created by apxs was different for the **"C version"** and the **"C++ version"** of the gSoap module.  

The difference is in the symbols that the DSO exposes. Running ````nm -C -D calcserver.so```` on the C and C++ versions shows this difference. You may also try this command on the output created in the sample C++ module from the blog post referenced earlier.

There may well be some parameteres to apxs that I am not aware of currently, but it sure is creating some inconsistent behavior. 

To solve the problem, we compiled the "C++ version" of the gSoap module separately as a DSO without using apxs. This is consistent with the C++ tutorial we went through. There was no need to use apxs there as well. The command to compile the gSoap service into a DSO is as follows: 
````bash
g++ -shared -fpic calcserver.cpp soapC.cpp soapcalcService.cpp $HOME/gsoap-2.8/gsoap/stdsoap2.cpp  
-I$HOME/gsoap-2.8/gsoap/mod_gsoap/mod_gsoap-0.9/apache_20 
-I$HOME/apachegsoap/include -o .libs/calcserver.so
````

After this, running the ````nm -C -D <path to DSO>```` command on the new DSO generated output which is consistent with the other examples in C and C++. Specifically, the **"apache_init_soap_interface"** method is shown in the output, which was not the case earlier.

![_config.yml]({{ site.baseurl }}/images/gSoap-C++-module-in-Apache-server-1.png)


To load the module in Apache, the following lines were added in the httpd.conf file:
````ApacheConf
LoadModule gsoap_module modules/mod_gsoap.so
<IfModule mod_gsoap.c>
 <Location /soap>
  SetHandler gsoap_handler
  SOAPLibrary /<path>/apachegsoap/bar/.libs/calcserver.so
  Order allow,deny
  Allow from all
 </Location>
</IfModule>
````  
