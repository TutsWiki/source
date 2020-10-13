---
date: 2020-08-22
linktitle: Access internet on Linux using Android Tethering
title: How to access to internet on Linux using Android Tethering
weight: 10
url: /access-internet-on-linux-using-android-tethering
description: Find out how to connect your Linux machine to internet using Android's mobile data via USB, WiFi or Bluetooth.
keywords:
  - android
  - tethering
  - internet
  - usb
  - wifi
  - bluetooth
tags: [Android, Linux]
---
<meta property="og:image" content="https://tutswiki.com/images/usb_tether_2.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="How to access to internet on Linux using Android Tethering" />
<meta name=”twitter:description” content="Find out how to connect your Linux machine to internet using Android's data via USB, WiFi or Bluetooth." />

## What is Tethering?
It's been over 30 years since the World Wide Web was introduced to the world. Over time, the internet took its toll and now has roughly around 45 million web pages compared to 10,000 websites earlier. We really do have come a long way.

Emerging from the primitive world of 2G signals we are now living in the era where mostly every device has an internet connection and if not of its own, gets one from another system. The technical term for sharing your phone’s internet connection to another phone or system is known as Tethering.

## Tethering on Linux
Tethering the internet through your phone to your Linux desktop can be done mainly via three simple ways.

Let's check them out!

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### USB Tethering
Connection over USB is just as simple as connecting your phone to a charger point. However, just like electricity is essential to charge your phone, internet connection in your phone is necessary to share it with others. Rare is the case when your charging cable is not suitable to be used as a data-transfer wire and you need to buy an apt one separately.

The connection is automatically established by plugging in your USB cable to both your computer and phone. A dialog box of confirmation highlighting the name of your device pops up.

![usb tether connect device](/images/usb_tether.png "Notification on connecting Android to USB")

Now, Under the `Settings` in your phone select `Wireless and Network` (or `Network and Internet`).
Click on the `More` tab and choose `Tethering & Portable Hotspot` and `Enable USB Tethering`. On the top right corner of your Linux machine, bring down the `Network Connections` toolbar as shown, where a tab `Ethernet Connected` is visible.

![usb tether Ethernet Connected](/images/usb_tether_2.png "Ethernet Connected")

**Your system's internet connection is ready!**

However, in a few cases you might have to further enable the `Wired Settings` window, where you can add a new connection and also view the IP address of your attached device. Here in this case, it is named `Profile 1`.

![usb tether Profile 1](/images/usb_tether_3.png "Profile 1")

Click on `Profile 1`

![usb tether IP address](/images/usb_tether_4.png "IP address")

### Wifi Tethering
Following the same process as mentioned above, in your Android phone, open `Settings` and go to `Network & Internet`. Next, select `Mobile Hotspot and Tethering` and turn on the `Mobile Hotspot`. In your Linux system, again under the `Network Connections`, turn `Wifi On`

![wifi tether turn on wifi](/images/wifi_tether.png "Turn On Wifi")

and go to `Wifi Settings`.

![wifi tether settings](/images/wifi_tether_2.png "Wifi Settings")

Here you can view all the available networks and connect with anyone by just entering the password available in your phone under `Mobile Hotspot`.

![wifi tether settings connect](/images/wifi_tether_3.png "Connect to Wi-Fi")

The Wi-Fi symbol in the top panel will now indicate that it's connected.

### Bluetooth Tethering
Yes! The bluetooth connection can be used not only to transfer data files but for passing on your phone's internet connection to your Linux machine. Navigating through the `Network Connections` tab, switch on the `Bluetooth` on your Linux distro and go to `Bluetooth Settings`.

![bluetooth tether turn on](/images/bluetooth_tether.png "Turn On Bluetooth")

Make sure you have turned on Bluetooth in your phone too. Both of your devices will discover each other so click on the device's name and a confirmation pin message will show up on both of the devices. If the pin matches, click confirm and Bluetooth pairing will be established.

![bluetooth tether pairing code](/images/bluetooth_tether_2.png "Pairing Code")

Now, again go to the Network Connections tab and under the paired device, here `e9b7c69c`, select the option to `Connect to Internet`.

![bluetooth tether connect internet](/images/bluetooth_tether_3.png "Connect to Internet")

The Bluetooth symbol in the top panel will now indicate that it's connected.

Go ahead and explore the unlimited boundaries of the internet on your Linux system! Thanks to the unimaginable scope of technology, without a doubt, your cellphone will always be at your rescue whenever a sudden need of an internet connection is required to your system.
