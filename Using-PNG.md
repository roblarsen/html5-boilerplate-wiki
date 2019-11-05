# Notes on using PNGs
There are some things to keep in mind when using the PNG image format.

PNG comes in three flavours; 8-bit, 24-bit and 32-bit. A 32-bit PNG is a 24-bit PNG with a transparent alpha channel. [8-bit PNGs with transparency are recognized mostly by IE6](http://www.sitepoint.com/png8-the-clear-winner/ "PNG8 - The Clear Winner"), while transparent 32-bit PNGs are not. So, you can do one of three things with transparent/semi-transparent 32-bit PNG images for IE6:

1. Convert to 8-bit PNGs.
2. Use the included DD_BelatedPNG script to render them correctly.
3. If the transparent region is going to be in front of a solid color, you can use [TweakPNG](http://entropymine.com/jason/tweakpng/) to make sure that the 32-bit PNG renders a particular color instead of the typical grey-blue color for transparent regions. You can set the color by editing the "bKGD" attribute.

### Convert to 8-bit PNGs

8-bit PNGs are mostly smaller than the 32-bit variety. There is no discernible difference in how smooth the transparent regions are in all browsers except in IE6 where the edges appear jagged.

[pngquant](http://www.libpng.org/pub/png/apps/pngquant.html) is a command line utility that can convert 32-bit PNGs to 8-bit ones. [There is a GUI](http://pngmini.com) and a [png8](https://github.com/perifer/png8) command line tool (OS X only) that wraps pngquant and some other utilities to make it easy to create 8-bit PNGs.

### Using DD_BelatedPNG

DD_BelatedPNG script uses VML to render the transparency and is useful if you have few PNGs on which the script needs to work on. If you have a page with scores of PNG images, using DD_BelatedPNG might end up slowing down your page. You could either target a small set of PNGs for it, or convert them into 8-bit PNGs.

### Additional optimization
Here are some techniques and guides:

 * [Clever PNG optimization techniques](http://www.smashingmagazine.com/2009/07/15/clever-png-optimization-techniques/)
 * [Clever JPEG optimization techniques](http://www.smashingmagazine.com/2009/07/01/clever-jpeg-optimization-techniques/)
 * [How to Optimize PNG and JPEG without Quality Loss](http://www.splashnology.com/article/how-to-optimize-png-and-jpeg-without-quality-loss-part-1/2071/)
 * [PNG Optimization Guide: More Clever Techniques](http://www.smashingmagazine.com/2009/07/25/png-optimization-guide-more-clever-techniques/).
 * [Image Optimization: 9 best practices and tools](http://www.slideshare.net/stoyan/image-optimization-for-the-web-at-phpworks-presentation).
