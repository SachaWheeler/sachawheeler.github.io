---
layout: page
title: Arduino Candle
image: https://i.imgflip.com/qlrhv.gif
summary: A digital candle using and Arduino and 8x8 led matrics.
---

## Why
I saw a beautiful LED candle at the Museum of Modern Art (New York) and wanted one. How difficult could it be with an Arduino?

![LED candle inspiration](https://j.gifs.com/ygq8LA.gif)

## Get a video

I probably could have shot something myself, but I did a search and found this on youtube 
![](https://j.gifs.com/Kz8JGP.gif)

Which I grabbed using:

```bash
youtube-dl https://www.youtube.com/watch?v=FZc9a5Kg8Yk
```

Trim it
I cropped it down to a square, including only the flame:

```bash
ffmpeg -i Candle\ clipped.mov -vf "crop=275:275:210:0" -strict -2 Clipped\ final.mp4
```

Scaled it down to 16x16 pixels:

```bash
ffmpeg -i Candle\ final.mp4 -strict -2 -vf scale=16:16 Candle.16x16.mp4
```

And split it into it's frames (4 per second seemed sufficient):

```bash
ffmpeg -i Candle\ final.mp4 -r 4 images/image-%03d.png
```

which left me with this: 
![](http://sachawheeler.com/home/images/LEDCandle-images.png)

## Processing the frames
```php
<?php

$files = glob("images/*.png");
foreach($files as $png){
    echo " { // {$png}\n";
    $im = ImageCreateFromPng($png); 

    $imgw = imagesx($im);
    $imgh = imagesy($im);

    // reduce to 8x8
    $bw_image = imagecreatetruecolor(8, 8);
    imagecopyresampled($bw_image, $im, 0, 0, 0, 0, 8, 8, $imgw, $imgh);

    $dest_imgw = imagesx($bw_image);
    $dest_imgh = imagesy($bw_image);

    for ($i=0; $i<$dest_imgh; $i++)
    {
        echo "  B";
        for ($j=0; $j<$dest_imgw; $j++)
        {
            // get the rgb value for current pixel
            $rgb = ImageColorAt($bw_image, $j, $i); 

            // extract each value for r, g, b
            $rr = ($rgb >> 16) & 0xFF;
            $gg = ($rgb >> 8) & 0xFF;
            $bb = $rgb & 0xFF;

            // get the Value from the RGB value
            $g = round(($rr + $gg + $bb) / 3);

            // grayscale values have r=g=b=g
            // $val = imagecolorallocate($im, $g, $g, $g);

            // set the gray value
            // imagesetpixel ($im, $i, $j, $val);

            echo $g>20?1:0; // This is where I decide to output TRUE or FALSE based on the threshold (20 in this case). If I can output partial values (on another matrix) I'll remove this
            // if($j<$dest_imgw-1) echo ", ";
        }
        // echo "},\n";
        if($i<$dest_imgh-1) echo ",";
        // echo "\n";
    }
    echo "}, \n";
}
?>
```

## Output
The script above gives me the data structure I need to paste into my Arduino Code

```cpp
{ // images/image-001.png
B00000000,  B00110000,  B00110000,  B00110000,  B00111000,  B00111000,  B00011000,  B00000000},

{ // images/image-002.png
B00000000,  B00110000,  B00110000,  B00110000,  B00111000,  B00111000,  B00011000,  B00000000},

{ // images/image-003.png
B00000000,  B00110000,  B00110000,  B00110000,  B00111000,  B00111000,  B00111000,  B00000000},

{ // images/image-004.png
B00000000,  B00110000,  B00110000,  B00110000,  B00111000,  B00111000,  B00111000,  B00000000},

{ // images/image-005.png
B00100000,  B01110000,  B00110000,  B00111000,  B00111000,  B00111000,  B00111000,  B00000000},
```

## The Code
[Arduino Code](https://github.com/SachaWheeler/ArduinoLEDCandle)

## The hardware
Pretty simple this time. Wired up at the pub. 
![](https://pbs.twimg.com/media/CN-mWcDWwAAjnTS.jpg)

## Result

![Final Result](https://i.imgflip.com/qlrhv.gif)

The MAX7219 chip (which drives this LED matrix) doesn't allow individual pixel intensity, so they're either on or off. This means the subtleties of the animation are lost.

## Addendum
I ran the above steps again, but this time cropping to an 8x16 grid, and brought another identical LED Grid into play.

![8x16](https://i.imgflip.com/qqa1b.gif)
