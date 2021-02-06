# Databending with Audacity: The Effect of Paulstretch on Planar RGB Images

## Required Software
(This is not truly mandatory to achieve the same result; this is just what I know works. Feel free to port this piece to other OSes or optimize it)
 - Windows 7 or newer
 - Irfanview
 - Audacity
 
## Required Reading
If you wish to follow along, please read and understand these two articles first:

[Databending using Audacity](https://www.hellocatfood.com/databending-using-audacity/) by hellocatfood

[Databending with Audacity: What I do Differently/Required Reading](https://github.com/multiplealiases/Databending-In-Audacity-Required-Reading/blob/main/README.md) by me

I'll assume you understand the basic idea of converting images into raw data, importing them into Audacity, applying effects to it, exporting the results, then opening the raw data in a suitable image viewer. That's covered by the first one.

The second one covers how I do databending with Audacity, and how my method differs from the one detailed in the first article.

To replicate my results, go to each respective image's source, and download the "Medium" size.

## Paulstretch, what does that do?
[Paulstretch](http://hypermammut.sourceforge.net/paulstretch/) is an effect suitable for extreme stretching of audio, 50x or more. It essentially "smooths out" audio. Try it yourself in Audacity; open a file with some music on it, run Paulstretch at a Stretch Factor of 10 and a Time Resolution of 2 seconds, and listen to how it just "blurs" the sound together. 

With that in mind, I thought to myself, "Wait, what happens if I tried this with image data?". Let's get today's test subject.

![White wavy texture of a sand dune](https://i.imgur.com/wBdHKEI.jpg)
[Photo](https://unsplash.com/photos/7Y0NshQLohk) by [Sumner Mahaffey](https://unsplash.com/@sumnerm) on [Unsplash](https://unsplash.com/)

You know the drill. Convert to planar RGB RAW, import as shown here:

![enter image description here](https://i.imgur.com/iKdjGlD.png)

...and you'll see this in Audacity. Select the entire thing.

![enter image description here](https://i.imgur.com/SA1p4Qg.png)

Go under Effect > Paulstretch. You'll get this:

![enter image description here](https://i.imgur.com/gdNpBOv.png)

The Time Resolution's not important here yet; just set it to 50 seconds first. Stretch Factor, however, must be 1. The image becomes unusable nonsense if it's not 1. Once that's done, press OK.

![enter image description here](https://i.imgur.com/fovHbz7.png)

You can listen to this if you want. It just sounds "blurred". For the eagle-eyed among you, you'll have noticed that Paulstretch has changed the length of the file. This won't affect anything, not in this article.

As usual, just export this and open it with the correct parameters...

![enter image description here](https://i.imgur.com/gf1RDvH.jpg)

What? How? It's a spectacular effect, for sure. I didn't do this beforehand; I databent the image while I was writing this article. You'd think that an effect that "blurs" sound would just be a blur effect on images, but this is something else.  

Let's split it into individual R, G and B planes to understand what's going on. I've done it such that R is the first vertical third, G is the 2nd third, and B is the final third.

![enter image description here](https://i.imgur.com/BsWC5X4.jpg)

It's like it copies the textures in the original image over and over, overlaying them into a nice final texture. I did it, and you can, too!

### Paulstretch on other textures
Paulstretch seems to follow the "texture" of the source image. Let's pull out some more test subjects:

![enter image description here](https://i.imgur.com/iRAVqL0.jpg)
[Photo](https://unsplash.com/photos/EMP0_-qfva8) by [Olivia Hutcherson](https://unsplash.com/@ohutcherson) on [Unsplash](https://unsplash.com)

![enter image description here](https://i.imgur.com/jJZYJfQ.jpg)[Photo](https://unsplash.com/photos/UKLIuV8rAks) by [Christian Perner](https://unsplash.com/@christianperner) on [Unsplash](https://unsplash.com)


![enter image description here](https://i.imgur.com/AfZMYYR.jpg)[Photo](https://unsplash.com/photos/sk59I1qRfEM) by [Scott Webb](https://unsplash.com/@scottwebb) on [Unsplash](https://unsplash.com)


Paulstretch them with the same settings as before, and they turn into this:

![enter image description here](https://i.imgur.com/Yj7jOFS.jpg)
![enter image description here](https://i.imgur.com/GvtsE4E.jpg)
![enter image description here](https://i.imgur.com/9WWAtmM.jpg)

They all have a faint resemblance of their original pictures, but it's certainly a massive change. It's completely different. (Protip: Apply the Amplify effect at default settings after Paulstretch to prevent clipping of colors.)

### But it doesn't stop there
Let's try it on images with less-obvious textures. I'll take these ones:

![enter image description here](https://i.imgur.com/r5TfLva.jpg)[Photo](https://unsplash.com/photos/ulG2K7id26s) by [Christopher Czermak](https://unsplash.com/@czermak_photography) on [Unsplash](https://unsplash.com) 

![enter image description here](https://i.imgur.com/kjS9pav.jpg)
[Photo](https://unsplash.com/photos/fsH1KjbdjE8) by [Alexander Andrews](https://unsplash.com/@alex_andrews) on [Unsplash](https://unsplash.com)

Put it through the Paulstretcher, and we have:


![enter image description here](https://i.imgur.com/ozHPG87.jpg)![enter image description here](https://i.imgur.com/PfAqSQv.jpg)
Fascinating, huh?

### What does Time Resolution do, anyway?
So far, I've been using a Time Resolution of 50 seconds, because I think it produces nice results. Here's a guinea pig.

![enter image description here](https://i.imgur.com/sFoWw5o.jpg)[Photo](https://unsplash.com/photos/cKYM8KMwaUQ) by [Nils Schirmer](https://unsplash.com/@nilsschirmer) on [Unsplash](https://unsplash.com/)

Let's experiment on it(s picture). First off, Paulstretch at 1 second.

![enter image description here](https://i.imgur.com/fbxtj7M.jpg)

2:

![enter image description here](https://i.imgur.com/mmqNmhu.jpg)

4:

![enter image description here](https://i.imgur.com/e61VwAd.jpg)

8:

![enter image description here](https://i.imgur.com/wgtWwmx.jpg)

16:

![enter image description here](https://i.imgur.com/2K5gO2c.jpg)

32:

![enter image description here](https://i.imgur.com/LkZDKvI.jpg)

64:

![enter image description here](https://i.imgur.com/IKsAVJQ.jpg)

128:

![enter image description here](https://i.imgur.com/8wCqNhq.jpg)

It seems that the higher the Time Resolution, the less "liney" the resulting image is. However, the higher it is, the more likely it is that the blue component just gets cut off entirely. I think a safe value for "reasonably sized" images is somewhere between 32 and 64 seconds, so I was justified in using 50 seconds before this.

### Interleaved, and why it sucks here
Now, I generally don't like interleaved because it tends to lose color here, as it does with most things databending. I'll do it for the sake of completeness. I'll just pull a picture of a cat here:

![enter image description here](https://i.imgur.com/6Jl0faD.jpg)[Photo](https://unsplash.com/photos/X7UR0BDz-UY) by [Rana Sawalha](https://unsplash.com/@ranasawalha) on [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash)

Convert this to interleaved RAW, run it through Paulstretch, export it...

![enter image description here](https://i.imgur.com/msAAndc.jpg)

The color's just mostly gone, and it's pretty good pareidolia material; quite spooktacular if you told people that it was haunted or something. You'd have better luck opening this as planar RAW:

![enter image description here](https://i.imgur.com/a3gKasL.jpg)

I'd say this one's way better.

### A small puzzle
This is a Paulstretched image in planar RGB. Can you figure out what the original image was? Can you reconstruct the original? This is a PNG, so it's an exact pixel-for-pixel copy of what I got from Paulstretching; no compression artifacts here.

![enter image description here](https://i.imgur.com/gga9wHR.png)
## Conclusion
Paulstretch... it's a wacky effect. Just transforms images into layers upon layers of textures pasted on top of each other. Try it for yourself; realizing that you've pulled off that with nothing but Audacity and Irfanview is an experience.


