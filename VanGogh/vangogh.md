# The VanGogh Dilemma - 400
	Famous computer scientist and artist Keith Van Gogh seems to have misplaced their paint bucket. Find the flag telling them where they put it.

[TheVanGoghDilemma.png](pic.png)

Since this is a forensics problem, we do the normal steg checks: end of file appending, stegsolve, etc. Nothing really there. So next, we find the original image, and on google images there's only 1 file of Van Gogh's starry night that has the same dimensions as ours. That's the original file, and we download it.

[Original File](van-gogh-starry-night.png)

We then write a program to find differences between the two pictures, and the locations at which they are found:

```python
from PIL import Image

im = Image.open("pic.png")
width, height = im.size
mode = 'RGB'
im2 = Image.open("van-gogh-starry-night.png")


count = 1
a = im.getdata()
b = im2.getdata()
out2 = []
out3 = []
for i in range(len(a)):
    
    
    if a[i] != b[i]:
        print(a[i],b[i])
        if count != 0:
            print count
            out3 += [count]
            count = 1
        out2 += [a[i]]
        
        
    else:
        count = count + 1
```

Note: out3 is a list of positions, and out2 is a list of the modified values.

We soon see that the modified pixels are at [tetrahedral number](https://oeis.org/A000292) offsets from each other, and that the modified pixels are all gray values. Sadly, the tetrahedral numbers were a red herring, and we got stuck here for a while.

Later, we noticed that XORing the blue value of the pixels that were different led to partially readable text:

```python
from PIL import Image

im = Image.open("pic.png")
width, height = im.size
mode = 'RGB'
im2 = Image.open("van-gogh-starry-night.png")
a = im.getdata()
b = im2.getdata()
out = []
for i in range(len(a)):
    if a[i] != b[i]:
        x = a[i][2]^b[i][2]
        print(x,chr(x))
        out += [hex(x)]
    else:
        count = count + 1
```

And the printed data is:
`\n\xa6<\x04!\x00\x00\xf8 North, \x1cQ\xca\x00F%\xf8West. Flag Is Google MapsName.Have Fun!`

Those seem to be coordinates, and the `\xf8` is probably the degrees sign. We convert this hex data to int:

```
>>> int('0aa63c04210000',16)
2997526464626688
>>> int('1c51ca004625',16)
31137606944293
```

Since latitude and longitudes are on the scale from 0 to 180, we add the decimal point, so we have `29.97526464626688 N, 31.137606944293 W`.
We plug it into [a gps coordinate thing](http://www.gps-coordinates.net/), and try all 4 positive and negative values. Turns out it's actually East. We now have `Ibn Sofian, Nazlet El-Semman, Al Haram, Giza Governorate, Egypt` as the only coordinate that isn't in the middle of nowhere. But that isn't the flag. Googling this exact string, or zooming in on the map a lot on the lat/lng thing, we see that this is actually the Great Sphinx of Giza.

Flag: `Great Sphinx of Giza`.
