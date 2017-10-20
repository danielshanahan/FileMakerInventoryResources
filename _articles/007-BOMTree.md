---
layout: article
collectiontitle: BOM Tree
title: BOM Tree
description: A Bill of Materials (BOM) tree structure that enables users to see all products components at once.
permalink: /BOM-Tree.html
created: Created 2017-03-11
---
If you haven’t read the article on Bill of Materials (BOM), you may want to take a look at that before reading about the tree.  The difference between a BOM and a BOM tree is that the former is only one level deep and used to build the BOM (which is discussed in the BOM article).

A Bill of Materials (BOM) tree is a list of all the components of an assembled item.  It is multi-leveled.  At a glance you can see everything necessary to make a product or a kit.  In addition to showing all the components of an assembled item, the BOM Tree can also display important quantities to help with planning.  For example, a BOM tree might show:  

1. Quantity required.
2. Quantity available.
3. Quantity that can be made.
4. Quantity that can be sold.
5. Quantity needed for fulfillment.
6. Lead time for receiving.

![Bill of Materials Tree in FileMaker Pro]({{ site.baseurl }}/assets/images/fmp_BOM_Tree.png)<br />
*Data is from [http://www.arenasolutions.com/resources/articles/excel-bill-of-materials/](http://www.arenasolutions.com/resources/articles/excel-bill-of-materials/)*

## Quantity Required

This is the quantity that is required to make one unit of a component.

## Quantity Available

This is the quantity that can be pulled off the shelf and immediately used in building the component.  If the organization doesn’t assemble until an order is received (a Pull system) then the only items in the tree that will have an available quantity are the raw materials.



## Quantity That Can Be Made

The quantity available divided by the quantity required provides the quantity that can be made.  For example, if we have 12 bike tires we can make 6 bicycles because each bicycle takes 2 tires.

## Quantity That Can Be Sold

Sometimes subassemblies are created in advanced, even in a Pull system.  In those cases, the subassembly has an available quantity and this is added to the quantity that can be made.  This gives us the quantity that can be sold.



## Quantity Needed for Fulfillment

In a BOM tree we can tell how many items we can make and how many we can sell based on the available quantity.  But what if an order comes in?  Can the BOM tree help determine what is needed?  YES!  In the image below, this number is the NTF - Need to Fulfill.  This number calculates the number requested with what can be sold.



## Lead Time

Materials have different rates in which they are available.  The time it takes a vendor to supply raw materials or the time it takes an internal time to assemble a component can all be stored in the BOM tree.  This can help determine the amount of time it takes to receive and fulfill an order.

##### Complex Quantities

A couple of other quantities can be useful, though they add to the complexity of the tree: **Quantity on Order** and **Quantity on Backorder**.  The Quantity on Order can tell the user what is coming in and when (presuming the order has an Estimated Time of Arrival or ETA).  The time component is what makes this quantity more complex.  The organization may have more than one order active at a time, with different ETAs.  For example, an item on the tree might show a Quantity on Order number of 750 but it could be three different orders of 250, coming over the next week.

Likewise, if an item is backordered - either for an external or internal customer, then that will affect the Quantity on Order.

If you are thinking about building a BOM tree for your inventory management system, here are three things to keep in mind:

1. The quantities are snapshots
2. Repeated items should have a different availability
3. The unit of measure matters

## Quantities Are Snapshots

Everything is based on what quantity is available and that number is (hopefully) often changing.  Work order, sales orders, and receipts will all affect this number.  So it is best to consider the quantity numbers as snapshots.  For this reason, it may be best to have a button/script that can quickly create the tree to update the numbers as needed.



## Repeated Items

Some items may appear more than once in the BOM Tree.  That means the available quantity for that item cannot simply use the quantity available field for that item (to learn more about item quantity fields, see the article on Quantity Fields).

For example, suppose a subassembly uses 6 bolts and the quantity available for that bolt is 3,000.  That bolt is also used in another subassembly for the product.  However, we can’t just use the quantity available field because 3,000 is no longer an accurate number.  6 were used in a previous subassembly so the true quantity available number is: <br>
QUANTITY::available - 6 = 2,994.

## Units Of Measure Matter

I haven’t mentioned the unit of measure in the above discussion of BOM Tree quantities but units of measure definitely matter.  Like the previous section on Repeated Items, units of measure can alter the quantity available.  Consider an item that has a component that requires 2 inches of strapping material  However, that item is not purchased in inches but in yards.

To accommodate this, we need a unit of measure conversion table.  The conversion table contains fields for the two units as well as an equivalent numerical value.

![Unit of Measure conversion table in FileMaker Pro]({{ site.baseurl }}/assets/images/fmp_uofm_conv.png)

Two entries for each equation makes it easy to go from one unit to another.
