# Lineage for Huawei Mate 10 Pro

This is a very early work in progress to get Lineage OS working on a Huawei
Mate 10 Pro, codename `blanc`. It builds on the [work of
LuK1337](https://forum.xda-developers.com/honor-view-10/development/rom-lineageos-15-1-t3753000),
which gets Lineage OS working on Huawei Honor View 10 (berkeley) and Huawei P20 Pro (charlotte).

## Building

The build is similar to the one [described here](https://wiki.lineageos.org/devices/nash/build), with the following differences:
1. Where those instructions say `nash`, use `blanc`,
2. Ensure that `.repo/local_manifests/roomservice.xml` contains project entries for kirin970-common and blanc:
```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
    <project name="LineageOS/android_device_huawei_kirin970-common" path="device/huawei/kirin970-common" remote="github" revision="cm-15.1" />
    <project name="LineageOS/android_device_huawei_blanc" path="device/huawei/blanc" remote="github" revision="cm-15.1" />
</manifest>
```
3. Skip the `breakfast` step, as this is only for officially supported devices, and we're not there yet. Instead do the following:
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

If your build fails with an error like this:
```
FAILED: /home/me/android/lineage/out/target/product/blanc/obj/APPS/framework-res_intermediates/package-res.apk
/bin/bash /home/me/android/lineage/out/target/product/blanc/obj/APPS/framework-res_intermediates/package-res.apk.rsp
/home/me/android/lineage/out/target/product/blanc/obj/APPS/framework-res_intermediates/flat-res/device/huawei/kirin970-common/overlay-lineage/frameworks/base/core/res/res/values_lineage_config.arsc.flat: error: resource array/config_oemFastChargerStatusPaths does not override an existing resource.
/home/me/android/lineage/out/target/product/blanc/obj/APPS/framework-res_intermediates/flat-res/device/huawei/kirin970-common/overlay-lineage/frameworks/base/core/res/res/values_lineage_config.arsc.flat: note: define an <add-resource> tag or use --auto-add-overlay.
error: failed parsing overlays.
[ 53% 59651/112185] AAPT2 compile /home/me/android/lineage/out/target/product/blanc/obj/APPS/Dialer_intermediates/flat-res/package...lator/res/values-ne_strings.arsc.flat <- packages/apps/Dialer/java/com/android/dialer/enrichedcall/simulator/res/values-ne/strings.xml
ninja: build stopped: subcommand failed.
08:44:32 ninja failed with: exit status 1

#### failed to build some targets (44:13 (mm:ss)) ####
```
Then you will need to get [this
change](https://review.lineageos.org/#/c/LineageOS/android_frameworks_base/+/207583/),
which enables fast charging. To check it out, run:
```bash
cd ~/android/lineage/frameworks/base
git fetch https://github.com/LineageOS/android_frameworks_base refs/changes/83/207583/10 && git checkout FETCH_HEAD
```

If your build fails with an error like this:
```
FAILED: /home/cha748/android/lineage/out/target/product/blanc/obj/STATIC_LIBRARIES/libedify_intermediates/lexer.cpp
/bin/bash -c "prebuilts/misc/linux-x86/flex/flex-2.5.39 -o/home/cha748/android/lineage/out/target/product/blanc/obj/STATIC_LIBRARIES/libedify_intermediates/lexer.cpp bootable/recovery/edify/lexer.ll"
flex-2.5.39: loadlocale.c:130: _nl_intern_locale_data: Assertion `cnt < (sizeof (_nl_value_type_LC_TIME) / sizeof (_nl_value_type_LC_TIME[0]))' failed.
[  4% 5424/112185] Building with Jack: /home/cha748/android/lineage/out/target/common/obj/JAVA_LIBRARIES/libphonenumber_intermediates/classes.jack
ninja: build stopped: subcommand failed.
00:50:24 ninja failed with: exit status 1
```

Then you need to do this before the `brunch blanc` step:
```
export LC_ALL=C
```

## Getting it Running
Once you have built your system.img, check out [this guide for installing it](https://forum.xda-developers.com/mate-10/how-to/guide-treble-compatible-rom-t3761927).

## Development
This repo is pretty simple. It just defines a few strings, such as `blanc`, and
`Mate 10 Pro` in the right places, and contains a few xml files to overlay over
the base kirin970-common system. That's about all I know so far! If you work
out more, let me know :)
