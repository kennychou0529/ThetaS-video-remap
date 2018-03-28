# equirectangular-remap
![Example](src.gif) ➡️ ![Result](generated.gif)

Generate maps for conversions of spherical video from full frame fish-eye to equirectangular video in [ffmpeg](http://ffmpeg.org).

Adapted from the example given for ffmpeg's [`RemapFilter`](https://trac.ffmpeg.org/wiki/RemapFilter), with support added for video captured with Ricoh Theta S devices.


## Guide

### Building

1. Install ffmpeg, (make sure it's a recent version)
2. Checkout the source of this repository
3. Build: `$ gcc projection.c -lm -o project

### Running

#### Create maps `xmap.pgm` and `ymap.pgm` for use with videos from Theta S devices:

```
$ ./project -x xmap.pgm -y ymap.pgm -h 960 -w 960 -c 1920 -r 960 -m theta --verbose
```

![xmap](xmap.png) ![ymap](ymap.png)

This creates two ASCII encoded [PGM files](https://en.wikipedia.org/wiki/Netpbm_format#PGM_example) which act as a lookup table for ffmpeg to remap the video.

#### Apply the maps to the video `R0014696.MP4`:

```
$ ffmpeg -i R0014696.MP4 -i xmap.pgm -i ymap.pgm -q 0 -lavfi "format=pix_fmts=rgb24,remap" output.mp4
```
