# [FFmpeg](https://www.ffmpeg.org/) hardware transcoding (nvidia)

## h264/AAC -> DNxHR/PCM (with hardware decoding, deinterlace and clip-cut)
```
ffmpeg -ss 00:03:50.00 -c:v h264_cuvid -i ./CLIP.MTS -vf bwdif -c:v dnxhd -c:a pcm_s16le -profile:v dnxhr_sq -pix_fmt yuv422p -f mov -t 00:00:15.00 ./clip.mov
```

## h264/AAC -> DNxHR/PCM (with hardware decoding and clip-cut)
```
ffmpeg -ss 00:04:45.00 -c:v h264_cuvid -i ./CLIP.MP4 -c:v dnxhd -c:a pcm_s16le -profile:v dnxhr_sq -pix_fmt yuv422p -f mov -t 00:00:10.00 ./clip.mov
```

## DNxHR/PCM -> h264/AAC (yt)
```
ffmpeg -i clip.mov -codec:v h264_nvenc -bf 2 -flags +cgop -pix_fmt yuv420p -b:v 12M -codec:a aac -strict -2 -b:a 384k -r:a 48000 -movflags faststart clip.mp4
```

## DNxHR/PCM -> h265/AAC (hq)
```
ffmpeg -i ./clip.mov -c:v hevc_nvenc -preset slow -rc cbr_hq -cbr 1 -2pass 1 -b:v 20M -c:a aac -b:a 384k ./clip.mp4
```

## DNxHR/PCM -> h265/AAC (yt)
```
ffmpeg -i ./clip.mov -c:v hevc_nvenc -preset slow -rc vbr_hq -b:v 12M -c:a aac -b:a 384k ./clip.mp4
```

<br />




# [FFmpeg](https://www.ffmpeg.org/) screengrab

## linux
```
ffmpeg -f x11grab -video_size 1024x768 -i $DISPLAY -s 1024x768 -r 25 -preset ultrafast -b:v 10M output.mp4
```

## windows
```
ffmpeg -f gdigrab -video_size 1920x1200 -i desktop -s 1920x1200 -r 30 -preset ultrafast -b:v 10M wingrab.mp4
```

## screengrab with visible region
```
ffmpeg -f x11grab -video_size 1080x608 -show_region 1 -grab_x 100 -grab_y 100 -i $DISPLAY -s 1080x608 -r 25 -preset ultrafast -b:v 10M output.mp4
```

<br />




# [FFmpeg](https://www.ffmpeg.org/) edit

## video cut (no rerender)
```
ffmpeg -ss 00:00:00.00 -i input.mp4 -c copy -t 01:01:00.00 output.mp4
```

## animated gif
```
ffmpeg -ss 00:00:43.5 -i input.mp4 -t 00:00:02.00 -r 15 -s 480x270 output.gif
```

## double the speed of the video
```
ffmpeg -i input.mp4 -filter:v "setpts=0.5*PTS" output.mp4
```