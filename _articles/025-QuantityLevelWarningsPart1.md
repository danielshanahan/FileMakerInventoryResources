---
layout: article
collectiontitle:  Quantity Level Warnings - Part 1
title:  Quantity Level Warnings - Part 1
description: Setting up the data structure for quantity level warnings.
permalink: /Quantity-Level-Warnings-Part-1.html
published: true
created: Created 2017-08-23
---

## Intro
One of the problems with inventory is that it is difficult to know how much to store.  If you run a push system - that is, you purchase or build before you receive a customer request - then storing inventory can be a combination of science and art.  In this two part series, I’ll describe warning systems that may be helpful as you maintain your inventory quantity levels.  Part 1 will focus on time periods for low/high data and where to store that data.

## Quantity Levels - Low or High
Mostly we think of having warnings with quantity levels are too low.  After all, if the quantities are low than sales may suffer (i.e. potential customers go elsewhere).  But quantities that are too high are also problematic.  Onsite inventory has a carrying cost and a storage cost.  If the quantities are too high, then those costs will likely increase.  For this reason, you may want to add two fields to your schema.  (Note: I’m using FileMaker’s TABLE::field naming structure along with my own convention of tables in capitals and fields in lower camel case [e.g. lowerCamelCase]).

Where you put those fields depends on a couple of things:
1. Do you have multiple locations for this item?
2. If you do have multiple locations for this item, do you want to track the low/high quantities at the location level or the overall level?

Let’s take the scenario where there are multiple locations and we want to know the low/high value in each location.  We could store that information in the warehouse table:

WAREHOUSE::qtyLowThreshold
WAREHOUSE::qtyHighThreshold

If you’re using a quantity table, as described in the [Updating Inventory article](http://filemakerinventoryresources.com/Updating-Inventory.html), (“Where to Locate Quantity Fields”, option 3), then the fields would go in the quantity table:
 
QUANTITY::qtyLowThreshold
QUANTITY QTY::qtyHighThreshold

Now let’s take the scenario where we either want to know the overall low/high of all locations or we only have one location.  In either case, we will store the low/high fields on the item table:

ITEM::qtyLowThreshold
ITEM::qtyHighThreshold

For simplicity’s sake, the rest of this article will describe this latter scenario, so the low/high quantity data will be stored in the item table.

## Track over time
Let’s say we’re selling a blue widget.  Is 3 each too low?  Too high?  What about 1,003?  Again, is that quantity too low or too high?  The answer is, we don’t know.  The low/high numbers need context and that context is time.  When you first set the low and high quantities, you’ll probably be guessing at first, unless you have data to indicate what those values should be.  Once you have data over time, you’ll be able to refine your accuracy with the low/high numbers.

That, of course, begs the question, how much time?  Are you tracking the low/high numbers over a year?  Over the quarters?  Over the months or weeks?  It will depend on the product your selling and the seasons that item peaks.

Let’s return to our low/high fields.  And let’s say that the low/high can change by the quarter.  Now, our fields might look like this:

ITEM::qtyLowThresholdQ1
ITEM::qtyLowThresholdQ2
ITEM::qtyLowThresholdQ3
ITEM::qtyLowThresholdQ4
ITEM::qtyHighThresholdQ1
ITEM::qtyHighThresholdQ2
ITEM::qtyHighThresholdQ2
ITEM::qtyHighThresholdQ2

The low/high could just as easily be tracked by the month or by the week:
ITEM::qtyLowThresholdJan
ITEM::qtyLowThresholdFeb
ITEM::qtyLowThresholdMar
etc.
ITEM::qtyLowThreshold01
ITEM::qtyLowThreshold02
…
ITEM::qtyLowThreshold52

Anything other than annual (qtyLowThreshold and qtyHighThreshold), I would store in a separate table and create a one-to-one relationship.  When FileMaker pulls down a record from the server, all the fields of that record are pulled down.  This can cause performance issues as fields are added to the table.  One way to keep performance optimal is to move some fields to another table and create a one-to-one relationship.

By the way, I highly recommend learning more about narrow tables and using one-to-one relationships.  Here are a couple of resources:
1. [Normalization vs denormalization thread](https://community.filemaker.com/thread/79676) in FileMaker Community forums.
2. Jon Thatcher’s slide show “[Performance - Under The Hood](https://www.slideshare.net/fmkonferenz/fmk2014-file-maker-performance-under-the-hood-by-jon-thatcher)” for 2014 FileMaker Konferenz.
3. Jon Thatcher’s presentation at FM DevCon 16, [Core: Under the Hood - Server Performance](https://youtu.be/VCrNP4VZiM4?t=48m11s) (starting at 48 minutes and 11 seconds).

## Lowest common denominator
What if you have some items that are stored by the quarter and some by the week?  In my opinion, it would be too difficult to use more than one time control.  Therefore, I’m in favor of using the lowest common denominator.  In this case, that means all items have a low/high for 52 weeks, even if that number only changes once for a 12 week span.

## Normalize or Not
If you are keeping more than one low and one high (e.g. per quarter, month, or week), you may want to normalize the data.  Or, you may not.  Sound ambiguous?  Honestly, I’m ambiguous about it. Normally, I normalize (pun intended).  So the related table might look something like this:

![](http://newleafdata.com/images/qtyThresholdTable.png)

The “Term” field is by month in this example but could easily be Q1, Q2, etc. for quarter or 01, 02…52 for weeks.

Having said that, I don’t know that I would normalize the data.  It is not clear to me that normalizing the data would have any performance benefits.  First, a little math.  Let’s say we have 1,000 items and we are marking the low and high quantities by the week.  There are 52 weeks and each week has a low and high so we need 104 related records per item. 1,000 items x 104 related records = 104,000 records in that related table.  The table, by the way, would have a handful of fields such as those shown above plus any housekeeping fields like Created, Created On, Modified, Modified On, etc.

On the other hand, we could keep all the related data in one record.  Again, 52 weeks x low and high = 104 fields.  Add our housekeeping fields and we’re at roughly 110 fields in that related table.  Since we’re tracking the low/high values in one record, 1,000 item records = 1,000 low/high records.  That table might look like this:

![](http://newleafdata.com/images/qtyThresholdTableUnnormalized.png)

These numbers would all be considerable less if we were tracking by the month (12 x 2 = 24 fields) or the quarter (4 x 2 = 8).

There is at least one factor that would sway me to store the data in a normalized form rather than an unnormalized form: if we were retaining our data annually.  In other words, we want to know the data from last year and the previous year and 5 years ago.  In that case, I would normalize the data and add a year field, like this:

![](http://newleafdata.com/images/qtyThresholdYear.png)

Which could then be queried to return something like this:

![](http://newleafdata.com/images/qtyThresholdYearQueried.png)

## Conclusion
Inventory level warnings can be as simple or complex as your organization needs.  A warning for low inventory is the base but you may also want to set a warning for quantities that are too high.  And you’ll need to decide if those quantities should change over time (within a given year).

Once that is completed, you can move onto the warning methods, which I’ll cover in part 2 of this series.
