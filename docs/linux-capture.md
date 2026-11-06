# Linux capture and migration checks

VitaUSBStream exposes USB video (UVC) and USB audio (UAC). Linux applications normally see
these as separate capture devices. This checklist covers the PC side after following the
installation instructions in the main README; it does not flash the Vita or replace its
complete taiHEN configuration.

## Before replacing an older UDCD-UVC setup

1. Save a copy of your current Vita plugin configuration and the old plugin files.
2. Follow VitaUSBStream's installation steps to remove the older UDCD-UVC entries. Preserve
   unrelated storage plugins and configuration, including SD2Vita/YAMT entries when present.
3. Reboot after changing kernel plugins, then open VUS Control and explicitly enable
   streaming. A loaded plugin alone does not mean a stream is active.
4. Connect using a USB data cable. Keep the configuration backup until both video and audio
   have been tested.

## Find the video device

Install `v4l-utils` using your distribution's package manager. On Arch Linux/CachyOS:

```sh
sudo pacman -Syu v4l-utils
```

With streaming enabled, list capture devices and their persistent paths:

```sh
v4l2-ctl --list-devices
ls -l /dev/v4l/by-id/
```

Identify the Vita entry rather than assuming it is `/dev/video0`. Other cameras and loopback
video devices may already use that number. Some devices expose more than one node; inspect
the capabilities and available formats for the candidate node:

```sh
v4l2-ctl --device /dev/videoN --all
v4l2-ctl --device /dev/videoN --list-formats-ext
```

Replace `/dev/videoN` with the path you identified. Select a format/frame rate advertised by
that device; do not assume every streaming mode supports the camera-mode resolutions listed
in the main README.

## Add video and audio in OBS

1. Add **Video Capture Device (V4L2)** and select the identified Vita device. Start with its
   default settings, then choose from the formats it advertises if needed.
2. Add **Audio Input Capture** and select the Vita USB audio source. Do not select a desktop
   output monitor just because it has an active meter.
3. If no USB audio input appears, verify that VUS Control has streaming enabled and reconnect
   the USB device. With PulseAudio or PipeWire-Pulse, list current sources:

   ```sh
   pactl list short sources
   ```

4. Check that the audio meter responds to an audible game or shell sound. Make a short test
   recording and play it back to check both picture and sound. A silent game/menu is not an
   audio-capture test.

## If the device exists but capture fails

- Close other camera/capture applications that may have the video node open, then retry.
- For a permission error, inspect `ls -l /dev/videoN` and, where available,
  `getfacl /dev/videoN`. Check your desktop session's device-access policy; do not make video
  devices world-writable or run OBS as root.
- For a sandboxed OBS installation, check its device permissions. Compare with a native
  installation when diagnosing a sandbox-specific problem; avoid blindly granting access to
  the whole host filesystem.
- After a plugin update or streaming-mode change, stop the OBS source, reconnect the device,
  and enumerate its formats again. A saved OBS source can refer to an old device selection.

When reporting a problem, include the plugin/control-app version, Linux distribution,
OBS installation type, chosen capture device and advertised format, and whether video,
audio, or both fail. Review logs for serial numbers and local paths before sharing them.
