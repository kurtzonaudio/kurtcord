# Kurtcord

A custom Discord desktop client built for voice. High fidelity audio, call guards that keep the chaos out, and real control over your microphone, camera, and screen share.

Current release: **2.8.0**

## Download

Everything is on the [latest release page](https://github.com/kurtzonaudio/kurtcord/releases/latest).

| File | What it is |
| --- | --- |
| `Kurtcord-2.8.0-Setup.exe` | Installer. Installs Kurtcord to your user folder and adds a Start menu entry. |
| `Kurtcord-2.8.0-Portable.exe` | Portable. Run it from any folder or a USB drive, nothing gets installed. |
| `Kurtcord-2.8.0-Setup.exe.zip` | The installer and the license in one zip, for passing around. |

## What Kurtcord is

Kurtcord is a Windows desktop client for Discord. It runs Discord's own web app inside an Electron shell and adds a custom voice pipeline, extra audio and video controls, and a set of built-in plugins on top. Your account, servers, friends, and messages are the same as anywhere else. Kurtcord changes how your voice sounds and how a call behaves for you.

## 2.8.0

2.8.0 adds full control over what you send: your screen share, your camera, and every Voice & Video setting, all live.

**StreamEnhancer.** The StreamEnhancer plugin now ships inside Kurtcord's Voice & Video page. It tunes the screen share you send and the streams you watch:

- Screen share quality: resolution up to 8K, frame rate up to 240 fps, video bitrate and a minimum bitrate floor, codec choice (Auto, AV1, VP9, H.264), one high quality layer, adaptive max quality, HDR capture, keyframe interval, and an SDP bitrate boost.
- Stream previews: scale, saturation, contrast, upload resolution, JPEG quality, refresh and retry timing, a custom preview URL, and an optional upload filter.
- Viewer controls: a resize slider in the stream menu and outgoing video filters.
- Stream telemetry: outgoing and observed stream stats on demand.

**Camera fixes.** The camera path is rebuilt:

- Cameras render natively again. The gray tiles some calls showed are gone.
- When no video filter is set, the camera passes through untouched instead of being re-encoded through a canvas. That was capping the camera at a fraction of its capture rate.
- The camera encoder now uses a constant bitrate target from the Camera video bitrate slider instead of Discord's adaptive 150-400 kbps ladder, and it no longer inherits the screen share resolution.
- Camera presets run from 720p60 up to 2160p60, plus 1080p120, with custom resolution, frame rate, and bitrate.

**Voice & Video quick panel.** The side panel now mirrors the whole Voice & Video page in a smaller panel:

- Voice: microphone, second and third microphone with their own device and volume, speaker, microphone and speaker volume, input profile (Voice Isolation, Studio, Custom), input sensitivity, noise suppression, echo cancellation, auto gain control, and push to talk.
- Camera: always preview video, camera device, Advanced Camera Controls, quality preset, and Advanced Hardware Acceleration.
- Streaming: stream previews.
- Soundboard: soundboard volume.
- Advanced: reset all Voice & Video settings.
- Stream Enhancer: the full plugin panel.
- The removed Opus codec, Spatial Audio, and VST sections are gone for good.

**Optimizations.** Kurtcord and StreamEnhancer telemetry are off, the duplicate VU meter implementation is disabled (the built-in meters stay), React DevTools is off, and the second message logger no longer caches server messages in memory.

The 2.7.9 voice output engine and the 2.7.7 account safety work are included.

## 2.7.9

2.7.9 rebuilt voice output so a call stays clean and audible the whole time. It fixed doubled or hollow voices (two playback paths per person, now one shared 48 kHz engine with the hidden media element silenced) and voice cutting out after switching channels (the engine now reattaches every output when its stream changes, reapplies your output device after a device reset, and rebuilds itself if its clock stalls). Per-user volume, local mutes, the live voice meter, and 96 kHz incoming voice all keep working.

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

- Camera presets from 720p60 to 2160p60, plus 1080p120 and custom resolution, frame rate, and bitrate. The camera sends a constant bitrate you set, not a value Discord picks for you.
- Advanced Camera Controls remove Discord's 720p cap, and Advanced Hardware Acceleration forces GPU rasterisation, zero copy, and accelerated video decode.
- Screen share resolution, frame rate, bitrate, and a choice between quality and latency mode, all from the StreamEnhancer panel in Voice & Video.

## Privacy

- Every setting lives on your machine.
- No analytics and no telemetry. Kurtcord has no backend server.
- The client reports a standard Chrome fingerprint.

## Built-in mods

Kurtcord ships with the Equicord plugin suite and the Testcord plugins, ready out of the box. The voice guards, the voice state fix, StreamEnhancer, and the rest of the Testcord set are already installed. Plugins can be toggled and configured from the client's own plugin settings.

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
