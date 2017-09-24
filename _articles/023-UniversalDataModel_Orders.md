---
layout: article
collectiontitle: Universal Data Model - Orders
title: Universal Data Model - Orders
description: Learn how you can reduce your data schema by combining all the different types of order tables.
permalink: /Universal-Data-Model-Orders.html
published: true
---
In [part one](http://filemakerinventoryresources.com/Universal-Data-Model-Company.html) of this series I discussed how a Universal Data Model can reduce data schema by combining the vendor and customer tables into one table called Company.

In this article I’ll discuss the same concept with the various order tables.  Order tables are composed of two components:
1. Header table
2. Line items table

A Purchase Order, then, would have a Purchase Order Header and Purchase Order Lines.  Sometimes the “Lines” table is called “Details”.  It may have other names as well, but “Lines” and “Details” are the ones I’m most familiar with.  The idea is that it is a parent-child relationship.

![](http://newleafdata.com/images/Header_Lines.png)

In an inventory management system there are a number of order tables and they all follow the same pattern of a Header-Line (or parent-child).  For example:

* Purchase Order
* Sales Order
* Loan Order
* Work Order
* Assembly Order
* Shipping Order

Presuming a large overlap of fields in the Header and Lines tables, then these can be combined to an Order and Order Line table.  This reduces redundancy in the data schema.

![](http://newleafdata.com/images/UniversalDataModel_Orders.png)

As you probably guessed, there are a few things to keep in mind:
1. With more tables merging to one, there is a higher likelihood that you’ll have fields for only a particular table.  This is not inherently bad, but it can have an adverse affect on performance, which I’ll address below.

## What’s Wrong with a Lot of Fields?
A FileMaker table can have a lot of fields.  “A lot” is a relative term, but **256 million** is a lot of fields to me.  Remember, that’s not 256 million records; it’s 256 unique columns for a single record.  Technically it is 256 million fields over the life time of the file.  Some fields will inevitably be created and deleted.  But still, 256 million fields minus a few hundred that are created and removed over the course of the database’s life IS STILL A HECK OF A LOT OF FIELDS!

So what’s the problem?  If FileMaker can handle all those fields, why be concerned about the number of fields in a table?  In a word: Performance.

When a record is called (via navigation, query, etc.) FileMaker returns all the data of that record.  For example, let’s say that you have an Order table and you’re trying to find out the status of that particular record.  You don’t need any other data - just the status.  You might create a layout like this:

![](http://newleafdata.com/images/Simple_Search_Layout.png)
I used to do this.  In fact, in a few older systems, I have blank layouts.  There is nothing on the layout at all - no fields, no buttons, no design.  I even made the layouts small (but visible).  Turns out, this really didn’t matter.

Back to our scenario, when the “Perform Find” button is clicked FileMaker will return all the data from all the fields, even though there are only two fields showing on this layout.  If there were 20 fields for that table, we’d receive 20 data points for this request.  If there were 200 fields in that table, we’d receive all 200 data points.  And if we had 256 million fields in the table…

You get the point.

So before you combine all the order tables into one as per the Universal Data Model, be aware of your field count.  If you do have fields that are unique to a particular order (e.g. fields that are only used for purchase orders), one option is to move those fields into a new table called something like Order Annex (in my naming convention, I would call it ORDERANNEX).  You can then create a one-to-one relationship with the order table.  That keeps the tables narrow and still allows you to combine them into one unified orders and orders line tables.

For a more in-depth analysis of FileMaker Performance when calling records, [see this SlideShare](https://www.slideshare.net/fmkonferenz/fmk2014-file-maker-performance-under-the-hood-by-jon-thatcher) by [Jon Thatcher](https://www.linkedin.com/in/jon-thatcher-3483321/).  And you can learn more about FileMaker’s impressive capacity numbers on their [tech spec page](http://help.filemaker.com/app/answers/detail/a_id/16373).

## Relating to the Same Table
This technique involves a certain level of abstraction and somethings may not be obvious.  For example, foreign keys are pretty straight forward with multiple order tables.  Let’s presume a scenario where you have a Sales Order and one of the items is out of stock.  This triggers a new Purchase Order.  The layout might look like this:
![]http://newleafdata.com/images/SalesOrder_outOfStock.png)

And the header table relationship can be viewed this way:
![](http://newleafdata.com/images/SO_to_PO_relationship.png)

But if we have all the orders in one table, then the header table might look like this:
![](http://newleafdata.com/images/Orders_All.png)

This can be a little tricky, particularly if you are new to relational database development, regardless of platform.

## What about other Header-Line tables?
To keep the concept simple, I’ve only discussed combining order tables, but you can really do the with any table that has a header and lines such as these:

* Invoice - Invoice Line
* Receipt - Receipt Line
* Putaway - Putaway Line
* Pick - Pick Line
* Pack - Pack Line

Two things to keep in mind:
1. Be aware of the number of fields in your table, as I mentioned above.  Narrow tables usually have a better performance.
2. Combining tables involves a certain level of abstraction.  Make sure it is clear to you and other developers (including future developers) how to manage the abstraction.

## Conclusion
I like the Universal Data Model but it’s not a one-size-fits-all.  As with everything in custom app development, your business goals should determine your methodology.
