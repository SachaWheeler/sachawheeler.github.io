---
layout: project
title: Arduino Hourglass
image: /images/hourglass/hourglass_cover.gif
categories: [electronics]
summary: A digital hourglass using an Arduino, 2 8x8 led matrices and a 3-axis accelerometer.
---

I saw a nice digital hourglass on instagram

![LED hourlass inspiration](/images/hourglass/G5356L.gif){:style="width: 500px;"}

Which i decided to build for myself.

## The 3 axis accelerometer

![](/images/hourglass/IMG_7093.JPG){:style="width: 500px;"}

The accelerometer gives a value for X, Y and Z (we don't use Z), which I normalise

```cpp
  x_adc_value = analogRead(x_out) - x_range_mid;
  y_adc_value = analogRead(y_out) - y_range_mid;
  // z_adc_value = analogRead(z_out) - z_range_mid;

```

and convert into an angle

```cpp
  theta = round( atan2 (y_adc_value, x_adc_value) * 180/3.14159265 );
  if(theta < 1) theta += 360;

```

and assign an orientation of the device

```cpp
  // these are reversed
  if(theta < 22.5 or theta > 337.5) quadrant = D_NORTH;
  else if(theta < 67.5)             quadrant = D_NW;
  else if(theta < 112.5)            quadrant = D_WEST;
  else if(theta < 157.5)            quadrant = D_SW;
  else if(theta < 202.5)            quadrant = D_SOUTH;
  else if(theta < 247.5)            quadrant = D_SE;
  else if(theta < 292.5)            quadrant = D_EAST;
  else                              quadrant = D_NE;
```

## The finished product

![](/images/hourglass/IMG_7698.MOV.gif)

## The code

<https://github.com/SachaWheeler/hourglass>

