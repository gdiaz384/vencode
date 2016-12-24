# vEncode

vEncode.bat is a windows script that encodes video to h264/h265.

aEncode.bat, a companion script, supports audio-only encoding.

The development emphasis is on zero-configuration "just works" software.

## Key Features:

- Makes batch video processing very easy
- AviSynth/Scripting friendly
- H264/H265/Opus support
- Supports changing key encode options (crf/resolution/preset/bit-depth/chroma/fps)
- Easily change the encode setting defaults and/or change them at runtime dynamically
- Supports using the latest versions of key tools (ffmpeg, x264, x265, mkvmerge)
- Automatically place encoded video into Matroska (mkv) or standard MPEG (mp4) containers

## Basic Usage Guide:

1. copy the folder somewhere such that vEncode.bat is in %path%
2. open a command prompt
3. navigate using the CLI to the directory that has the file to encode
4. vEncode myfile.mp4 h265
5. wait a while

## Download:
```
Latest Version: 1.0.0-rc3
In Development: 1.0.0-rc4
```
Click [here](//github.com/gdiaz384/vEncode/releases) or on "releases" at the top to download the latest version.

## Example Usage:
```
vEncode Syntax:
vEncode myfile.mp4 {codec} {resolution} {crf} {a_codec} {preset} {bit-depth} {chroma} {fps}
Note1: order is important
Note2: {} means optional
Note3: Double quotes "" means "use the default value"

Examples:
vEncode myfile.mkv
vEncode "my file.mkv" h264
vEncode "my file.mkv" h265
vEncode file.mkv h264
vEncode file.mkv "" 720p
vEncode file.mkv "" 720p 20
vEncode file.mkv h264 1080p 20
vEncode file.mkv h265 1080p 20 opus
vEncode file.mkv h264 1080p 20 aac veryslow
vEncode file.mkv h265 480p 20 opus veryslow
vEncode file.mkv h265 "" 18 opus veryslow
vEncode file.mkv h265 720p 18 opus veryslow 10
vEncode file.mkv h264 720p 16 aac veryslow 10 420
vEncode file.mkv h265 720p 18 opus slow 10 444
vEncode file.mkv h264 1080p 16 copy slow 10 420
vEncode file.mkv h265 720p 18 opus slow 10 444
vEncode file.mkv h265 "" "" opus "" "" 420 24000/1001

Suggested values and (defaults):
Codec: h264, h265, (h265)
Resolutions (16:9): 480p, 576p, 720p, 1080p, 1440p, 2160p, 4k
(4:3): 480p_43, 576p_43, 720p_43, 1080p_43, 1440p_43, 2160p_43, 4k_43
CRF values: usually 16-28, (17)
AudioCodecs: copy, none, opus, vorbis, aac, mp3, ac3, (opus)
Presets: ultrafast,fast,medium,slow,veryslow,placebo, (veryslow)
Bit depth: 8, 10 or 12, (10)
YUV Pixel Format: 420, 422, 444, (444)
FPS: 24000/1001, 25000/1000, 30000/1001, 30000/1000, (original)
Note: Enter "" for a value to use the default value.

To encode all video files in a directory:
vEncode * h264 "" 16 none veryslow 8 420
vEncode * h265 "" 17 copy "" 12 420
vEncode * h265 720p 17 opus veryslow 10 444 24000/1001   
vEncode *
```

```
aEncode Syntax:
aEncode myfile.mp4 {audioCodec} {audioBitrate} {volumeLevel}

Examples:
aEncode myfile.mp4
aEncode myfile.mp4 opus
aEncode myfile.mp4 mp3 192
aEncode myfile.mp4 opus 320 1
aEncode myfile.mp4 opus 320 3.5

Suggested values and (defaults):
Codec: opus, vorbis, aac, mp3, ac3, (opus)
Bitrate: 96, 128, 160, 192, 224, 320, (192)
VolumeLevel: 0.5, 0.8, 1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0, (1.0)

To encode all media files in a directory:
aEncode *
aEncode * opus
aEncode * opus 192
aEncode * opus 192 2.5
```

## Release Notes:

- Intended use case is to set lots of videos to encode and come back later to do the subs (Aegisub/SubtitleEdit) and fix the metainfo (mkvmerge-gui).
- If downloading from github manually (instead of using an official release.zip) remember to change the line ending format from Unix back to Windows using Notepad++.
- 8-bit encodes can use either ffmpeg.exe or x264-8.exe/x265-8.exe but 10/12 bit encoding always require x264-10.exe/x264-12.exe and x265-10.exe/x265-12.exe
- The binaries included in releases require AVX capable CPUs. For non-AVX CPUs, download different binaries using the links below.
    - x264: [VideoLan](//download.videolan.org/x264/binaries/)
    - x265: [msystem.waw.pl/x265](http://msystem.waw.pl/x265/)
- The following OS architecture charts lists the default compatibility of the provided binaries with various bit depths. If the required binary is not provided (marked as "No" on the chart) and needed, compile/obtain one and place into bin/x86 or bin/x64. Rename it appropriately.

![screenshot1](misc/BitDepthCompatability.png)

## Additional Notes For AVISynth Users:

- Remember that the architecture of avisynth must match the architecture of any executables that interact with the .avs file directly. 
- When using vEncode, this means the architectures of the ffmpeg/ffprobe must match.
- On 64-bit systems, to invoke the correct executable automatically use the command prompt located at C:\Windows\sysWOW64\cmd.exe  
- On 64-bit systems with Avisynth+ installed, use the normal cmd.
- On 64-bit systems, the encode executable's (x264.exe/x265.exe) architecture does not have to match (since they do not directly interact with the .avs file but rather take their input from ffmpeg). Thus, feel free to copy x265-10.exe/x265-12.exe from the bin\x64 folder to the bin\x86 folder for convenience purposes.

## Dependencies (included):
```
Basic: ffmpeg.exe, mkvmerge.exe, ffprobe.exe
(optional) For native h264/h265 8-bit support: x264-8.exe, x265-8.exe
For 10-bit support: x264-10.exe, x265-10.exe
For 12-bit support: x264-12.exe, x265-12.exe
```

For the latest versions of the binaries:
- ffmpeg: [Zeranoe](//ffmpeg.zeranoe.com/builds/) (static)
- mkvmerge: [FOSS Hub](//www.fosshub.com/MKVToolNix.html) (portable)
- x264: [VideoLan](//download.videolan.org/x264/binaries/)
- x265: [msystem.waw.pl/x265](http://msystem.waw.pl/x265/)

## License:
Pick your License: GPL (any) or BSD (any) or MIT/Apache
