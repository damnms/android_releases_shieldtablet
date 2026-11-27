# LineageOS for the NVIDIA SHIELD Tablet

Unofficial LineageOS 20 (Android 13) and 18.1 (Android 11) for the NVIDIA
SHIELD Tablet K1 (shieldtablet). Downloads are on the
[releases page](https://github.com/damnms/android_releases_shieldtablet/releases)
(tags `shieldtablet-20.0-…` and `shieldtablet-18.1-…`).

## LineageOS 20

- LineageOS security backports up to the build date; Settings shows
  patch level 2026-02-01. Kernel 3.10 (with an eBPF backport) and NVIDIA
  blobs get no patches
- SELinux enforcing, signed with private release keys, `userdebug`
- Bootloader `4.00.2016.04` required (the zip checks it)
- `/data` with file-based encryption
- Tested on the Wi-Fi model; the LTE model is not tested
- Fits both partition layouts (system partition 1.25 or 2 GiB). Apps
  are compiled on the tablet after installing, the first minutes are
  slower

### Working

- Display, touch, sensors and rotation, auto-brightness
- Wi-Fi 2.4 and 5 GHz including DFS channels
- Bluetooth, speaker, microphone, GPS
- Rear and front camera (photo, 1080p30 video), hardware video decoding
  and encoding, screen recording
- HDMI (mini-HDMI): picture and sound, plugging in and out while running,
  web video at normal speed
- Battery, charging over USB, deep sleep
- Encryption with screen lock; motion sensors in the browser

### Known issues

- WPA3-only networks and networks with PMF required cannot be joined:
  the Wi-Fi firmware has no SAE and no PMF; the Wi-Fi list marks them as
  not supported. WPA2 and WPA2/WPA3 transition work
- 5 GHz and DFS channels need a country code, taken from nearby access
  points (two or more). The first connection after boot can land on
  2.4 GHz; Wi-Fi then reconnects once
- The European Wi-Fi firmware tables have no channels 149–165, so
  networks there (often phone hotspots) are not found once the country
  code is set. To keep the worldwide table (36–48 and 149–165, no DFS):
  `adb shell settings put global wifi_country_code_from_scan 0`, then
  reboot; undo with `settings delete global wifi_country_code_from_scan`
- HDMI sound starts about 5 s after the screen turns on
- The camera offers no video stabilization (it halved the frame rate)
- Moving between 18.1 and 20 needs *Format data* in both directions
  (18.1 encrypts the whole partition, 20 per file)

### Not tested

3.5 mm headphones, Bluetooth audio, hotspot with a client, Wi-Fi Direct,
USB tethering, charging with a power adapter, standby overnight, stylus,
microSD, MTP, USB OTG, Google apps, the LTE model.

### Installation

**Update from an earlier 20 release:** Settings → System → Updates, or
`adb sideload` in the recovery. No data wipe. The recovery cannot read
the encrypted `/data`, so it installs only over ADB.

**From 18.1 or another ROM:**

1. Unlock the bootloader: `fastboot oem unlock` (erases the tablet)
2. `fastboot flash recovery recovery-lineage-20.0-….img` from the
   release, boot into the recovery
3. Factory reset → Format data
4. Apply update → Apply from ADB: `adb sideload lineage-20.0-….zip`
5. Reboot system now (`adb reboot` from the recovery boots the recovery
   again)

## LineageOS 18.1

- LineageOS security backports up to the build date; Settings shows
  patch level 2024-02-05. Kernel 3.10 and NVIDIA blobs get no patches
- SELinux enforcing, signed with private release keys, `userdebug`
- Bootloader `4.00.2016.04` required (the zip checks it)
- Tested on the Wi-Fi model; the LTE model is not tested

### Working

- Display, touch, stylus, sensors and rotation, auto-brightness
- Wi-Fi 2.4 and 5 GHz, Wi-Fi Direct, hotspot, USB tethering
- Bluetooth (A2DP with LDAC), audio, 3.5 mm headphones, microphone, GPS
- Rear and front camera (1080p30 video), hardware video decoding and
  encoding (1080p60), screen recording
- Battery, charging, deep sleep, microSD, MTP and PTP, USB OTG
- HDMI (mini-HDMI): picture and sound, plugging in and out while running
- Encryption, screen lock, Google apps (Android 11, ARM)

### Known issues

- WPA3-only networks and networks with PMF required cannot be joined:
  the Wi-Fi firmware has no SAE and no PMF. WPA2 and WPA2/WPA3
  transition work
- 5 GHz networks on DFS channels (52–144) appear only after two nearby
  access points report the same country. While connected to such a
  network, Wi-Fi Direct fails
- Encryption can only be removed with *Format data*, which erases all
  data
- Browser: sounds of web pages (WebAudio) have small gaps, on HDMI and
  on the speaker; video sound is clean
- Not tested: headset microphone and button

### Installation

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

`shieldtablet.json` (LineageOS 18.1) and `shieldtablet-20.0.json`
(LineageOS 20) are the update lists for the LineageOS Updater, read
through `lineage.updater.uri`. Each points to the zip of the latest
release of its version; pre-releases are not listed. 18.1 reads only
its own file and is not offered 20.

Sources: [local manifest](https://github.com/damnms/android_local_manifests_shieldtablet),
[device tree](https://github.com/damnms/android_device_nvidia_shieldtablet),
[kernel](https://github.com/damnms/android_kernel_nvidia_shield).
