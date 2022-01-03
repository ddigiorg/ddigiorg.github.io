---
layout: page
title: Samsung Tablet Change Aspect Ratio
date: 2022-01-03
tags: computer
---

Ensure tablet is connected and USB debugging is on.

Install [Android Studio](https://developer.android.com/studio?pkg=tools) and install any SDK packages it reccomends.

In a terminal, use Android Debug Bridge (ADB) to change resolution on tablet:

``` bash
cd "C:\Users\ddigiorg\AppData\Local\Android\Sdk\platform-tools"
./adb.exe devices
./adb.exe shell
wm size 1080x1920
exit
```

## Links

- <https://joyofandroid.com/change-screen-resolution/>
- <https://joyofandroid.com/what-is-adb-and-how-to-use-it/>