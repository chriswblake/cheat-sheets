# Convert a video from dark mode to light mode

```bash
# FFMPEG 7.1
ffmpeg -i input.mp4 -vf "negate,hue=h=180,eq=contrast=1.2:saturation=1.1" output.mp4
```
