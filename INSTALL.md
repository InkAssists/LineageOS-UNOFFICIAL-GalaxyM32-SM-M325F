# First install — LineageOS 24.0 for Galaxy M32 4G, India SKU (SM-M325F)

Package: `lineage-24.0-20261124-UNOFFICIAL-m32dd.zip`
SHA-256: `a6a78eba6e8072b8a2b11e1e193eb0644af4eb8b7a3a3635866c38d54f4703fb`
Size: 1,367,520,863 bytes

Do **not** use the `m32` zip on this phone. Both packages pass the same assert, so a
wrong-SKU zip will not be rejected — it will install and then boot-loop on mismatched
dtbo/vbmeta.

Preconditions: bootloader unlocked, TWRP 3.6.11 running, USB debugging works in TWRP
(`adb devices` must show the device in state `recovery`, not `device`).

---

## 0. Sanity checks

```
adb devices -l
adb shell getprop ro.boot.bootloader
```

Note: `model:` reported by TWRP is not a reliable SKU indicator — confirm SM-M325F in stock Settings, on the device label, or in Download Mode. Verify the ZIP hash separately.

## 1. Initialise the device for custom ROMs (first time only)

This is the step that has to happen once, while a Samsung stock system is still on
the phone. It disables the auto-restore of stock recovery and Samsung's security
services that otherwise fight the custom ROM. Skip it if the phone already ran a
custom ROM before.

Before starting, back up your data off-device. Bootloader unlocking and Format Data erase user data. Use only an SM-M325F-compatible TWRP and vbmeta setup; do not use EU recovery or firmware files. Never relock the bootloader while running a custom ROM.

Check whether your TWRP already ships the installer:

```
adb shell which multidisabler
```

**If it prints a path** (Mujammil's TWRP does):

```
adb shell multidisabler
```

**If it prints nothing**, flash the zip instead:

```
adb push multidisabler-samsung-3.1.zip /tmp/
adb shell twrp install /tmp/multidisabler-samsung-3.1.zip
```

Either way you must see the script finish without errors. Reboot to recovery once
afterwards (`adb reboot recovery`) so /data is usable again.

## 2. Format data (required for a clean install)

```
adb shell twrp format data
```

Use `format data`, **not** `wipe data` — a plain wipe leaves /data/media and the
encryption metadata in place, and the new ROM will fail to boot.

Everything in `/data` and `/sdcard` is gone after this step. Copy photos and downloads
off the phone first. Nothing you push before this step survives it.

## 3. Push the ROM

```
adb shell mkdir -p /data/local/tmp
adb push lineage-24.0-20261124-UNOFFICIAL-m32dd.zip /data/local/tmp/rom.zip
adb shell md5sum /data/local/tmp/rom.zip
```

Compare the md5 with `md5sum lineage-24.0-20261124-UNOFFICIAL-m32dd.zip` on your PC.
Push to `/data/local/tmp`, never to `/sdcard` (the FUSE layer can report success while
silently dropping data).

## 4. Install

```
adb shell twrp install /data/local/tmp/rom.zip
```

Expected tail of the output:

```
Patching system image unconditionally...
Patching vendor image unconditionally...
Patching product image unconditionally...
Patching odm image unconditionally...
Patching firmware images...
script succeeded: result was [1.000000]
```

## 5. Reboot

```
adb shell sync
adb shell twrp reboot
```

First boot takes about 5 minutes and adb is not available during it. Leave the phone
alone until the setup wizard appears.

## 6. Verify the installed build

After setup, re-enable USB debugging and run `adb shell getprop ro.build.version.incremental`. It must return `1795557904`. A successful TWRP install message alone does not prove the new build booted.
