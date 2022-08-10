# Onder's FFmpeg Cheat Sheet

Here i note scripts and commands i mostly use while working on videos. If you somehow end up here, feel free to take a look :)

## Scaling Videos

```sh
ffmpeg -i input.avi -vf scale=320:240 output.avi
```

## Splitting a Video to Frames

```sh
INPUT_NAME = "INPUT.mp4"

mkdir frames
ffmpeg -i $INPUT_NAME frames/frame_%03d.jpg
```

### Splitting Videos in a Folder to Frames Using Python

```python
import subprocess

video=0
while video < 45: # I had 45 videos named like VID000.mp4 , VID001.mp4 ...
	name = 'VID' + str(video).zfill(2)
	name2 = name + '.mp4'
	subprocess.run(f'mkdir {name}', shell = True)
	result = subprocess.run(f'ffmpeg -i {name2} {name}/{name}_%03d.jpg', shell = True)
	print(result)
	video += 1

```
With subprocess we can execute shell commands in a python script. Write this code and save it in a python file (eg: "split.py"). You may need sudo privilages to run it. 

```sh
sudo python split.py
```

If you are more comfortable with bash scripts you can do it like what i did on compressing multiple videos in a folder down below.

## Merging Frames to a Video with FFmpeg

```sh
FPS = 30
OUTPUT_NAME = "OUTPUT.mp4"

ffmpeg -r $FPS -f image2 -s 1920x1080 -i frame_%03d.png -vcodec libx264 -crf 25  -pix_fmt yuv420p $OUTPUT
```
"frame_%03d.png" means all frames from "frame_001.png" to "frame_999.png".
In order to get all frames with ".png" extension use "*png"


## Compressing Videos with FFmpeg

```sh
INPUT_NAME = "INPUT.mp4"
OUTPUT_NAME = "OUTPUT.mp4"

ffmpeg -i $INPUT_NAME -vcodec libx265 -crf 28 $OUTPUT_NAME
```

### Compressing Multiple Videos in a Folder with FFmpeg
Write and save the following code in a bash file (eg: "compress.sh"), don't forget to change _path_ variables

```sh
#!/bin/bash
for filename in /PATH_TO_INPUT_VIDEOS/*.mp4; do
   ffmpeg -i "$filename" -vcodec libx265 -crf 28 /PATH_TO_OUTPUT_VIDEOS/"$(basename "$filename" .mp4).mp4"
done
~    
```
