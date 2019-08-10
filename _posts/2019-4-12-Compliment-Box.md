---
layout: project
title: Compliment box
image: /images/heart/title.gif
categories: [electronics]
summary: An Arduino-powered, beating heart in a box
---

![](/images/heart/title.gif)

# A device which gives compliments

I wanted to build a box which, when we pressed a button, it gives a compliment.

![](/images/heart/IMG_7899.JPG)

It was for a friend and I didn't give myself enough time to do something clever
so I hand-coded a bunch of animations for an 8x8 led matrix

```cpp
// explosion
const byte EXPLOSION[][8] = {
     { B00000000, B00000000, B00000000, B00011000, B00011000, B00000000, B00000000, B00000000
  }, { B00000000, B00000000, B00011000, B00111100, B00111100, B00011000, B00000000, B00000000
  }, { B00000000, B00001000, B00011000, B01111100, B00111110, B00011000, B00010000, B00000000
  }, { B00001000, B01001010, B00111100, B11111100, B00111111, B00111100, B01010010, B00010000
  }, { B10011011, B11001010, B00111100, B11100101, B10100111, B00111100, B01010011, B11011001
  }, { B10111011, B11111110, B01000011, B11000011, B11000011, B11000010, B01111111, B11011101
  }, { B11111111, B11100111, B11000011, B10000001, B10000001, B11000011, B11100111, B11111111
  }, { B01100110, B10000001, B10000001, B00000000, B00000000, B10000001, B10000001, B01100110
  }, { B01000010, B10000001, B00000000, B00000000, B00000000, B00000000, B10000001, B01000010
  }, { B00000000, B00000000, B00000000, B00000000, B00000000, B00000000, B00000000, B00000000
  }
};
```

which is cycles through at the push of a button

![](/images/heart/IMG_7900.JPG)


That was fun, until I realised I'd rotated the matrix 90 degrees after coding hundreds of frames by hand, so
I wrote a python programme to rotate the frames to their correct orienttion.

```python
x=0
for image in IMAGES:
    initial = []
    output = []
    x += 1
    # if x > 5: break
    # print(image)
    for row in image:
        # print(row)
        initial.append(list(row))
    # print(output)
    rotated_output = zip(*initial[::-1])
    rotated_output = [list(row) for row in rotated_output]

    # print(rotated_output)
    trimmed_output = rotated_output[1::]  # remove the row of Bs
    [row.insert(0, "B") for row in trimmed_output]
    # pprint.pprint(trimmed_output)
    output = [''.join(row) for row in trimmed_output]
    # pprint.pprint(output)
    print("}, {")
    for row in output:
        print("  %s," % row)
```

![](/images/heart/IMG_7902.JPG)

## The finished product

<iframe width="560" height="315" src="https://www.youtube.com/embed/Utl96o-0_tw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## The code
The code is [here](https://github.com/SachaWheeler/heart-box){:target="_blank"}
