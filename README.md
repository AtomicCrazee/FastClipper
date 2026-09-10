<img width="962" height="849" alt="Screenshot 2026-09-10 114034" src="https://github.com/user-attachments/assets/89f0fcee-663e-462c-b62e-d49f41415f6a" />

# FastClipper
A vibe coded game clipper that just works
=============================

1. Install FFmpeg once, if it is not already installed:
   winget install Gyan.FFmpeg

2. Double-click "Build EXE.bat" once. This uses the compiler included with
   Windows to create MP4-to-WAV.exe without installing Visual Studio.

3. Launch MP4-to-WAV.exe. It opens without a console window and can be pinned
   to the taskbar. Keep the EXE and MP4-to-WAV.ps1 together in this folder.

4. Drop an MP4 into the window or choose one with the file picker.

5. Scrub with the timeline. Press I to set the In point and O to set the Out
   point. Space toggles playback. Left Arrow and Right Arrow move the playhead
   backward or forward by one source-video frame. The buttons can also set both
   trim points. The picture updates continuously as the timeline is scrubbed.

6. Use either export tab:
   - WAV Audio exports the first audio stream as lossless 32-bit float WAV.
   - Game Clip exports the selected range as an MP4.

GAME CLIP MODES
---------------
Fast source copy is the quickest option. It copies the original video and audio
without re-encoding. Because compressed video can only be copied cleanly from a
keyframe, the opening frame can be slightly earlier than the selected In point.

Match source makes a frame-accurate clip using the source resolution, frame rate,
H.264/H.265 codec family, and approximate source video bitrate. Use this when you
want the clip to behave like the source but need exact trim points.

Custom lets you select H.264 or H.265 and enter any positive frame rate. "Source"
keeps the original frame rate. H.264 has the widest playback compatibility; H.265
usually produces a smaller file but can take longer to encode.

Discord (under 8 MB) uses two-pass encoding and a conservative 7.75 MB target. It
automatically assigns the available bitrate and scales very low-bitrate clips down
when necessary. H.264 is the safest Discord choice. Very long selections may become
visibly soft because the entire clip must fit into the fixed size limit.

AUDIO TRACKS
------------
The source summary shows how many audio tracks were detected. Game clips can keep
all tracks separately (useful for game/voice tracks), mix every track together for
simple playback, or keep only the first track. The Discord size budget includes all
selected audio tracks.

Enable "Show processing console" inside the editor to reveal FFmpeg's detailed
processing log. It is hidden by default.

PREVIEW PERFORMANCE
-------------------
When a video is opened, the editor creates a temporary 720p all-intra preview
with audio. It first attempts NVIDIA CUDA decoding, CUDA scaling and NVENC
encoding. If the installed NVIDIA driver is too old for the FFmpeg NVENC API, it
uses CUDA decoding/scaling with an ultrafast CPU encoder. A fully CPU-based path
is the final fallback. Expected GPU-probing failures are hidden automatically.
The temporary preview is deleted when the editor closes and is never used for
the final WAV export.

QUALITY
-------
The utility removes the video and decodes the first audio stream to uncompressed
32-bit floating-point PCM. It preserves the source sample rate and channel layout
and performs no normalization, filtering, or deliberate resampling.

Most MP4 files contain compressed AAC audio. Converting AAC to WAV cannot restore
detail that was already discarded when the AAC was created, but this process adds
no further lossy compression. A WAV will be considerably larger than the MP4 audio.

FFMPEG
------
The program checks for ffmpeg.exe beside the script first, then checks the Windows
PATH. For a portable setup, place ffmpeg.exe in this folder.
