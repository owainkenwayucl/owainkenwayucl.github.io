---
layout: post
title: Renting Devices (or Android patching is a clusterfuck)
---

One of the things that infuriates me about modern technology is planned obsolescence – that is the way that modern devices are designed to expire after a fixed period of time.  In the “bad old days” this used to be that things mysteriously failed a few weeks after the warranty expired which in reality was at best sod’s law and at worst a sort of mad conspiracy theory. I have a PowerBook 150 (circa 1994) that works.  OK, so it has lots of new parts in it but it works and is maintainable like and old car – I can take it apart, Apple published an excellent manual for servicing it, it uses standard tools and so on. Modern devices are sealed, batteries are non-user removable and companies like Apple deliberately use weirdly patterned screwdrivers to frustrate repair. Apple, incidentally work even harder to restrict the supply of spares and prevent even trained non-Apple technicians from repairing them (currently the subject of a strong campaign for right to repair legislation). Now with everything being Internet connected, this kind of behaviour is enforced in another way – in manufacturers limiting the period over which devices will get security updates.

Android is really what started off this mess for me – I’ve had a succession of cell phones running various “smart” operating systems, both Android and back when it was a thing, Windows Phone and the patching on Android is a catastrophic disaster waiting to happen. When Google created Android, partly because they had no power over cell providers or manufacturers, Google pushed all Android patching left to the manufacturer and worse, to the cell provider.  What does this mean for the user?

Well Google releases security updates for “supported” versions of Android on the first Monday of every month.  Supported Google devices connected to Wifi get those updates either at about 8PM UK time or if the update service in Play Services is broken again “at some point over the next two weeks”.  If you don’t have a Wifi connection you may eventually get the update over the cell network although I have never observed this in the wild.

Sounds Okish, right? Well no. “[Supported Google devices](https://support.google.com/pixelphone/answer/4457705#pixel_phones&nexus_devices)” is a surprisingly small number of devices, consisting only of devices Google makes that were released on the Google US store up to three years ago (or stopped being sold less than eighteen months ago).  Currently this means the Pixel 2 upwards, with the 2 expiring in October.

So what about all the other Android devices?

Well they are not supported.

What happens is in the month leading up to patch day, Google shares the patches it intends to make with its larger partners.  They then decide their own patching schedule. Samsung, for example, one of the largest manufacturers, [Samsung](https://security.samsungmobile.com/securityUpdate.smsb) says:

> “Samsung Mobile is releasing a maintenance release for major flagship models as part of monthly Security Maintenance Release (SMR) process. This SMR package includes patches from Google and Samsung.”

That’s interesting. What do they mean by “major flagship models”?

Well it turns out that if you buy an expensive device from Samsung, you get monthly updates. If you are not well off, you get [quarterly ones](https://security.samsungmobile.com/workScope.smsb).  I couldn’t find a link for how long you get updates, but previously it’s been reported as two years from when the device is first sold.

It’s not clear how long LG support devices either but they [seem](https://lgsecurity.lge.com/security_updates_mobile.html) to patch cell phones monthly.

It gets worse however. If you buy a phone from your cell provider (and sometimes even if you buy a bare device from the manufacturer) they step in the way between you and the manufacturer and decide whether you get an update or not, blocking it behind opaque testing regimes, if they allow you to get the update at all.

And then there are the budget devices, running historical re-enactment editions of Android such as 4.x which will never receive updates.  Security updates are important. Those of us old enough will remember the numerous Windows worms in the XP era where no-one ever applied updates.  There are now **billions** of [unpatched Android devices](https://www.which.co.uk/news/2020/03/more-than-one-billion-android-devices-at-risk-of-malware-threats/?utm_campaign=whichukf&utm_medium=social&utm_source=twitter&utm_content=AndroidMalware&utm_term=twnews), mostly owned by the poor or those not educated in IT sitting quietly connected to the Internet all the time, used by most people for financial transactions.  The implications are terrifying.

With ChromeOS, Google has learned a bit and seized control of update delivery, but still sets really short limits (2-3 years from first sale) on how long updates are provided for.

I want at this point to point out that it doesn’t have to be this way. I also own an Ipad Mini 4 which has had about five years of timely updates direct from Apple and will continue to get security and even feature updates until at least IpadOS 15 in late 2021.  Even though this sounds like an Apple fanboy, there is actually no reason for me to throw away my iPad Mini 4 except that it will stop being supported, so even then it is still a waste. It's also unhelpful that Apple don't publish a planned time-scale for support which means [every WWDC people with older devices are waiting with baited breath to find out whether they have to buy a new device in the next couple of months](https://twitter.com/owainkenway/status/1275146900535214080). Microsoft have revolutionised patching in the Windows environment where if your device is powerful enough to run Windows 10 it will now get updates effectively until it falls apart, as long as you keep up with the feature updates.  Even before that, Microsoft supported versions of Windows long past sanity, with XP lasting a particularly long time and Windows 7 only being retired January this year.  Microsoft on PCs (their record on cell phones is as bad as Google’s sadly) gets a gold star.

It’s clear to see why this is a problem – not only do consumers spend a lot of money when purchasing a device, but throw-away electronics are fast becoming an ecological disaster.

The net result is whereas in the world of cellphones consumers are effectively renting their devices.

For example, I bought a Pixel 3 on release day. I got a bare phone partly because my cell provider didn’t carry Google devices, partly to avoid them getting in the way of updates.  The Pixel 3 cost £750.  Since it gets updates for 3 years, I am effectively renting it for £250 a year.  That’s a lot.  I’m still daily using PCs that are ten years old.  This will probably be my last Android phone because this is expensive compared to an iPhone. If iPhones get updates for five years, then they cost less per year, even if they initial outlay is larger.  Indeed the only reason I got a Pixel this time is that Apple cancelled the iPhone SE months before my Nexus 5X’s patching expired (the phone was still fine).  Now I can afford to pay more for an iPhone, but what if you can’t afford the one off expense?  Why is it that an acceptable level of security is “not for the poor”, both implied in the structure of the market and deliberately codified by companies like Samsung?

As a final whinge, Microsoft are launching their first Android device, the Surface Duo that retails for $1399.99 later this year.  [It gets three years](https://www.androidauthority.com/microsoft-surface-duo-android-updates-1147535/) of (presumably) monthly Android updates from release which is $466.67 per year, assuming you buy it on the first day – every day you delay it costs more!

Consumers who buy these devices and who rely on them for tasks such as banking need security.  They should have it for longer than three years. The planet needs devices to last longer than three years. Manufacturers should support their products with monthly security updates, delivered direct to the device for a minimum of five years if not significantly more.  We are past the point where devices getting more powerful is meaningful for anyone but gamers and people should expect to keep phones longer than PCs, not shorter.

There’s another out – at the end of support, the device’s bootloader should be unlocked so that users can move to an Open Source operating system if they wish, without resorting to dodgy firmware jailbreaks. On one shelf I have a small pile of every smart phone I’ve every owned.  They all work fine. They are all powerful enough.  None of them get patches.  It’s a tower of the complete failure of an industry to care about its customers or the planet.