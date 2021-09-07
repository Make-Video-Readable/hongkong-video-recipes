## Technical Notes

All transcriptions are generated using Azure or Google Cloud Platform's Speech-To-Text Service.

### Creating video summary (aka mosaic/tiles etc.) using ffmpeg


> Tiles are 6x6, so the frame interval is: (length in minutes*60)/36*25

`ffmpeg -ss 00:00:22 -i input.mp4 -frames 1 -vf "select=not(mod(n\,400)),scale=480:270,tile=6x6" tile.png`

### Preparing audio files for Google Cloud Compute Speech-to-text service using ffmpeg

GCP recommend no noise cancelling and lossless format, however, YouTube videos are compressed so mp3 will be sufficant.
Audio files larger then 10MB please upload to GCS using gsutil

`ffmpeg -ss 00:00:00 -i input.mp4 output.mp3`

**Slowing down the audio to get a better result**

Turns out some video creators speaking a little bit too fast for machine learning to create a meaningful output, hence we need to slow down the speed.

`ffmpeg -ss 00:00:45 -i input.mp4 -filter:a "atempo=0.75" output.mp3`

_atempo only can set to 0.5 to 2(so slowest is 0.5x)_

Worth knowing GCP is billed by minutes and slowing down will cause the chargeable time increase.

**Making it pitch-perfect using ffmpeg(again)**
```
ffmpeg -ss 00:00:23 -i input.mp4 -filter:a "asetrate=44100*1.1,aresample=44100,atempo=0.75" output.mp3
# including slowdown
```

