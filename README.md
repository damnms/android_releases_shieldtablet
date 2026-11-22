# LineageOS 18.1 for the NVIDIA SHIELD Tablet

Unofficial LineageOS 18.1 (Android 11) for the NVIDIA SHIELD Tablet K1
(shieldtablet). Downloads are on the
[releases page](https://github.com/damnms/android_releases_shieldtablet/releases).

- LineageOS security backports up to the build date; Settings shows
  patch level 2024-02-05. Kernel 3.10 and NVIDIA blobs get no patches
- SELinux enforcing, signed with private release keys, `userdebug`
- Bootloader `4.00.2016.04` required (the zip checks it)
- Tested on the Wi-Fi model; the LTE model is not tested

## Working

- Display, touch, stylus, sensors and rotation, auto-brightness
- Wi-Fi 2.4 and 5 GHz, Wi-Fi Direct, hotspot, USB tethering
- Bluetooth (A2DP with LDAC), audio, 3.5 mm headphones, microphone, GPS
- Rear and front camera (1080p30 video), hardware video decoding and
  encoding (1080p60), screen recording
- Battery, charging, deep sleep, microSD, MTP and PTP, USB OTG
- Encryption, screen lock, Google apps (Android 11, ARM)

## Known issues

- WPA3-only networks and networks with PMF required cannot be joined:
  the Wi-Fi firmware has no SAE and no PMF. WPA2 and WPA2/WPA3
  transition work
- 5 GHz networks on DFS channels (52–144) appear only after two nearby
  access points report the same country. While connected to such a
  network, Wi-Fi Direct fails
- Encryption can only be removed with *Format data*, which erases all
  data
- HDMI: fix in pre-release 20261122, waiting for a test with a monitor
- Not tested: headset microphone and button

## Installation

**Update:** Settings → System → Updates, or `adb sideload` in the
recovery. No data wipe.

**From another ROM:**

1. Unlock the bootloader: `fastboot oem unlock` (erases the tablet)
2. `fastboot flash recovery recovery-….img` from the release, boot into
   the recovery
3. Factory reset → Format data
4. Apply update → Apply from ADB: `adb sideload lineage-….zip`
5. Optional: Google apps for Android 11 (ARM), in the same session
6. Reboot system now (`adb reboot` from the recovery boots the recovery
   again)

## OTA updates

`shieldtablet.json` is the update list for the LineageOS Updater, read
through `lineage.updater.uri`. It points to the zip of the latest
release; pre-releases are not listed.

Sources: [local manifest](https://github.com/damnms/android_local_manifests_shieldtablet),
[device tree](https://github.com/damnms/android_device_nvidia_shieldtablet),
[kernel](https://github.com/damnms/android_kernel_nvidia_shield).
