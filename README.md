# HQ-Audio
## Formats: MP3, M4A, ALAC, FLAC, WAV
<sub>This guide was written with the intent of use on a Linux distro, if you have questions, contact me.</sub>

**Step #1:**<br>
**Install or compile the required dependencies and packages.**<br>
`sudo apt-get -y install ffmpeg` <br>
`sudo apt-get -y install lame` <br>
`sudo apt-get -y install mp4v2-utils` <br>
`sudo apt-get update` <br>

**Step #2:**<br>
**Upload your audio file to your Linux or Windows server.**<br>
For example, my audio file is going to be **`track.mp3`**<br>
Here is some metadata and stream information for my specified file:<br>
>Bitrate: `96 kbps`<br>
>Sample Rate: `44.1kHz`<br>
>Channels: `1 (Mono)`<br>
>Audio Codec: `MPEG-1 Audio Layer III`<br>
>Compression Mode: `Lossy`<br>

**Step #3:**<br>
**Now that you have your audio file and dependencies/packages installed, we can begin the encoding process.**<br>
Just for first-timer purposes, check if you have the required packages installed by running these two commands.<br>
>`ffmpeg`<br>
>`lame`<br>

After you have ensured that these two dependencies are installed, the next step is to encode your audio file into a lossless format.<br>
Run this command to begin the encoding process of your audio file into a 24-Bit/48kHz FLAC file. (Replace `track.mp3` with the name and format of your audio file. keep in mind that certain formats have limited transcoding properties).<br>
`ffmpeg -i track.mp3 -sample_fmt s32 -codec:a flac -ar 48k -b:a 1411k -ac 2 track.flac`<br>
The output should look like something around this subject.<br>
```
[flac @ 0x5616606c4880] encoding as 24 bits-per-sample
Output #0, flac, to 'track.flac':
  Metadata:
    encoder         : Lavf58.45.100
    Stream #0:0: Audio: flac, 48000 Hz, stereo, s32 (24 bit), 1411 kb/s
    Metadata:
      encoder         : Lavc58.91.100 flac
size=   34551kB time=00:03:10.98 bitrate=1482.0kbits/s speed= 158x
video:0kB audio:34543kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: 0.023431%
```
If FFmpeg does not print a similar output, you most likely forgot or did a step incorrectly, or the format of your file could not be transcoded into a high quality, lossless, FLAC format. If done correctly, just go right ahead and download your new, transcoded audio file and enjoy the crisp and immersive experience with CD quality.<br>

Unfortunately, lossless audio files can be quite large so if you would like to combat that, we will use the **`LAME Encoder`** to transcode your audio file into a high qualtiy MP3 format instead.<br>
To begin the encoding process, paste this command into your console. (Remember to change `track.mp3` to the name of your audio file.)<br>
`lame -h --abr 320k -b 320k -B 320k -q 0 --resample 48 track.mp3 highqualitytrack.mp3` <br>
The output should look something like this. <br>
```
LAME 3.100 64bits (http://lame.sf.net)
Using polyphase lowpass filter, transition band: 20323 Hz - 20903 Hz
Encoding track.mp3 to highqualitytrack.mp3
Encoding as 48 kHz j-stereo MPEG-1 Layer III (4.8x) average 320 kbps qval=0
```

**Useful Information to Know** <br>
It would probably be wise of you to know what each flag/switch in these encoders do or mean, so here, I'm going to list out the most important ones.<br>
`-ar <number>k` - Sample Rate (48kHz, 96kHz, 192kHz) <br>
`-b:a <number>k` - Audio Bitrate (320kbps, 1411kbps, 9126kbps) <br>
`-i` - Name of Input File (Comes before **ALL** encoding options) <br>
