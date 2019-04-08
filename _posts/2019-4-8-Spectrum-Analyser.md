---
layout: project
title: Spectrum Analyser
image: /images/spectrum_analyser/cover.png
categories: [electronics]
summary: An Arduino powered spectrum analyser (graphic equalizer)
---

I found an old, unused 16x32 RGB led matrix which I bought on a whim years ago and
had completely forgotten about.

![](/images/spectrum_analyser/led_matrix_rgbmatrix.jpg)

After a little googling I found the
[Sparkfun Spectrum Shield](https://learn.sparkfun.com/tutorials/spectrum-shield-hookup-guide/all)
which seemed to do most of the heavy lifting of a 7-band audio spectrum analyser.

![](/images/spectrum_analyser/shield.jpg)

## assembling the shield
I soldered headers onto the shield before reading the spec sheet and later realised that in order
to get the comparatively low-powered arduino to handle the refresh rate on the matrix, certain *liberties*
were taken in the Matrix library; the matrix is *required* to use specific ports for it to work.

The Spectrum Shield *also* uses specific ports for its functionality, and these ports conflict. So I would be unable
simply to plug the shield into the arduino, as I'd soldered it.

So I decided to use *extension* cables so I could redirect the 4 pins (2 analogue, 2 digital) that the Shield required
to ports that were avalaible. (Essentially moving the Shield digital ports 4 & 5 to 12 & 13, and Analogue ports A0 & A1 to A4 & A5).

![](/images/spectrum_analyser/jumper_cables.jpg)

## Power
The Matrix wants 5v but needs up to 4 Amps, averaging about 2A of current, so I split the output of a 5v 4A supply between the
Matrix and the current-limited Arduino barrel plug.

![](/images/spectrum_analyser/power.jpg)

And went the easy route of simply super-glueing non-solder wires into the ribbon cable of the Matrix

![](/images/spectrum_analyser/ribbon.jpg)

## Sadly, that was most of it

At this point it was all about software. Creating a basic visualiser was quite simple.

![](/images/spectrum_analyser/video.gif)

## The finished product

It's not finished yet, but I like it. I'm adding visualisations and some controls.
