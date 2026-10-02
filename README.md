# Kurtcord

A custom Discord desktop client built for voice. High fidelity audio, call guards that keep the chaos out, and real control over your microphone, camera, and screen share.

Current release: **2.7.8**

## Download

Everything is on the [latest release page](https://github.com/kurtzonaudio/kurtcord/releases/latest).

| File | What it is |
| --- | --- |
| `Kurtcord-2.7.8-Setup.exe` | Installer. Installs Kurtcord to your user folder and adds a Start menu entry. |
| `Kurtcord-2.7.8-Portable.exe` | Portable. Run it from any folder or a USB drive, nothing gets installed. |
| `Kurtcord-2.7.8-Setup.exe.zip` | The installer and the license in one zip, for passing around. |

## What Kurtcord is

Kurtcord is a Windows desktop client for Discord. It runs Discord's own web app inside an Electron shell and adds a custom voice pipeline, extra audio and video controls, and a set of built-in plugins on top. Your account, servers, friends, and messages are the same as anywhere else. Kurtcord changes how your voice sounds and how a call behaves for you.

## 2.7.8

2.7.8 fixes a bug where some calls played every voice twice, once through Discord's normal audio path and once through the client's own audio guard. The guard no longer opens a second playback path; it only listens to the call for the speaking rings, so it cannot double or echo voice audio anymore.

Also in this release:

- The voice quick panel no longer shows the removed codec picker, Spatial Audio, or the VST sections. It now matches the Voice & Video page.
- The quick panel text is light on a solid dark background, so labels and descriptions stay readable in any Discord theme.
- Audio Bitrate and Sample Rate moved up in Voice & Video, directly under Opus Complexity, and the Sample Rate note travels with its slider.
- The Band width (Q) slider in the EQ now matches the layout of the other sliders.

## 2.7.7 and account safety

2.7.7 fixes the issue that could get accounts flagged for using Kurtcord.

The previous build ran with a Chromium automation flag set, which made every page report `navigator.webdriver = true`. Discord and its captcha provider read that flag as "this browser is controlled by automation", so friend requests, server joins, and captchas could be scored as bot activity and end in a suspension. 2.7.7 removes it. The automation flag is disabled, remote debugging is off by default, and the client presents a fully consistent Chrome fingerprint: user agent, client hints, and the native user agent data all agree, with no JavaScript spoofing for a fingerprinting script to catch.

Users are no longer flagged for using Kurtcord.

## Voice

- 96 kHz HD voice with stereo, when both ends support it.
- Bitrate, sample rate, frame time, complexity, FEC, DTX, and CBR controls for Opus.
- Bypass system input processing to send your microphone raw.
- Microphone gain, plus a second and third microphone input, each with its own device and gain.
- Reverb and EQ built into the mic chain.

## Voice guards

Three guards watch the call and mute locally, so you keep hearing everyone else.

- **StereoGuard** mutes anyone whose audio is not fully mono: panning, stereo width, panning around, and reverb tails all push the score up. Centered mono voice is never touched.
- **MicSpamGuard** mutes anyone who gets too loud or spams their mic. You set the threshold, the sensitivity, and how long until the mute lifts.
- **MicSpamGuard DH** balances voice volumes automatically and protects the call from sustained extreme loudness, with a 200% ceiling on quiet voice boost.

Each guard keeps a "never mute" list. To add someone to it, right click their name anywhere in Discord and pick **Allow through Stereo Guard** or **Allow through Mic Spam Guard** from the menu, directly under the volume slider. The same people can be managed from each plugin's settings, and the two stay in sync. The menu entry flips to "Stop allowing through ..." once someone is on the list.

## Camera and screen share

- Camera presets from 720p60 to 2160p60, plus custom resolution, frame rate, and bitrate.
- HD video upload.
- Screen share resolution, frame rate, bitrate, and a choice between quality and latency mode.

## Privacy

- Every setting lives on your machine.
- No analytics and no telemetry. Kurtcord has no backend server.
- The client reports a standard Chrome fingerprint.

## Built-in mods

Kurtcord ships with the Equicord plugin suite and the Testcord plugins, ready out of the box. The voice guards, the voice state fix, and the rest of the Testcord set are already installed. Plugins can be toggled and configured from the client's own plugin settings.

## How it works

- An Electron shell loads Discord's web app and keeps it patched at runtime.
- The mic chain is built in the Web Audio graph, and the session description for each call is tuned before it goes out.
- The camera and screen share paths are adjusted at the WebRTC layer.
- The client checks GitHub for updates on startup and every few hours, using this repository's releases and `version.json`.
- Kurtcord is closed source. This repository carries the client description, the license, the update manifest, and the release downloads. No source code is published or attached to releases.

## Requirements

- Windows 10 or Windows 11, 64 bit.
- A Discord account.

## Updating

Run the newest installer or portable from the releases page. Your settings stay where they are. The client will also tell you when a new release is available.

## FAQ

**Will my account get banned for using Kurtcord?**
The client side cause of the flags is fixed in 2.7.7, and the client now behaves like a normal Chrome session. Discord can still act on behavior and environment that no client controls: mass friend requests, mass server joins, VPN or proxy addresses, and brand new accounts. Third party clients are also not officially supported by Discord, so use Kurtcord at your own risk.

**Does Kurtcord talk to any Kurtcord server?**
No. There is no Kurtcord backend. The client talks to Discord, and to GitHub for updates.

**Does it change anything for other people in the call?**
No. The guards mute locally on your side, and your voice settings only change what you send.

**Where are my settings stored?**
In your own Windows user folder, under `%APPDATA%\Kurtcord`.

## License

MIT. Copyright (c) 2026 Kurtzon (Kurtzon Audio). See [LICENSE](LICENSE).

Built and maintained by Kurtzon.
