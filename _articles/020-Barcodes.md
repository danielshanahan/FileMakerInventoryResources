---
layout: article
title: Barcodes
description: Some thoughts about implementing barcodes into your FileMaker inventory system.
permalink: /Barcodes.html
created: Created 2017-06-29
---
![Barcode](http://newleafdata.com/images/FMIR_barcode.jpg)

*Barcode image from [Morguefile.com](https://morguefile.com/search/morguefile/1/barcode/pop)*

## Overview
Barcodes and inventory go together like chocolate chips and  - well, anything else, frankly.  Barcodes have been [around since the 1960s and began their wide use in the early 70s](https://en.wikipedia.org/wiki/Barcode).  The earliest references I could find regarding barcodes and FileMaker Pro were on forums regarding FileMaker Pro 5.5:

[Where to Load Barcode Font](http://fmforums.com/topic/17311-where-to-load-barcode-font/)
[Bar Codes for inventory control](http://fmforums.com/topic/8348-bar-codes-for-inventory-control/)

If you know of barcode use in FileMaker Pro prior to that, let me know.  Also, if you can describe the steps it took to implement barcodes in FileMaker Pro 5.5, let me know that as well.  It doesn’t really affect users of current versions of FMP, but I think it would be interesting in a trivial sort of way.

This article is not a tutorial on how to implement barcodes into your FileMaker solution.  Those articles and videos already exists.  And there are a lot of them.  Go to your favorite search engine and enter “FileMaker Barcode” and you’ll find a plethora of blogs and videos.  The implementation spans from using a barcode font, plugins, and web viewers.  You’ll find solutions that are free and others that are priced.  There’s no shortage of barcode information for FileMaker Pro.

## The Nitty Gritty of Barcodes
If you’re going to implement barcodes system into your inventory system, it would be good to have an overall view (or “big picture” if you will) of what is involved.  One way to think about this is to categorize the process in four sections:

### 1. Planning
Determine the type of barcode.
Determine the scanning device.
Determine the size of the barcode printed label.

### 2. Data Entry
Barcode the location records in the software.
Barcode the item* records in the software.
Barcode the asset* records in the software.
Barcode the people records in the software.

### 3. Printing
Purchase a specialized label printer (if needed).
Print the barcode labels.
Label the locations.
Label the items.
Label the assets.
Label the employee card.

### 4. Using the Barcodes
Once you’ve planned and carried out the barcode plan, it’s time to scan the items, locations, assets, and people as items move throughout the storage facility. 

\* For the sake of this article, I’m considering items as products that are either resold or used in an assembled product that is resold.  Assets are things that help you resell items or make a product (e.g. pallets, forklifts, shop tools, etc.).

## Barcode Alternatives
There are a number of alternatives for barcodes (again, use your favorite search engine and type: “Future of Barcodes” if you have a couple of hours on a rainy afternoon).  Some very promising technologies have even prompted some to declare the demise of the barcode.  But that shouldn’t deter you from using barcodes.  It is still a strong technology. (This is my opinion, based on what I’ve read and on my experience with various warehouse visits.  To my knowledge, there is no central location of barcode usage data/measurements over the years.  If you know of such a site, please let me know.)

I won’t go into much detail here, primarily because I haven’t fully researched the alternatives and I haven’t come across many in the FileMaker community asking for barcode alternatives.  Nonetheless, a few alternatives seem interesting:

### Radio Frequency Identification (RFID)
RFID is a radio frequency emitted from a tag.  Capturing the emitted frequency requires the use of a device, however, unlike barcodes, the RFID device doesn't need to scan each tag individually.  Also, the device does not need to be mobile.  A fixed location device might be used in receiving or shipping where product would pass by (on a conveyer, via lift truck, etc.).

### Voice Directed Warehousing (VDW)
This was initially limited to picking, so you you will find information on this if you perform a search for “Voice Picking.”  However, this is now a misnomer as the technique can be used in any part of the inventory process.  The idea is that a user reads the numbers into a microphone mounted on a headset, which, of course, is connected to a mobile software device.   

VDW can be a great choice when working in climates where it is difficult to use fingers on the mobile software device (such as refrigerated storage facilities).  Obviously, it may not be suitable for environments that are very loud.

### Smart Glasses and Warehousing
This is a relatively new technology and there appears to be some implementations with enterprise systems such as SAP.  The videos look pretty cool but everything I saw was produced by the smart glasses manufacturers so its hard to tell how effective they are.  It appears to be a nice hands-free method and the promos make a convincing argument for accuracy.  However, it doesn’t appear to be any faster than barcode scanning, from what I can tell.  Still, the fact that companies are pursuing this technology and implementing it with enterprise inventory systems can be beneficial for Small-Medium Businesses in the near future.

## Do You Need Barcodes
The point of this article is to help illustrate the intricacies of deciding whether or not to use barcodes.  But with proper planning you can definitely use this technology to help reduce inventory errors by increasing the accuracy of inventory usage and location.
