---
layout: project
title: The Shining, back and forth
image: /images/the_shining/cover.png
summary: The Shining, superimposed onto itself in reverse.
---

In the film [Room 237](https://en.wikipedia.org/wiki/Room_237) they mention a
bootleg version of The Shining which runs forwards and backwards, simultaneously.

![](/images/the_shining/Room_237_(2012_film).jpg)

I wanted to watch it, and if you follow these instructions, you can too.

### Find yourself a copy of the movie

I found one called: "The.Shining.1980.mkv"

### Get a copy of [ffmpeg](https://www.ffmpeg.org/)

If something is worth doing, it's worth doing on the command line.

### Strip out subtitles
```sh
ffmpeg -i The.Shining.1980.mkv -c copy -sn The.Shining.1980.nosubtitles.mkv
```

### Create a copy with no audio
for the reversed version
```sh
ffmpeg -i The.Shining.1980.nosubtitles.mkv -c copy -an The.Shining.1980.nosound_nosubtitles.mkv
```

### Reverse the copy
ffmpeg loads the whole file into memory to reverse it, so I split it into 10 minute
chunks first to save my laptop
```sh
ffmpeg -i The.Shining.1980.nosound_nosubtitles.mkv -threads 3 -vcodec copy -f segment -segment_time 600 The.Shining.1980.%04d.mp4
```

which left me with a directory full of split movie files to reverse each one
of which I processed with

```sh
ffmpeg  -i The.Shining.1980.0001.mkv -vf reverse The.Shining.1980.reversed_0001.mkv
ffmpeg  -i The.Shining.1980.0002.mkv -vf reverse The.Shining.1980.reversed_0002.mkv
ffmpeg  -i The.Shining.1980.0003.mkv -vf reverse The.Shining.1980.reversed_0003.mkv
etc...
```

### Concatentate the reversed clips in the right order

I created a text file *split_files.txt* with the filenames of the reversed clips,
in reverse order

```sh
splits/The.Shining.1980.reversed_0011.mkv
splits/The.Shining.1980.reversed_0010.mkv
splits/The.Shining.1980.reversed_0009.mkv
splits/The.Shining.1980.reversed_0008.mkv
splits/The.Shining.1980.reversed_0007.mkv
splits/The.Shining.1980.reversed_0006.mkv
splits/The.Shining.1980.reversed_0005.mkv
splits/The.Shining.1980.reversed_0004.mkv
splits/The.Shining.1980.reversed_0003.mkv
splits/The.Shining.1980.reversed_0002.mkv
splits/The.Shining.1980.reversed_0001.mkv
splits/The.Shining.1980.reversed_0000.mkv
```

and concatenated them into a single file
```sh
ffmpeg -f concat -i split_files.txt -c copy The.Shining.1980.nosound_nosubtitles_reversed.mkv
```

and finally overlay them, the second (reversed) one with 50% opacity

```sh
ffmpeg -n -i The.Shining.1980.nosubtitles.mkv -i The.Shining.1980.nosound_nosubtitles_reversed.mkv -filter_complex "[0:v]setsar=sar=1[v];[v][1]blend=all_mode='overlay':all_opacity=0.5" -movflags +faststart The.Shining.1980.combined.mkv
```

## The finished product

![](/images/the_shining/elevator.png){:style="width: 500px;"}
![](/images/the_shining/lobby.png){:style="width: 500px;"}
![](/images/the_shining/corridor.png){:style="width: 500px;"}

### If you're in a hurry,
And have a powerful computer, you can probably run all of these in a single
ffmpeg command. ffmpeg is pretty powerful.

### If you know me
ask, and I'll give you a copy.
### If you're in a hurry,
