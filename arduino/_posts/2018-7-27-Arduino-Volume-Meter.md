---
layout: project
title: Arduino Volume Meter
image: /images/volume_meter/volume_cover.gif
categories: [arduino]
summary: A digital volume meter using an Arduino.
---

After a lifetime of listening to loud music, I have sensitive ears and (rather passive aggressively)
wanted to let my colleagues know how noisy the office sometimes gets.

I got a 12v [Tower Light](https://www.adafruit.com/product/2993){:target="_blank"}
and thought is would make a good volume indicator

![Tower Light](/images/volume_meter/2993-01.jpg){:style="width: 500px;"}

and a [MAX9814 Microphone Amplifier](https://www.adafruit.com/product/1713){:target="_blank"}

![MAX9814 Microphone Amplifier](/images/volume_meter/1713-00.jpg){:style="width: 500px;"}

## The Microhone and Amplifier

![](/images/volume_meter/IMG_7645.JPG){:style="width: 500px;"}

Very easy to install. The main loop of the code samples the amplitude of the signal coming from the microphone
throughout a 50 millisecond window and I convert it into volts.

```cpp
void loop() {
   unsigned long startMillis= millis();  // Start of sample window
   unsigned int peakToPeak = 0;   // peak-to-peak level

   unsigned int signalMax = 0;
   unsigned int signalMin = 1024;

   // collect data for 50 mS
   while (millis() - startMillis < sampleWindow)
   {
      sample = analogRead(0);
      if (sample < 1024)  // toss out spurious readings
      {
         if (sample > signalMax)
         {
            signalMax = sample;  // save just the max levels
         }
         else if (sample < signalMin)
         {
            signalMin = sample;  // save just the min levels
         }
      }
   }
   peakToPeak = signalMax - signalMin;  // max - min = peak-peak amplitude
   double volts = (peakToPeak * 5.0) / 1024;  // convert to volts
```

![](/images/volume_meter/IMG_6825.JPG){:style="width: 500px;"}

And I light the appropriate light, depending on the voltage while I defined by trial and error

```cpp
float green = 0.3;
float yellow = 0.6;
float red = 1.2;
```

```cpp
  if(volts < green){
     digitalWrite(GREEN, HIGH);
     digitalWrite(YELLOW, HIGH);
     digitalWrite(RED, HIGH);
   }else if(volts < yellow){
     digitalWrite(GREEN, LOW);
     digitalWrite(YELLOW, HIGH);
     digitalWrite(RED, HIGH);
   }else if(volts < red){
     digitalWrite(GREEN, HIGH);
     digitalWrite(YELLOW, LOW);
     digitalWrite(RED, HIGH);
    }else{
     digitalWrite(GREEN, HIGH);
     digitalWrite(YELLOW, HIGH);
     digitalWrite(RED, LOW);
   }
```

It's very simple and really doesn't need an arduino, but a 3 hour build is a 3 hour build...

## The finished product

I put it in a box, pokeing the microphone through the lid.

![](/images/volume_meter/IMG_7658.JPG){:style="width: 500px;"}

## Thoughts

Fun, but the best bits were a bit too easy. The mic and amplifier do such a good job, I really only had to install them.

But it worked! For a while... People seemed surprised to realise how quickly a noisy environment grew, and it changed behaviour.
Until people got used to it, and possibly, quite reasonably, resented being scolded for making a noise.

![The final product](/images/volume_meter/finished.gif)

An interesting experiment.

## The code

<https://github.com/SachaWheeler/volume_meter>

