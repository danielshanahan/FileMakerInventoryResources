- - - -
layout: article
title: Variants
description: Creating product variants in FileMaker Pro
permalink: /Variants.html
- - - -

# Variants
![](../_assets/Variants.png)
*Icons made by Freepik from www.flaticon.com is licensed by CC 3.0 BY*

Some products have multiple options.  The options can be a different color, size, etc.  Each instance of an option - for example, a large blue widget, a large green widget, a large yellow widget, etc. - is known as a variant.

I’ll divide this article into two parts:
1. Variants by the Numbers - Rule of Product
2. FileMaker Demo

## Variants by the Numbers - Rule of Product
Each variant option is known as an attribute and there is no hard and fast rule as to how many attributes a variant might have.  Well, that’s not exactly true; off-the-shelf products usually have an attribute limit.  But in FileMaker, there needn’t be a limit, but I’m getting ahead of myself.

**Attributes**
A single product can have a list of attributes.  To illustrate, I’ll use the tried and true example of shirts.  And to make the illustration a bit extreme, I’ll give the shirts a lot of attributes.  Before we can define the attributes of the shirt we need a “Master Product”.  Our Master Product will be intentionally generic in order to demonstrate attributes.   Our Master Product is a T-shirt.  Our T-shirt product can have the following attributes:

Gender
Brand
Composite
Color
Size
Sleeve Length
Neck Cut

**Attributes Can Add Up**
Each of those attributes can have choices, which are displayed here as a set:

Gender: {Female, Male}
Brand: {Hanes, Fruit of the Loom, Jockey, Calvin Klein}
Composite: { 100% Cotton, 80/20 Blend, 50/50 Blend, 20/20/60 Blend}
Color: {Blue, Green, Red, White, Black, Yellow, Purple, Orange, Brown}
Size: {X-Small, Small, Medium, Large, X-Large, XX-Large}
Sleeve Length: {Short, Long, Tank}
Neck Cut: {Crew, V}

Here are the totals of the set:

Gender: 2
Brand: 4
Composite: 4
Color: 9
Size: 6
Sleeve Length: 3
Neck Cut: 2

The total number of variants is the Rule of Product.  That is, the total number  of potential items we can sell is the product of all the attributes.  Note that “product” here is used in its mathematical sense and not as a synonym with “item”.  In our example, then, the total number of individual variants for a T-shirt is

2x4x4x9x6x3x2
or
2*4*4*9*6*3*2
=
10,368

**Extreme Example Explained**
This is admittedly an extreme example though not entirely unrealistic.  We could have just as easily named our Master Product as Hanes 100% Cotton Mens’ T-shirt which would have significantly reduced the attributes.  The point here is two-fold:

1. How you determine your Master Product is up to you.
2. The number of options can become a large number very quickly.

Now the fact that our example has 10,368 variants is not a problem for FileMaker.  The FileMaker platform is robust enough to handle 100x that and more.  The issue is, do you need to create a record for all 10,368 variants?

**Make to Stock vs Make to Order**
Make to Stock - or Push Inventory Model - creates all the inventory, stores it, and then sells it.  First it’s made, then it’s sold.  If your organization operates on a Make to Stock Inventory Model, then you’ll need to make 10,368 new records in FileMaker.  Keep in mind that these records are all unique since they each have a unique combination of the available options.

As previously mentioned, 10,368 may be an extreme example.  It may be that you never need to create more than 9 new variants; for example:
Color: {Blue, Green, Red}
Size: {Small, Medium, Large}

Manually creating 9 new records isn’t time consuming, but creating 10,368 new record is.  The accompanying demo automates the process of creating new variant records.

Returning to stock inventory models, it may be that you do not create inventory until *after* you’ve sold it.  Once sold, the materials can be purchased and assembled (or simply purchased if you are a distributor).  This is a Make to Order or Push Inventory Model.  In that scenario, you may not want to create the 10,368 variant options.  Instead, you may chose to create the variant record as it is sold.

Extreme Example of Rule of Product
	Master Product
	Master Price/Cost
	No Quantities

## FileMaker Demo
The variant demo file shows one way to create variant records.  The set up is deliberately simple.  The ITEM table has the following fields:

1. A boolean field to indicate whether this is record has variants.*
2. A field for the variant list.  The formatting for the list is hard-coded in the script so make sure that it follows this formula: 
Variant Label: {Option 1, Option 2, Option 3, etc.}
3. A field for the attributes.  These occur only on the variant record and not the Master Item.
4. A foreign key field to relate the newly created variant items to the Master Item (ITEM::masterItemID)
5. A timer field.  This is not required but I was curious how long it would take to create 10,368 variants so I added a field to store that information.  (FWIW - it takes me about 40 seconds on a local file on my MacBook Pro i7, 16GB RAM)

*The checkbox for the UI is a button.  The idea comes from Mark Scott’s excellent series on Button Bars ( [Fun with FileMaker Button Bars: Check Please – beezwax > blog](https://blog.beezwax.net/2016/04/28/fun-with-button-bars-check-please/)).

**Master Item**
The Master Item is the product from which all other product variants are built.  The variant list is visible only on the Master Item and the attributes field is visible only on the variant items.  In the demo the Master Item also serves as the cost and price source.  This data is passed on to all the variant records.  Obviously, you’ll need to modify for your particular situation.  For example, if green dye is more expensive than any other color, your cost and price may vary from the Master Item.

Item quantities are visible only for the variant records, not the Master Item.  The idea is that the Master Item is never sold.  Rather, it serves as the one item from which all other variants derive (it’s a bit of the [platonic idea of form and matter](https://en.wikipedia.org/wiki/Theory_of_Forms) ).

The process of selecting a variant can be more involved, but I’ve chosen to keep it simple for the demo.  You might, for example, choose to create a table of potential variants and display them in a portal with a dropdown for the variant label.  The online inventory system, Odoo, does something similar to this: https://i.ytimg.com/vi/37hnxObMgmk/maxresdefault.jpg.

**Scripts Overview**
Three scripts are involved in created the variant records:
1. A calling script
2. A searching script
3. A creating script

**Calling Script**
The calling script simply sets up the timers, clears out any legacy data, sets global variables, and calls the searching script.

**Searching Script**
Honestly, I’m not sure what to call this script.  It is not a searching script as in a query or a find.  Rather, its a bit like a mouse in a maze.  The script runs through each iteration of all the variant options and puts them together.  The processes is recursive using both a Loop/End Loop as well as calling itself.  An If/Else condition tells the script to either:
1. Go Deeper
2. Create the new record

Here is a GIF to help visualize what the script is doing.  The GIF only goes through a few iterations, not all 10,368!

![](Variants/Variants.gif)


**Creating Script**
Once the variant combination is determined, this script goes to the Master Item and duplicates it.

The script is dynamic in that you can create any label and variant options and as long as you followed the format (see “FileMaker Demo” #2 above) the script will (or SHOULD) produce the correct number of variant combinations.

Final Note
I put an SKU field in the table as well and had the script grab the first letter of each option.  For some options such as Color: {Blue, Black, Brown} and Size: {X-Small, X-Large, XX-Large}, this will produce duplicate SKUs.  Obviously, you’ll want unique SKUs and there are a number of ways to get a unique SKU:
* Use a code and create a reference table with the code (e.g. Blue=01).
* Manually put a mark in the set and grab all letters to that mark.  So Color: {Blue, Green, Red, Black} might be marked Color: {Blue^, Green, Red, Black^} and an added condition would be used as the script processes each option:
`If ( PatternCount ( $attribute ; “^“ ) ; Left ( $attribute ; 3 ) ; Left ( $attribute ; 1 )`

## Conclusion
Variants are ways to offer options to a base product.  You can add those variant items to your FileMaker Inventory database and in fact, FileMaker can handled fairly large data sets for small and medium size businesses.  The accompanying demo is one approach to creating variants in FileMaker Pro.

Good luck!
#fmir
