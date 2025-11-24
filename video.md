# Video

## Scale to 1280px width
```bash
ffmpeg -i input.mp4 -vf scale=1280:-1 output2.mp4
```

```bash
ffmpeg -i input.mp4 -vf scale=1280:720 output.mp4
```

## Convert a video to gif / webp for animation

```bash
ffmpeg -i input.mp4 output.gif
```

```bash
ffmpeg -i input.mp4 output.webp
```

Small file size
```bash
ffmpeg -y -i input.mp4 -loop 0 -filter_complex "fps=5,scale=480:-1:flags=lanczos,split[s0][s1];[s0]palettegen=max_colors=32[p];[s1][p]paletteuse=dither=bayer" input.webp
```

## Convert a video from dark mode to light mode

> [!IMPORTANT]
> Tested with FFMPEG v7.1

```bash
ffmpeg -i input.mp4 -vf "negate,hue=h=180,eq=contrast=1.2:saturation=1.1" output.mp4
```

### Add an alias to the CLI

Run the below command and then reopen terminal.

```bash
# Define text for alias
newCommand='
# Alias for converting videos from dark mode to light mode
convert-to-light-mode() {
    # Inputs
    inputFileName=$1
    outputFileName=$2
    # Append suffix if output file name not provided
    if [ -z "$outputFileName" ]; then
        fileBaseName="${inputFileName%.*}"
        suffix="-light-mode"
        fileExtension="${inputFileName##*.}"
        outputFileName="${fileBaseName}${suffix}.${fileExtension}"
    fi
    # Convert video
    ffmpeg -i "$inputFileName" -vf "negate,hue=h=180,eq=contrast=1.2:saturation=1.1" "$outputFileName"
}
'

# Add command to bash and zsh
echo $newCommand >> ~/.bashrc
echo $newCommand >> ~/.zshrc
```

### Usage

#### Convert dark-mode video to light-mode.

```bash
convert-to-light-mode input.mp4 output.mp4
```

or without output filename

```
convert-to-light-mode my-video.mp4
```

> Result: my-video-light-mode.mp4

#### Convert video to light-mode and export as `.gif` image

```bash
convert-to-light-mode input.mp4 output.gif
```

>
