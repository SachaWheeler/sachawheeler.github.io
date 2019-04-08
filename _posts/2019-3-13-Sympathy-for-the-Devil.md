---
layout: project
title: Sympathy for the Devil
image: /images/sympathy/cover.png
categories: [movies]
summary: Jean-Luc Godard's Sympathy for the Devil with the politics removed
---

![](/images/sympathy/titles.png)

In Jean-Luc Godard's film [Sympathy for the Devil](https://en.wikipedia.org/wiki/Sympathy_for_the_Devil_(film)){:target="_blank"}
the famous (I find him impenetrable) director gets footage of The Rolling Stones recording their song but decides to
combine unrelated footage, which might be interesting in its own right, but simply gets in the way.

### Good
![good](/images/sympathy/rehearsal.png){:style="width: 500px;"}
### Bad
![bad](/images/sympathy/politics.png){:style="width: 500px;"}
![confusing](/images/sympathy/politics2.png){:style="width: 500px;"}

I wanted to remove everything except The Rolling Stones, and if you follow these instructions, you can too.

### Find yourself a copy of the movie

I found one called: "Sympathy for the Devil 1968.avi" which I converted into a MKV with

```sh
ffmpeg  -i ./Sympathy\ for\ the\ Devil\ 1968.avi -acodec libvorbis Sympathy\ for\ the\ Devil\ 1968.mkv
```

### find the start and end points of the scenes we want to keep

Which turn out to be

```sh
#!/bin/sh

# cut the following timestamps

ffmpeg -ss 00:00:00 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 00:03:40 -c copy output_1.mkv
ffmpeg -ss 00:04:02 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 00:07:08 -c copy output_2.mkv
ffmpeg -ss 00:07:33 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 00:10:54 -c copy output_3.mkv
ffmpeg -ss 00:21:38 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 00:32:05 -c copy output_4.mkv
ffmpeg -ss 00:41:08 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 00:51:43 -c copy output_5.mkv
ffmpeg -ss 01:03:00 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 01:05:31 -c copy output_6.mkv
ffmpeg -ss 01:07:41 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 01:12:25 -c copy output_7.mkv
ffmpeg -ss 01:22:31 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 01:25:13 -c copy output_8.mkv
ffmpeg -ss 01:25:33 -i Sympathy\ for\ the\ Devil\ 1968.mkv -to 01:37:10 -c copy output_9.mkv

```

### Concatenate the clips

```sh
for f in ./output_*.mkv;
    do echo "file '$f'" >> split_files.txt;
done
ffmpeg -f concat -safe 0 -i split_files.txt -c copy Sympathy\ for\ the\ Devil\ 1968\ output.mkv
```

## The finished product

Much better. MUCH BETTER!

Look, I like art films, but if you are lucky enough to witness and record an historical event,
you have to work pretty hard to make it about you. Some people succeed
([Albert and David Maysles](https://en.wikipedia.org/wiki/Albert_and_David_Maysles){:target="_blank"}, for instance),
but I don't think Godard cares about the footage he captured, or the message he smashes into it.

If you like the Stones, this edit is better.

(full disclosure: I identify politically as a Marxist. I don't mind the political content, just that it's getting
in the way!)

### If you know me
ask, and I'll give you a copy.
