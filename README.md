# Databending with Audacity: What I do Differently/Required Reading

## Required Software
(These are not strictly needed to achieve the same result; this is just what I know works. Feel free to port this piece to other OSes or automate/optimize it)
 - Windows 7 or newer
 - Irfanview
 - Audacity

## Background and Introduction

A while ago, I read about [databending using Audacity](https://www.hellocatfood.com/databending-using-audacity/). It was fascinating that you could just have an audio program just interpret raw image data as if it were audio.

I won't bore you with the details, but the basic premise is as follows: 

	   +-------------------+         +--------------------------------+            +-------------------------------------+
	   |                   |         |                                |            |                                     |
	   | Take a .bmp image | +-----> | Import as raw data in Audacity |  +------>  | Do wacky effects to imported audio  +-+
	   |                   |         |                                |            |                                     | |
	   +-------------------+         +--------------------------------+            +-------------------------------------+ |
	                                                                                                                       |
	+----------------------------------------------------------------------------------------------------------------------+
	|
	|  +-------------------+                     +----------------------+              +---------------------------------+
	|  |                   |                     |                      |              |                                 |
	+> | Save audio as RAW | +---------------->  | Open resulting image | +----------> | Save in a more shareable format |
	   |                   |                     |                      |              |                                 |
	   +-------------------+                     +----------------------+              +---------------------------------+




And that's it, really. There are, however, certain things I want to do differently. These things will be the basis of how I databend with Audacity, so it's the "Required Reading" of my guides.

## Import as Unsigned 8-bit PCM, not U-Law.
The idea here is that this format is a direct representation of the bytes in a raw image format. If we were to use 24-bit RGB, then every R, G and B component each gets one byte each mapped to one sample of the audio.

Also, U-Law introduces some error when you export it. Take this photo:

![Wallpaper photo](https://imgur.com/CCUypqN.jpg)
[Photo](https://unsplash.com/photos/n7a2OJDSZns) by [Harli Marten](https://unsplash.com/@harlimarten) on [Unsplash](https://unsplash.com/)
(If you wish to replicate my results, download the Medium-sized image)

Convert that into .bmp. Import it into Audacity as raw data with the following settings:

![Audacity "Import Raw Data" dialogue. Encoding is U-Law, Byte order is Little-endian, Channels is 1 Channel (Mono). Start offset is 0 bytes, Amount to import is 100%, and sample rate is 44100 Hz](https://i.imgur.com/UqudxNc.png)

You'll see something like this:

![Audacity interface. It shows the waveform of a .bmp file interpreted as U-Law audio. ](https://i.imgur.com/gh4VPtZ.png)

Just ignore it, and go straight to File -> Export -> Export Audio..., and you'll see this dialogue. Change the settings as shown.

![Audacity "Export Audio" dialog. Under Format Options, Heading is set to "RAW (header-less)" and Encoding is set to "U-Law"](https://i.imgur.com/LLozAix.png)

Save, and you'll end up with an image that looks like this:

![Same image as the original wallpaper photo, but with curved lines of green and red running across it.](https://i.imgur.com/K2gOGy7.jpg)

I'm not sure about you, but I'd rather have the effects corrupt the image, not the encoding itself. 

If you did the exact same thing, but with the import and export encoding set to "Unsigned 8-bit", you'll get this:

![Irfanview "Set RAW open parameters" dialog](https://i.imgur.com/mAv1UkA.png)

Which is not the intended result. Unsigned 8-bit seems to corrupt the header more often than not, and it'll open as a .raw file instead of a .bmp file. Most photo viewers won't even give you this, instead spitting out something like "corrupted file".

However, if you open it with those parameters anyway, you'll be greeted with this:

![An identical copy of the original wallpaper photo.](https://i.imgur.com/s83ZWvD.jpg)
It's identical to the original, minus whatever compression artifacts were introduced due to saving in JPEG. This is perfect.

This nonsense involving having to open as RAW brings me to my second point:

## Don't use BMP. Just use RAW images.
The problem with .bmp is that if the header's borked, you can't open it, as we just saw. This is why I prefer to work with RAW images, since you can do whatever you want with it, and as long as you know what parameters to use, it *will* open, no questions asked. They don't have a header to corrupt, so it is utterly impossible to render them unopenable.

Using Irfanview, it's absolutely trivial to save an image as RAW. Let's take this image:

![close-up photo of common sunflower](https://i.imgur.com/XHsLn0s.jpg)

(Attribution: [Photo](https://unsplash.com/photos/5lRxNLHfZOY) by [Paul Green](https://unsplash.com/@pgreen1983) on [Unsplash](https://unsplash.com/))
(To follow along, use the Medium version)

Open it up in Irfanview:

![The same sunflower picture, but opened in Irfanview](https://i.imgur.com/VE22aJn.png)

Go under File -> Save as..., and you'll be shown this:

![Save as dialog in Irfanview](https://i.imgur.com/xgqtng2.png)

The things you want to change here are the "Save as type" and the RAW save options. Set "Save as type" to "RAW - RAW Image Data" and under "Options for 24 BPP images", set "Color order: RGB" and "Planar (RRR...GGG...BBB)"

I'll get to why I use a planar format later on. For now, save it. Let's take a look at it in Explorer.

![Windows Explorer interface. "sunflower.raw" is selected, showing a size of 15.8MB.](https://i.imgur.com/ATL8JZ7.png)

15.8 MB. It's absolutely gigantic compared to the original, which was only
698 KB. Why is that? The basic idea here is that these raw formats are uncompressed. They literally just write down the information as it is without compressing it at all. This is horrendous if you want to share images, but it's absolutely invaluable to how I databend. Compressed formats have this tendency to just break if you're not careful.

For most practical databending, a resolution of (something)×1080 (keeping aspect ratio) is probably enough. Larger files are harder for Audacity to work with because they're read as longer audio and take longer to process. I won't stop you, of course, but just be mindful of that. I'll assume you know how to resize images.

To open the raw image that we just made, let's go back to Irfanview and try to open it:

![Irfanview "Set RAW open parameters" dialog](https://i.imgur.com/BnppH7N.png)

And this shows up again. The important things to follow are

 - Set resolution to the original image's. In this case, "Image width" is 1920, and "Image height" is 2880.
 - Set BitsPerPixel (BPP) to "24 BPP (3 bytes per pixel)"
 - Don't touch the Misc options; just turn off the "Vertical Flip", "Grayscale" and "Bayer Pattern Used" options.
 - Under "Options for 24 and 32 BPP, set "Color order: RGB", and "Planar (RRR... GGG... BBB)".
 
In general, you want to match the original image's properties.

Once that's done, you should see this:

![Irfanview interface. It is showing a picture of a sunflower.](https://i.imgur.com/SquWZmU.png)

And you've just opened a RAW image. Let's pretend this is a databent file we just made, and you want to make a copy of it that you can easily share. Repeat the process of Saving As, and this shows up:

!["Save Picture As..." dialog in Irfanview.](https://i.imgur.com/iMnJiW1.png)

Set the "Save as type" to "PNG - Portable Network Graphics". PNG produces larger formats than JPEG, yes, but I do this because PNG is lossless. It is a perfect, pixel-for-pixel copy of the image. Think of it as a master, the absolute highest quality version of your image that you can use for editing. For sharing, you can use JPEG, scale the image down, whatever.  

It depends on how long you're willing to wait, but I prefer to set my "Compression level" to 9, the highest. My databending often produces very hard-to-compress files, so I like to compress it as hard as possible just because it's more space-efficient.

## Use *planar* RAW images.
"Wait, what's planar?", you're probably asking. I'll try to explain it as concisely as possible:

Let's define the terms R, G and B. These are the Red, Green and Blue components of an image. With RAW images, there are two standard ways to write down the image data.  Either it's interleaved, or it's planar. 

Interleaved basically goes (RGB, RGB, RGB) (the order can vary depending on the exact format, but you get the picture). Each pixel gets 3 color components packed together into one.

Planar goes RRR, (...) GGG, (...), BBB. In this case, you get 3 monochrome "planes" (hence the term "planar") of Red, Green, and Blue stored consecutively.

So what's the benefit of planar over interleaved? 
### Planar images are easier to work with
Suppose you databent an image, and it isn't centered. Maybe it's too far left, or too far right. Now, you can just save it as PNG and recenter it yourself, or you can use an option in the RAW open dialog.

![File header size](https://i.imgur.com/2fGamkI.png)

File header size. This essentially shifts the image left by however many pixels you specify in bytes. You don't have to be perfect on the first try; you can just reopen the image over and over, adjusting the file header size until the image is centered. You'll lose a few pixels this way, but one or two lines of pixels in an image with a thousand isn't worth crying over. I'll demonstrate with this picture:

![assorted color wooden frames photo (original)](https://i.imgur.com/yhgE1YJ.jpg)[Photo](https://unsplash.com/photos/-GUyf8ZCTHM) by [Jessica Ruscello](https://unsplash.com/@jruscello) on [Unsplash](https://unsplash.com)

Let's adjust the header size to 500, 1000, and 1500.

![Original color wooden frames photo, shifted by 500 bytes. Approximately a quarter of the image's left side is now being displayed on the right due to wraparound.](https://i.imgur.com/teSZTmz.jpg)![Original color wooden frames photo, shifted by 1000 bytes. Approximately half of the image's left side is now being displayed on the right due to wraparound.](https://i.imgur.com/c0ynnwH.jpg)![Original color wooden frames photo, shifted by 1500 bytes. Approximately three-quarter of the image's left side is now being displayed on the right due to wraparound.](https://i.imgur.com/L10oCed.jpg)

So the offset just shifts the image by some amount left, and anything that would "fall off" the left edge just gets wrapped around to the right.

Now, that was with a planar RAW image. What about interleaved? Let's offset by just 1 and 2 bytes.

![A hue-shifted version of the original colored wooden frames photo. All reds in the original have been shifted to being green.](https://i.imgur.com/YuclGdI.jpg)![A hue-shifted version of the original colored wooden frames photo. All reds in the original have been shifted to being blue.](https://i.imgur.com/U0SemKj.jpg)

Huh? What just happened to the colors? The problem here is that when Irfanview reads the image data, it expects it to be in the order of RGB. When you misalign it, it goes

	            Index  1 2 3 | 4 5 6 | 7 8 9
	   Expected order: R G B | R G B | R G B 
	   Shifted by 1  : - R G | B R G | B R G | B
	   Shifted by 2  : - - R | G B R | G B R | G B
	   Shifted by 3  : - - - | R G B | R G B | R G B
	   (- is blank; read as zero)
It's not reading it in the right order. At index 4, where it expects R, it instead sees the G component when shifted by 1 or the B component at 2. The hue gets shifted by multiples of 60 degrees.

So then you have to increment the offset by 1 over and over trying to hit a number that gives you the "expected order" and therefore the correct colors. If the image hasn't been databent, you basically lock it to the nearest multiple of 3. If it has been databent, you'll have to find out by trial and error to find if the pattern's 3n, 3n + 1, or 3n + 2.

Planar images do not have this issue. Let's do the shift by 1 and 2 again:

![assorted color wooden frames photo shifted by 1 byte. The difference between this and the original is imperceptible.](https://i.imgur.com/T6LFyst.jpg)
![assorted color wooden frames photo shifted by 2 bytes. The difference between this and the original is imperceptible.](https://i.imgur.com/jayNHzy.jpg)

Nothing drastic, just a tiny shift to the left. The colors are preserved.

### Planar images produce more colorful results when databent
This is a more subjective thing, but when I databend interleaved images, I often end up "losing" color. I'll use this picture as my test subject:

![yellow, red, blue, and green feathers photo (original)](https://i.imgur.com/A25Vjup.jpg)
[Photo](https://unsplash.com/photos/Haz8prUXrI4) by [David Clode](https://unsplash.com/@davidclode) on [Unsplash](https://unsplash.com)
(Medium size used)

Let's do the same databend to a planar and interleaved version of the same image: a low-pass filter of 1000 Hz with a roll-off of 48 dB per octave.

Interleaved:

![The same photo as before, but now monochromatic and blurry.](https://i.imgur.com/YVA1hsb.jpg)

Planar:

![The same photo as the original, but now blurry. It is even blurrier than the previous image.](https://i.imgur.com/QszUDA9.jpg)

More detail is lost in the planar version, yes, but that can be mitigated by just adjusting the cut-off of the filter upwards. What you can't mitigate, however, is the total loss of color in the interleaved version. Boost that saturation up as much as you want; you're not getting that color back.

This tendency for interleaved formats to lose color entirely after databending is exactly why I prefer using planar formats.

## Conclusion

This is my way of databending, and I choose to do it this way because this method produces results that I think are good. You can do whatever you want with your art. I encourage you to come up with new and different ways that better suit your purposes.

If there's anything you'd like to add or change, contact me on GitHub at [multiplealiases](https://github.com/multiplealiases). 

