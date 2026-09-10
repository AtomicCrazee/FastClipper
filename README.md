<img width="962" height="849" alt="Screenshot 2026-09-10 114034" src="https://github.com/user-attachments/assets/89f0fcee-663e-462c-b62e-d49f41415f6a" />

# MP4 Audio & Game Clip Creator

A lightweight Windows utility for trimming MP4 gameplay footage, producing Discord-ready clips, and extracting WAV audio.

## Features

- Visual MP4 preview with timeline scrubbing
- In/out points using buttons or the `I` and `O` keys
- Frame stepping with the left and right arrow keys
- Fast stream-copy exports with no quality loss
- Frame-accurate H.264 and H.265 exports
- Custom source, 24, 30, 60, or user-entered frame rates
- Source-matching resolution, frame rate, codec family, and approximate bitrate
- Discord export with a conservative 7.75 MB target and enforced 8 MB maximum
- Detection of multiple audio tracks
- Keep audio tracks separate, mix them, or keep only the first track
- Lossless 32-bit float WAV extraction

## Requirements

- Windows 10 or Windows 11
- Windows PowerShell 5.1 or newer
- Microsoft .NET Framework 4.x with WPF (included with supported Windows versions)
- FFmpeg and FFprobe

Run `Setup Prerequisites.bat` once to install FFmpeg through Windows Package Manager. FFmpeg is intentionally not committed because each executable is larger than GitHub's 100 MB per-file repository limit.

If `winget` is unavailable, download a Windows FFmpeg build manually and either:

1. Put `ffmpeg.exe` and `ffprobe.exe` beside `MP4-to-WAV.exe`, or
2. Add their `bin` folder to the Windows `PATH`.

## Privacy and security

The editor runs locally. It has no telemetry, analytics, advertising, account integration, API keys, or network requests. Selected media paths are passed only to the local FFmpeg/FFprobe processes and are not transmitted anywhere.

`Setup Prerequisites.bat` is optional and is the only component that can access the network; it asks Windows Package Manager to install the public `Gyan.FFmpeg` package. Every distributable source file is included for inspection. See `SECURITY.md` for the release audit and trust notes.

## Running

1. Run `Setup Prerequisites.bat` once.
2. Double-click `MP4-to-WAV.exe` or `Launch App.bat`.
3. Drop an MP4 into the window.
4. Set the In and Out points.
5. Choose either the **WAV Audio** or **Game Clip** tab and export.

### Export modes

- **Fast source copy:** fastest and lossless; the beginning can move to the nearest video keyframe.
- **Match source:** frame-accurate re-encode using source-like settings.
- **Custom:** frame-accurate H.264/H.265 export with an adjustable frame rate.
- **Discord:** two-pass encode that balances resolution, video bitrate, and selected audio tracks to remain below 8 MB.

H.264 is recommended for the widest Discord and browser compatibility. H.265 usually creates smaller files, but playback support varies.

## Building the launcher

The compiled launcher is included. To rebuild it from `Launcher.cs`, run `Build EXE.bat`. It uses the C# compiler bundled with Microsoft .NET Framework and does not require Visual Studio.

The launcher and PowerShell script must remain in the same folder.

## Repository contents

- `MP4-to-WAV.ps1` — application and FFmpeg export logic
- `Launcher.cs` — source for the console-free Windows launcher
- `MP4-to-WAV.exe` — compiled launcher
- `Build EXE.bat` — rebuilds the launcher
- `Setup Prerequisites.bat` — installs FFmpeg using `winget`
- `Launch App.bat` — checks dependencies and starts the application
- `SECURITY.md` — privacy, trust, and security information

FFMPEG
------
The program checks for ffmpeg.exe beside the script first, then checks the Windows
PATH. For a portable setup, place ffmpeg.exe in this folder.
