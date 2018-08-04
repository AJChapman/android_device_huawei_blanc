# Lineage for Huawei Mate 10 Pro

This is a very early work in progress to get Lineage OS working on a Huawei
Mate 10 Pro, codename `blanc`. It builds on the [work of
LuK1337](https://forum.xda-developers.com/honor-view-10/development/rom-lineageos-15-1-t3753000),
which gets Lineage OS working on Huawei Honor View 10 (berkeley) and Huawei P20 Pro (charlotte).

## Building

The build is similar to the one [described here](https://wiki.lineageos.org/devices/nash/build), with the following differences:
1. Where those instructions say `nash`, use `blanc`,
2. Skip the `breakfast` step, as this is only for officially supported devices, and we're not there yet. Instead do the following:
``` bash
~/android/lineage $ mkdir -p device/huawei
~/android/lineage $ cd device/huawei
~/android/lineage/device/huawei $ git clone https://github.com/berkeley-dev/android_device_huawei_kirin970-common kirin970-common
~/android/lineage/device/huawei $ git clone https://github.com/AJChapman/android_device_huawei_blanc blanc
~/android/lineage/device/huawei $ cd ../../vendor
~/android/lineage/vendor $ mkdir huawei
~/android/lineage/vendor $ cd huawei
~/android/lineage/vendor/huawei $ git clone https://github.com/berkeley-dev/proprietary_vendor_huawei_kirin970-common kirin970-common
~/android/lineage/vendor/huawei $ cd ../../kernel
~/android/lineage/kernel $ git clone https://github.com/berkeley-dev/kernel_common common
```

You don't need to extract binary blobs, that's what the
proprietary_vendor_huawei_kirin970-common repo does for you. Or maybe it's
better if you do? Not sure, sorry :/

## Getting it Running
Once you have built your system.img, check out [this guide for installing it](https://forum.xda-developers.com/mate-10/how-to/guide-treble-compatible-rom-t3761927).

## Development
This repo is pretty simple. It just defines a few strings, such as `blanc`, and
`Mate 10 Pro` in the right places, and contains a few xml files to overlay over
the base kirin970-common system. That's about all I know so far! If you work
out more, let me know :)
