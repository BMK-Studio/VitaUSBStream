# VitaUSBStream Audio & Video over USB — Beta Version

<img width="960" height="544" alt="VitaUSBStream" src="https://github.com/user-attachments/assets/d4411d1b-a85c-4720-9619-68da1c93ccfd" />

**VitaUSBStream Audio & Video over USB** allows you to stream both the **video and audio** from a PS Vita to a computer using a single USB cable.

The project is based on the original **UDCD-UVC** plugin, which streamed video only. This version adds USB Audio Class support so the console audio can be captured directly in OBS without using a 3.5 mm audio cable.

> This version is still in **beta**. Video and most audio sources work correctly, but a few specific cases still need to be fixed.

## Features

- USB video streaming through UVC;
- USB audio streaming through UAC;
- compatibility with OBS and software supporting standard USB video and audio devices;
- screen streaming with Screen On and Screen Off modes;
- front and rear PS Vita camera streaming;
- camera switching using the L/R buttons;
- native camera streaming at **640×480 at 60 FPS** or **480×272 at 30 FPS**;
- optional PS Vita microphone streaming while using a camera;
- automatic display restoration when USB is disconnected in Screen Off mode;
- automatic Screen Off restoration when USB is reconnected;
- standby and wake-up support while streaming;
- Shell theme music and system sounds;
- LiveArea sound effects;
- game audio;
- application and homebrew audio;
- automatic detection of the active process and audio port;
- support for 44.1 kHz and 48 kHz audio sources;
- automatic conversion to **48 kHz, stereo, 16-bit PCM**;
- simultaneous mixing of multiple audio sources;
- manual streaming control through a PS Vita application;
- automatic USB recovery after stopping a stream or encountering an error;
- automatic installation and updating of the VitaUSBStream kernel plugin;
- Sony PAF-based interface with active mode and source indicators;
- no WAV or PCM files recorded to the memory card.

## Installation

1. Remove any old configuration lines loading a previous UDCD-UVC audio plugin or audio bridge.
2. Copy `vpk file to your PSVita/TV`
3. Install with VitaTweakBox or VitaShell and launch it

## Usage

1. Connect the PS Vita to the PC with a USB data cable.
2. Open **VUS Control**.
3. Enable streaming with the button shown in the application.
4. Open OBS.

In OBS, add:

- a **Video Capture Device** source for the PS Vita video;
- an **Audio Input Capture** source for the PS Vita USB audio device.

Video and audio appear as separate sources in OBS, but both are transmitted through the same USB cable.

## Supported Audio Sources

The current version can automatically capture:

- Shell background music;
- navigation and LiveArea sounds;
- game audio;
- application audio;
- players using a BGM audio port or another active SceAudio port.

Silent ports are ignored. When a port starts producing audio, it is automatically selected and streamed to the PC.

## Known Issues

- some MP3 files played with Sony's native Music application may be distorted over USB;
- fast-forwarding or rewinding in VitaTweakBox may produce crackling sounds;
- depending on the PC, Windows or OBS may keep an older USB device configuration cached after updating the plugin. Restarting the PC or removing the old device may be required.

Normal playback from the Shell, LiveArea, tested games, and tested applications works correctly.

## Improved Compatibility with Other Plugins

The original UDCD-UVC activated some USB functions as soon as the kernel plugin started. This could cause conflicts with other plugins or services using USB, including MTP, and prevent certain plugins from working correctly at the same time.

This version now uses a passive startup mode: the plugin can remain loaded under *KERNEL in config.txt without immediately taking control of USB. Audio and video streaming are enabled only when requested through the Vita-USB-Stream Control application, and the previous USB services are restored when streaming is disabled.

Thanks to this behavior, the plugin can normally remain permanently enabled in config.txt, with much better compatibility with other plugins. Since this is still a beta version, uncommon plugin-specific conflicts may still be reported.

## Credits

[xerpi](https://github.com/xerpi)

- UDCD-UVC Code;

## Source Code

The source code will be made available after I’ve had a well-deserved break. Once I’m back from vacation, I’ll clean it up and publish it.
