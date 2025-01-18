# More Video Transcoding

More tools to transcode videos.

## About

Hi, I'm [Lisa Melton](https://lisamelton.net/). I created these tools to transcode my collection of Blu-ray Discs and DVDs into a smaller, more portable format while remaining high enough quality to be mistaken for the originals.

Unlike some [Ruby](https://www.ruby-lang.org/)-based projects, these tools are just separate standalone scripts which must be installed and updated manually.

You *do* know how to install a Ruby script manually, right? As well as [`HandBrakeCLI`](https://handbrake.fr/downloads2.php) and [`ffprobe`](https://ffmpeg.org/download.html)? Because both of those tools are required by my scripts.

Currently, this project contains four video transcoding tools:

- `two-pass-transcode.rb`
- `hevc-transcode.rb`
- `nvenc-hevc-transcode.rb`
- `av1-transcode.rb`

About 90% of the code in each script is the same. Really. Carefully and *inefficiently* duplicated. By design. Why? Because it's unlikely you'll ever use more than two of these. In fact, I expect most people will use only one.

### `two-pass-transcode.rb`

This Is The Way™. At least the best way I've found to create high-quality 1080p and smaller-resolution videos. This uses the venerable `x264` software-based encoder. With two-pass ratecontrol, it _is_ a bit slower than other `x264`-based systems but the output quality is worth the wait, as is the output size. All audio is in AAC format to further reduce output size.

**Video:**

Resolution | H.264 bitrate
--- | ---
1080p (Blu-ray) | 5000 Kbps
720p | 2500 Kbps
480p (DVD) | 1250 Kbps

**Audio:**

Channels | AAC bitrate
--- | ---
Surround | 384 Kbps
Stereo | 128 Kbps
Mono | 80 Kbps

For this script only, 4K inputs are automatically scaled to 1080p and HDR is automatically converted to SDR color space.

Video is also automatically cropped and forced subtitles are automatically burned into the video track.

But there are many customization options including direct access to most of the `HandBrakeCLI` API.

### `hevc-transcode.rb`

Designed for 4K HDR content, this script uses the `x265_10bit` software-based encoder with a constant quality ratecontrol system. But it's reeeeeally slow. I mean, really slow. However, it does produce high-quality output. You just have to decide whether it's worth it.

One big selling point is that the `x265_10bit` encoder can produce output compatible with both the [HDR10](https://en.wikipedia.org/wiki/HDR10) and [HDR10+](https://en.wikipedia.org/wiki/HDR10%2B) standards as well as [Dolby Vision](https://en.wikipedia.org/wiki/Dolby_Vision).

Audio and subtitle track behavior is the same as with `two-pass-transcode.rb`.

### `nvenc-hevc-transcode.rb`

Also designed for 4K HDR content, this script uses the `nvenc_h265_10bit` Nvidia hardware-based encoder, also with a constant quality ratecontrol system, because you can't always afford to wait on `x265_10bit`. The output will be slightly larger and somewhat lesser in quality but you'll get it a LOT faster. A lot.

But be aware that the `nvenc_h265_10bit` encoder can only produce HDR10-compatible output.

Audio and subtitle track behavior is also the same as with `two-pass-transcode.rb`.

### `av1-transcode.rb`

This Is The Future. Unfortunately, the AV1 video format is currently the Star Trek Future. Other than desktop PCs, most devices can't play it yet. This script uses the `svt_av1_10bit` software-based encoder with a constant quality ratecontrol system. Although the encoder is already quite good, it's still a work in progress. And while the encoder is slower than `x265_10bit` on ARM platforms, it's faster on Intel and usually produces smaller output. So it's certainly worth a try. Especially on 4K HDR content.

The `svt_av1_10bit` encoder can produce output compatible with the HDR10 and HDR10+ standards, but not Dolby Vision.

While audio and subtitle track behavior is similar to the other scripts, audio is in Opus format at slightly lower bitrates. Why Opus? Because if you can play AV1 format video, you can certainly play Opus format audio.

## Usage

Did you actually *read* the `--help` option output for each script? Go ahead. I'll wait.

See? Aaaaand... that's how you use them.

## Feedback

Please report bugs or ask questions by [creating a new issue](https://github.com/lisamelton/more-video-transcoding/issues) on GitHub. I always try to respond quickly but sometimes it may take as long as 24 hours.

## Acknowledgements

This project would not be possible without my collaborators on the [Video Transcoding Slack](https://videotranscoding.slack.com/) who spend countless hours reviewing, testing, documenting and supporting this software.

## License

More Video Transcoding is copyright [Lisa Melton](https://lisamelton.net/) and available under a [MIT license](https://github.com/lisamelton/more-video-transcoding/blob/master/LICENSE).
