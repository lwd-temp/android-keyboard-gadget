Convert your Android device into USB keyboard/mouse, control your PC from your Android device remotely, including BIOS/bootloader.

For newer Kernel versions (>= 3.19) the patch is not anymore required and can be replaced by ConfigFS ([USB Gadget Tool](https://github.com/tejado/android-usb-gadget)).

#### Apps & tools using android-keyboard-gadget:
* [USB Keyboard](https://play.google.com/store/apps/details?id=remote.hid.keyboard.client)
* hid-gadget-test
* [Authorizer](https://github.com/tejado/Authorizer)

Installation
============

Nexus 7 2012 WiFi (Grouper)
---------------------------

- Plug your device into PC using USB cable.
- Power off the device.
- Hold Volume Down button and Power button for 5 seconds, to enter fastboot mode.
- Copy appropriate fastboot executable from the directory `fastboot`.
- Launch command `fastboot oem unlock`
- Confirm unlock action by pressing Power button. This will factory reset your device.
- Copy `boot.img` from directory [nexus7-2012-wifi-grouper](nexus7-2012-wifi-grouper).
- Launch command `fastboot flash boot boot.img`.
- Reboot your device using Power button.
- Install and run USB Keyboard app.

Nexus 7 2013
------------

- http://forum.xda-developers.com/showthread.php?t=2809979

LG G2
-----

- http://forum.xda-developers.com/showthread.php?t=2725023

LG G2 with Cyanogenmod 12.0
---------------------------

- https://github.com/pelya/android-keyboard-gadget/tree/master/lg-g2

Nexus 5
-------

- http://forum.xda-developers.com/showthread.php?t=2551441
- http://forum.xda-developers.com/showthread.php?t=2527130
- [Boot image for Android 5.0](nexus5-hammerhead-android-5.0)

Nexus 4
-------

- http://forum.xda-developers.com/showthread.php?t=2548872
- http://forum.xda-developers.com/showthread.php?t=2858561

Sony Ericsson phones
--------------------

- http://legacyxperia.github.io/

Motorola Moto G with Cyanogenmod
--------------------------------

- http://forum.xda-developers.com/showthread.php?t=2634745
- http://forum.xda-developers.com/showthread.php?t=2786336

Motorola Moto E with Cyanogenmod
--------------------------------

- http://forum.xda-developers.com/showthread.php?t=2931985

Motorola Moto G 2014
--------------------

- http://forum.xda-developers.com/showthread.php?t=3085277


Motorola Moto X Style (Pure) 2015
---------------------------------

- http://forum.xda-developers.com/showthread.php?t=3554226

OnePlus One
-----------

- http://sourceforge.net/projects/namelessrom/files/bacon/ - it's ROM, not just a kernel

Galaxy S4
---------

- http://forum.xda-developers.com/showthread.php?t=2590246 - you have to enable it in the included STweaks app

Galaxy Note 2
-------------

- http://forum.xda-developers.com/showthread.php?t=2231374

Huawei Ideos X5
---------------

- http://forum.xda-developers.com/showthread.php?t=2616956

Sony Xperia Z3 and Z3 Compact
-----------------------------

- http://forum.xda-developers.com/showthread.php?t=2937173

Sony Xperia Z Ultra
-------------------

- http://forum.xda-developers.com/showthread.php?t=3066748

Xiaomi Redmi 1S
---------------

- http://forum.xda-developers.com/showthread.php?t=2998620

Xiaomi Redmi 3S
---------------

- http://forum.xda-developers.com/showthread.php?t=3667804

Galaxy Ace 2
------------

- http://forum.xda-developers.com/showthread.php?t=2793420

Xiaomi MI3
----------

- http://forum.xda-developers.com/showthread.php?t=3093399

Galaxy Note 4
-------------

- http://www.echoerom.com/ael-kernels/

Asus Zenfone 2 ZE551ML
----------------------

- https://github.com/pelya/android-keyboard-gadget/blob/master/asus-Zenfone-2-ZE551ML/boot.img?raw=true

Asus Zenfone 2 Laser (Z00L/Z00T)
----------------------

- https://forum.xda-developers.com/zenfone-2-laser/development/kernel-firekernel-t3703326

Sony Xperia Z5 Premium E6853
----------------------------

- https://github.com/pelya/android-keyboard-gadget/blob/master/sony-xperia-z5p/boot.img?raw=true

Sony Xperia Z5 Compact
----------------------

- https://github.com/pelya/android-keyboard-gadget/blob/master/sony-z5c/boot.img?raw=true

Xiaomi Redmi 2
--------------

- http://forum.xda-developers.com/redmi-2/development/kernel-berserk-kernel-t3325044

Sony Xperia SP
--------------

http://forum.xda-developers.com/xperia-sp/development/kernel-helium-v1-t3251298

Xiaomi Redmi Note 3
-------------------

- http://forum.xda-developers.com/showthread.php?t=3439626

Samsung Galaxy Tab 2 (any espresso3g based device)
--------------------------------------------------

- https://github.com/pelya/android-keyboard-gadget/blob/master/compiled/Samsung_Galaxy_Tab_2-espresso3g_LINEAGE-13.0/boot.img

Other devices
-------------

- You will have to compile the kernel yourself.

Scripting
=========

There is a possibility to send keypresses in an automated way, using terminal emulator for Android or similar app.
This is done using [hid-gadget-test](hid-gadget-test/hid-gadget-test?raw=true) utility.

First, copy this utility to your device.

	adb push hid-gadget-test /data/local/tmp
	adb shell chmod 755 /data/local/tmp/hid-gadget-test

You will need to set world-writable permissions on /dev/hidg0, or run hid-gadget-test from root shell.

	adb shell
	su
	chmod 666 /dev/hidg0 /dev/hidg1

To always have root shell, so you don't need to enter 'su' each time, run command

	adb root

Then, use hid-gadget-test to send keypresses.

	adb shell
	cd /data/local/tmp

	# Send letter 'a'
	echo a | ./hid-gadget-test /dev/hidg0 keyboard

You can also run this command without launching ADB shell, from shell script or .bat file.

	adb shell 'echo a | /data/local/tmp/hid-gadget-test /dev/hidg0 keyboard'

Advanced examples.

	# Send letter 'B'
	echo left-shift b | ./hid-gadget-test /dev/hidg0 keyboard

	# Send string 'abcdeZ'
	for C in a b c d e 'left-shift z' ; do echo "$C" ; sleep 0.1 ; done | ./hid-gadget-test /dev/hidg0 keyboard

	# You may combine several modifier keys
	echo left-ctrl left-shift enter | ./hid-gadget-test /dev/hidg0 keyboard

	# Try to guess what this command sends
	echo left-ctrl left-alt del | ./hid-gadget-test /dev/hidg0 keyboard

	# Bruteforce 4-digit PIN-code, that's a particularly popular script
	# that people keep asking me for. It executes for 42 hours.
	for a in 0 1 2 3 4 5 6 7 8 9; do
	for b in 0 1 2 3 4 5 6 7 8 9; do
	for c in 0 1 2 3 4 5 6 7 8 9; do
	for d in 0 1 2 3 4 5 6 7 8 9; do
	echo $a $b $c $d
	for C in $a $b $c $d enter ; do echo "$C" ; sleep 0.2 ; done | ./hid-gadget-test /dev/hidg0 keyboard
	sleep 15
	done
	done
	done
	done

	# Press right mouse button
	echo --b2 | ./hid-gadget-test /dev/hidg1 mouse

	# Hold left mouse button, drag 100 pixels to the right and 50 pixels up, then release
	echo --hold --b1 | ./hid-gadget-test /dev/hidg1 mouse
	echo --hold --b1 100 0 | ./hid-gadget-test /dev/hidg1 mouse
	echo --hold --b1 0 -50 | ./hid-gadget-test /dev/hidg1 mouse
	echo --b1 | ./hid-gadget-test /dev/hidg1 mouse

You can check the modification time of file `/sys/devices/virtual/hidg/hidg0/dev`
to know when the USB cable has been plugged into PC, however this does not always work,
so it's better to simply check if hid-gadget-test returned an error.

Here's a sample shell script that will send a predefined key sequence when USB cable is plugged into PC:

	#!/system/bin/sh
	while true; do
		until echo volume-up | ./hid-gadget-test /dev/hidg0 keyboard >/dev/null 2>&1; do
			sleep 2
		done
		echo "USB cable plugged"
		sleep 1
		for C in 'left-meta r' c m d enter s t a r t space i e x p l o r e space x x x period c o m enter \
			; do echo "$C" ; sleep 0.3 ; done | ./hid-gadget-test /dev/hidg0 keyboard
		echo "Done sending commands"
		while echo volume-up | ./hid-gadget-test /dev/hidg0 keyboard >/dev/null 2>&1; do
			sleep 2
		done
		echo "USB cable unplugged"
	done

Here is [the list of keys that hid-gadget-test utility supports](hid-gadget-test/jni/hid-gadget-test.c#L33)

If you need to crack a PIN code, but the target system loses keypresses (happens in MacOS BIOS),
there is a [handy app](send-pin-with-camera/) for that,
which uses camera to check if each keypress is recognized.

You can also run DuckyScript files used by USB Rubber Ducky keystroke injection tool,
with the help of [this neat shell script](https://github.com/anbud/DroidDucky).


Compiling kernel
================

You have to run all following commands on Linux. Windows is not supported. These instructions are for Nexus 7 2012, change them for your device accordingly.

To compile kernel, launch commands

	git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.8
	git clone https://android.googlesource.com/kernel/tegra.git
	export PATH=`pwd`/arm-eabi-4.8/bin:$PATH
	export ARCH=arm
	export SUBARCH=arm
	export CROSS_COMPILE=arm-eabi-
	cd tegra
	git checkout android-tegra3-grouper-3.1-lollipop-mr1
	patch -p1 < ../kernel-3.1.patch
	make tegra3_android_defconfig
	make -j4

https://github.com/pelya/android-keyboard-gadget/tree/master/patches/existing_tested - Tested patch files
(pro) Tip: generic_kernel_version_3.xx.patch is just patches for AOSP roms and just by basic kernel versions. 3.01 is acceptable for 3.0.101. Other patch files is for devices only. The file named HTC Flounder will be acceptable ONLY FOR HTC Flounder!

To compile `boot.img`, launch commands

	mkdir ~/bin
	export PATH=~/bin:$PATH
	curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
	chmod a+x ~/bin/repo
	mkdir aosp
	cd aosp
	repo init -u https://android.googlesource.com/platform/manifest -b android-4.4.2_r1
	repo sync
	cp -f ../tegra/arch/arm/boot/zImage device/asus/grouper/kernel
	make -j4 TARGET_PRODUCT=aosp_grouper TARGET_BUILD_VARIANT=userdebug

You then can find `boot.img` in directory `aosp/out/target/product/grouper`.

Nexus 7 2012 does not put any SELinux restrictions on the files inside /dev,
however most other devices typically restrict all access inside /dev for apps,
which means you will be able to use `hid-gadget-test` command from the root shell,
but the Android app will fail to launch.

SELinux can be temporarily disabled with a command

	setenforce 0

however this will considerably weaken your device security,
so it's better to add specific SELinux exception for `/dev/hidg0` and `/dev/hidg1`.

SELinux rules can be found at path

	device/asus/grouper/sepolicy

Replace `asus/grouper` with your device manufacturer/model, then add following lines to SELinux rules.

In file `device.te` - the declaration of SELinux security context type:

	type hid_gadget_device, dev_type;

In file `file_contexts` - binding the device paths to the security context:

	# USB Gadget
	/dev/hidg(.*)                        u:object_r:hid_gadget_device:s0

In file `app.te` - the rule itself to allow apps using this security context:

	allow appdomain hid_gadget_device:chr_file rw_file_perms;

Then recompile `boot.img`.


Compiling USB Keyboard app
==========================

To compile USB Keyboard app, install Android SDK and NDK from site http://developer.android.com/ , and launch commands

	git clone https://github.com/pelya/commandergenius.git
	cd commandergenius
	git submodule update --init --recursive
	rm -f project/jni/application/src
	ln -s hid-pc-keyboard project/jni/application/src
	./changeAppSettings.sh -a
	android update project -p project

How it works
============

The custom kernel you have compiled adds two new devices, /dev/hidg0 for keyboard, and /dev/hidg1 for mouse.

You can open these two files, using open() system call,
and write raw keyboard/mouse events there, using write() system call,
which will be sent through USB cable to your PC.

A short description of the events format is provided below. For a more comprehensive overview see the [OS Dev Wiki](https://wiki.osdev.org/USB_Human_Interface_Devices#USB_keyboard).

Keyboard events
---------------
Keyboard events are 8 bytes long:
| Offset | Length | Field |
| --- | --- | --- |
| 0 | 1 byte | Modifier keys bitmask |
| 1 | 1 byte | Reserved padding |
| 2 | 6 bytes | Pressed keys in the order they were pressed in |
_______________________________________________________________
First byte is a bitmask of currently pressed modifier keys:

	typedef enum {
		LCTRL = 0x1,
		LSHIFT = 0x2,
		LALT = 0x4,
		LSUPER = 0x8, // Windows key
		RCTRL = 0x10,
		RSHIFT = 0x20,
		RALT = 0x40,
		RSUPER = 0x80, // Windows key
	} ModifierKeys_t;
_______________________________________________________________
The second byte is reserved and usually ignored by software.
_______________________________________________________________
Remaining 6 bytes are a list of all other keys currently pressed, one byte for one key, or 0 if no key is pressed.
A handy table with key scancodes can be found [here](https://www.win.tue.nl/~aeb/linux/kbd/scancodes-14.html).

Extended keys, such as Play/Pause, are not supported, because they require modifying USB descriptor in kernel patch.

Consequently, the maximum amount of keys that may be pressed at the same time is 6, excluding modifier keys.
Professional or 'gamer' USB keyboards report several keyboard HID descriptors, which creates several keyboard devices in host PC,
to overcome that 6-key limit.

Note that after writing an event the keys will be held indefinitely, so when finished an empty event should be sent to release all keys.

Mouse events
------------
Mouse event is an array of 4 bytes, first byte is a bitmask of currently pressed mouse buttons:

	typedef enum {
		BUTTON_LEFT = 0x1,
		BUTTON_RIGHT = 0x2,
		BUTTON_MIDDLE = 0x4,
	} MouseButtons_t;

Remaining 3 bytes are X movement offset, Y movement offset, and mouse wheel offset, represented as signed integers.
Horizontal wheel is not supported yet.

See functions outputSendKeys() and outputSendMouse() inside file [input.cpp](remote-client/input.cpp)
for reference implementation.
