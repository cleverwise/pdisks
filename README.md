# pdisks - Protect Disks

## This project is still in development ##

DD in the *nix world is a **VERY powerful yet VERY destructive command**.  

Many find the benefit of using DD to perform a variety of tasks like writing out an ISO image to an USB stick, copying one partition/drive to another partition/drive, creating an image from a partition/drive, etc.  *However with this great power comes the ability, by easily not paying attention, of destroying valuable data.*

There are many stories on the Internet of people, perhaps you are one, by a simple mistype overwriting root, home, block storage, etc partition/drive.  This mistake is far to easy to make because DD has no safe guards.  It doesn't ask for verification.  Nope.  It simply does what it is told.

It is time to fix this oversight with this extremely easy to use BASH utility called **pdisks**.  Once deployed you use pdisks in place of the DD command.  Yes it fully supports *all* DD commands and flags.

**However pdisks verifies you haven't entered a system partition/drive thus preventing accidental overwrites by DD, at least to your core system (valuable removable media could still be destroyed - read on)**.

When you run pdisks for the first time the utility will note **ALL** connected partitions and drives.  This includes any internal, block mounted, and removable storage (USB sticks, HDDs, SSDs).  Then this information will be saved to a configuration file in the form of a blacklist.  The way this works is the utility scans for active /dev/sd* so currently is compatible only with systems that use this schema like Linux.

It is very easy to update the blacklist by simply issuing **pdisks protect** to start a new scan.  The main goal is to protect your core system like /dev/sda, /dev/sda1, /dev/sda2, /dev/sdb, /dev/sdb1, etc.  If you add a new partition on /dev/sda you should have pdisks rescan your setup.

Also you should disconnect any removable media you do not want to protect.  Do not simply unmount the device(s) but rather disconnect them from your system before a pdisks drive scan otherwise pdisks will still see the device (ie: /dev/sdc).  However on the flip side if you always have an external USB drive connected and mounted as /dev/sdc (for example) then leave that powered on, connected, and mounted so it will be protected by pdisks.

If you do make an error by not connecting a device to protect or leaving a device connected by mistake then simply correct the error and rerun **pdisks protect** to rescan your system.  It is all very easy.

It should be noted to watch out for future changes to the /dev/sd* system.  What does that mean?

Well let's say you have an Internal SSD, an USB external drive connected, and an important USB stick for files you move around town.  You want to protect all these partitions and drives so you have pdisks build the blacklist.  Then you disconnect your USB stick and connect a second unimportant USB stick.  In this case you have now swapped out /dev/sdc going from an important drive to an unimportant drive.  This means you won't be able to write over the now unimportant /dev/sdc.  Of course you may add the unimportant USB stick while the important USB stick is connected so it becomes /dev/sdd.

So while the protection is worth it there are some situations that could occur that may get you confused.  Also you could create a situation where a protected removable drive is no longer protected.  How?  Let's say you have an SSD internal and external USB drive.  You run pdisks to build the blacklist.  Then you disconnect the USB drive which was /dev/sdb and protected and plugin a USB stick which now becomes /dev/sdb.  Now with the USB stick still plugged in you reconnect the USB drive which becomes /dev/sdc and thus no longer protected.

**So again pdisks is really about protecting your core unchanging partitions and drives, but it can offer some level of protection for devices that are connected and disconnected.  Still think of it as an internal and always connected removable storage protector.**
More information: [https://www.cyberws.com/bash/pdisks/](https://www.cyberws.com/bash/pdisks/)


## Updates

* Follow this page
* [Follow on Youtube](https://www.youtube.com/channel/UCeQtI9fcAapQkiHph42NjWA)
* [Follow on Facebook](https://www.facebook.com/cyberwscom/)

## Contribute/Contact

* Message through Github
* [Contact Form on CyberWS](https://www.cyberws.com/contact-us/)

## Installing

This is a very easy utility to install.  You simply need to put the file **pdisks** on your system and turn on executable permissions like chmod 700 or 755.  It is suggested you place the file in **/home/YOUR_USER/bin/**.  Once that step has been done simply run the command from the commandline.  **The first time pdisks is run a blacklist of current partitions and drives will be built**

To bring up a list of commands:

shell> **pdisks help**

### Storing Data

Pdisks stores configuration data in **/home/USER/.config/pdisks/**

### Removing

What? You want to remove pdisks? Why!? :-(

1) Remove the **pdisks** file itself; where ever you installed it.  If necessary use whereis or find commands to locate the file.
2) Finally delete the directory **/home/USER/.config/pdisks/**
3) Done.  Now reinstall it right? ;-)

### Normal Usage

You simply replace the dd command with pdisks.  All flags and commands stay the same.

Examples:

shell> **pdisks if="/dev/null" of="/dev/sde"**

shell> **pdisks if=system.img of=/dev/sdc bs=4096 conv=noerror**

shell> **pdisks if=/dev/sda2 of=/dev/sdb2 bs=4096 conv=noerror**

### Updating Blacklist

shell> **pdisks protect**

