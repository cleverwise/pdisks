# pdisks - Protect Disks

DD in the *nix world is a **VERY powerful yet VERY destructive command**.  

Many find the benefit of using DD to perform a variety of tasks like writing out an ISO image to an USB stick, copying one partition/drive to another partition/drive, creating an image from a partition/drive, etc.  *However with this great power comes the ability, by easily not paying attention, of destroying valuable data.*

There are many stories on the Internet of people, perhaps you are one, with a simple mistype overwriting the root, home, block storage, etc partition/drive.  This mistake is far to easy to make because DD offers zero safe guards.  It doesn't ask for verification but rather simply does what it is told.

It is time to fix this oversight with this extremely easy to use BASH utility called **pdisks**.  Once deployed you use pdisks in place of the DD command.  Yes it fully supports *all* DD commands and flags.

**However pdisks verifies you haven't entered a system partition/drive thus preventing accidental overwrites by DD, at least to your core system (valuable removable storage could still be destroyed - read on)**.

When you run pdisks for the first time the utility will note **ALL** connected partitions and drives.  This includes any internal, block mounted, and removable storage (USB sticks, HDDs, SSDs).  This information will be saved to a configuration file in the form of a blacklist.  The way pdisks builds the blacklist is it scans for active devices that start with sd (/dev/sd*).  Therefore it is only automatically compatible with systems that use this schema like Linux.  Other *nix OSes would have to manually set the configuration file or modify the code slightly.

Also updating the blacklist is very simple by issuing **pdisks protect** which will start a new scan.  The main goal is to protect your core system storage devices like: /dev/sda, /dev/sda1, /dev/sda2, /dev/sdb, /dev/sdb1, etc.  If you add a new partition on /dev/sda you should have pdisks rescan your setup.

It is important you disconnect any removable storage you do not want to protect.  Do not simply unmount the device(s) but rather disconnect them from your system before a pdisks device scan otherwise this utility will still see the device (ie: /dev/sdc).  However on the flip side if you always have an external USB drive connected and mounted as say /dev/sdc (for example) then leave that powered on, connected, and mounted so it will be protected by pdisks.

If you do make an error by not connecting a device to protect or leaving a device connected by mistake then simply correct the error and rerun **pdisks protect** to rescan your system.  It is very easy.

It should be noted to watch out for future changes to the /dev/sd*.  What does that mean?

Well let's say you have an internal SSD, an USB external drive connected, and an important USB stick for files you move around town.  You want to protect all these partitions and drives so you have pdisks build the blacklist.  Then you disconnect your USB stick and connect a second unimportant USB stick.  In this case you have now swapped out /dev/sdc going from an important drive to an unimportant drive.  This means you won't be able to write over the now unimportant /dev/sdc.  Of course you may connect the unimportant USB stick while the important USB stick is connected so it becomes /dev/sdd.

So while the protection is worth it there are some situations that could occur that may get you confused.  Also you could create a situation where a protected removable drive is no longer protected.  How?  Let's say you have an internal SSD and external USB drive.  You run pdisks to build the blacklist.  Then you disconnect the USB drive which was /dev/sdb and protected then you insert an USB stick which becomes /dev/sdb.  Now with the USB stick still plugged in you reconnect the USB drive which becomes /dev/sdc and thus no longer protected.

**So again pdisks is really about protecting your core unchanging partitions and drives, but it can offer some level of protection for storage devices that get connected and disconnected.  Still think of it as an internal and always connected removable storage protector.**

More information: [https://www.cyberws.com/bash/pdisks/](https://www.cyberws.com/bash/pdisks/)

## Updates

* Follow this page
* [Follow on Youtube](https://www.youtube.com/channel/UCeQtI9fcAapQkiHph42NjWA)
* [Follow on Facebook](https://www.facebook.com/cyberwscom/)

## Contribute/Contact

* Message through Github
* [Contact Form on CyberWS](https://www.cyberws.com/contact-us/)

## Installing

This is a very easy utility to install.  You simply need to put the file **pdisks** on your system and turn on executable permissions like chmod 700 or 755.  It is suggested you place the file in **/home/YOUR_USER/bin/**.  Once that step has been done simply run the command from the commandline.  **The first time pdisks is run a blacklist of current partitions and drives will be built.**

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

shell> **pdisks if="/dev/zero" of="/dev/sde"**

shell> **pdisks if=system.img of=/dev/sdc bs=4096 conv=noerror**

shell> **pdisks if=/dev/sda2 of=/dev/sdb2 bs=4096**

### Updating Blacklist

shell> **pdisks protect**

#### Manually Editing The Blacklist

1) Using your preferred text editor open the **config ** in **/home/USER/.config/pdisks**
2) Edit the variable **SDEVLIST** with each drive and partition separated by a space.
3) Save and close.

#### Running dd on protected partitions/drives

There may come a time when you actually want to run dd on a protected partition/drive.  In this case simply use the dd command itself.
