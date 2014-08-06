---
layout: page
title: "Selenium Grid"
category: doc
date: 2014-08-06 09:34:12
---


moz-grid-config
================

moz-grid-config is a project for manaing our Selenium infrastructure. it includes the latest version of Selenium  and a mechanism for starting the hub and nodes.


Starting the hub
=================

Connect using SSH or VNC to selenium.qa.mtv2.mozilla.com and execute the following from a terminal.

    cd ~/moz-grid-config
    ant launch-hub



Starting the OS X nodes
=======================

Connect to each node using SSH or VNC and execute the following froma terminal.

    cd ~/moz-grid-config
    ant launch-node


Starting the Windows nodes
==========================

We use the msysgit tool for git on Windows. Run the MsysGit tool from the desktop  and run the following commands

    cd ~/moz-grid-config
    ant launch-node


Shutdown
========

Connect to the nodes/hub and terminate the running process using CTRL+C  at the Terminal/ MsysGit tool

Architecture
=============

We have mac-minis running as Selenium nodes in MTV2 and 6 machines running Windows in SCL3.

Mac-Mini
---------

* selenium.qa.mtv2.mozilla.com

    * IP Address: 10.252.73.143
    * OS X 10.7.4, 2.7 GHz Intel Core i7, 8GB
    * Jenkins
    * Selenium Grid Hub


* selenium-2.qa.mtv2.mozilla.com ( offline/ cannot be located)
    
    * Mac Mini 1.83 Ghz Intel Core 2 Duo 4GB
    * Mac OS X  10.6.8
    

* selenium-3.qa.mtv2.mozilla.com
    
    * IP Address: 10.252.73.196
    * 2.4 Ghz Intel Core 2 Duo; 4GB
    * Mac OS X 10.6.8
    * VM: Windows 7 Professional (10.250.7.78)(No longer used after SCL3 switch)
    

* selenium-4.qa.mtv2.mozilla.com
    
    * IP Address: 10.252.73.197
    * Mac Mini; 2.4 Ghz Intel Core 2 Duo; 4 GB
    * Mac OS X 10.6.8
    * VM: Windows 7 Professional (10.250.4.68)(No longer used after SCL3 switch)
    

* qa-selenium5.qa.mtv2.mozilla.com
    
    * IP Address: 10.252.73.145
    * Mac Mini; 2.66 Ghz Intel Core 2 Duo; 8 GB
    * Mac OS X 10.6.8
    * VM: Windows 7 Professional (10.250.5.71)(No longer used after SCL3 switch)
    

* selenium-6.qa.mtv2.mozilla.com (does not exist/cannot be located)
    
    * Mac Mini; 2.53 Ghz Intel Core 2 Duo; 4 GB
    * Mac OS X 10.6.8
    * VM: Windows 7 Professional (does not exist/cannot be located)
    

* selenium-7.qa.mtv2.mozilla.com
    
    * IP Address: 10.252.73.9
    * Mac OS X 10.7.4, 2.5 GHz Intel Core i5; 4GB 1067 MHz DDR3
    * VM: Windows 7 SP1 pro (No longer used after SCL3 switch)
    

* selenium1.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.149
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    

* selenium2.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.150
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    

* selenium3.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.151
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    

* selenium4.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.152
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    

* selenium5.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.153
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    

* selenium6.qa.scl3.mozilla.com
    
    * IP Address: 10.22.73.154
    * Windows 7, 8 GB RAM, 2 cores, 150GB disk
    


Node Configuration
===================

Since Selenium 2.14.0 was released and allowed both RC and WebDriver nodes to co-exist, each node runs the same configuration of  browser nodes

Firefox version Policy
----------------------

On the Mac OS nodes we have:

    * Firefox 24
    * Firefox 28
    * Firefox 29

On the Windows 7 nodes we have:

    * IE 11
    * Chrome (latest-version)
    * Firefox 24
    * Firefox 28
    * Firefox 29


Adding a new browser environment
================================


Configuration
-------------

You will ned to fork and clone from the moz-grid-config github repository and work on a new branch.

* Add the browser, alias and path tho the grid_configuration.yaml file
* Add the browser, binary and details to the JSON configuration files
* Submit a pull request to the main GitHub repository from your branch


Hub/Nodes
----------

* Prepare Jenkins for shutdown
* Once no jobs are running , shutdown the Selenium grid hub
* Pull in the latest changes from moz-grid-config
* Start the Selenium Grid hub
* For each Selenium node:
    * Shutdown the Selenium node
    * Pull in the latest changes from moz-grid-config
    * Install the appropriate browser for the new environment
    * Start the Selenium node


Installing Firefox
-------------------

* All new browsers should be installed with the version number in the path so that moz-grid-config can always find them.
* Install mozdownload and mozinstall using pip to download and install the Firefox binaries.
    * use

            --help
     for both mozdownload and mozinstall to see all available options
    * to downlod the latest release using mozdownload you can simply use: 
            
            mozdownlooad -v latest

    * to download the latest Firefox Nightly build using mozdownload you can use
            
            mozdownload -t daily --branch=mozilla-central

    * to download the latest Firefox Aurora build using mozdownload you can use
            
            mozdownload -t daily --branch=mozilla-aurora
            
    * releases are also available to download manually here


Mac OS X
---------

From a terminal
    
    mozdownload -v 14.0.1 -d /tmp
    mozinstall -d /Applications/ /tmp/firefox-14.0.1.en-US.mac.dmg
    mv /Applications/Firefox.app/ "/Applications/Firefox 14.app/"
    rm /tmp/firefox-14.0.1.en-US.mac.dmg


Windows
--------

From a command prompt:

    mozdownload -v 14.0.1 -d %TMP%
    mozinstall -d %USERPROFILE% %TMP%\firefox-14.0.1.en-US.win32.exe
    move %USERPROFILE%/firefox-14 "\Program Files\Mozilla Firefox 14"
    del %TMP%\firefox-14.0.1.en-US.win32.exe



Setting hostname on Mac OS X
==============================

Shamelessly copied from http://budporter.net/?p=47

If you are running a version of OSX prior to 10.5, then look in /etc/hostconfig for the HOSTNAME= parameter and set it to be the FQDN (Fully Qualified Domain Name) that you want to use. So, for host foo located in the domain bar.com, the entry would be as follows: 

     HOSTNAME=foo.bar.com

For OSX 10.5, /etc/hostconfig is being deprecated. There are a couple of preferred methods to do this in 10.5

    sudo hostname -s foo.bar.com

or:

    sudo scutil –set HostName foo.bar.com

You can verify the change by issuing the following command:
    
    hostname

    

The output will display the FQDN of the computer.




