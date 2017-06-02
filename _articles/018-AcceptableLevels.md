---
layout: article
title: Acceptable Levels of Goods Received
description: Sometimes when you receive goods, close enough works!  This article has 5 ways you might use to receive goods at an acceptable level.
permalink: /AcceptableLevels.html
published: true
---

![](Goods%20Received%20-%20Acceptable%20Levels/fmp_AcceptableLevels.png)

This article is based on a question from John R. who wrote: 
*What about the scenario where say 10,000 are ordered but our supplier agreement says that anywhere from 9,500 to 11,000 is an an acceptable fulfillment of this PO? Often the case in printed boxes or rolls of material which have been taken off larger reels and the suppliers end*

Thanks John for the question.  Just a  reminder, if you have a topic that you’d like to see address, make sure to send that to me via the form on FileMaker Inventory Resources’ home page.  And now, on to Supplier Agreement levels !

There are a number of components that can go into a supplier agreement contract.  This article is only looking at the acceptable levels of goods received.  As John mentioned, a company orders a particular number but is willing to accept a range below and above that number.

Agreement levels can be measured in two ways:
1. Numeric values
2. Percentage

Of those two ways, your supplier agreement level may have one of three possibilities by which to measure acceptable levels:
1. Numeric value only
2. Percentage value only
3. Numeric and percentage value

So the first thing to figure out is how we are going to measure acceptable levels; by number, by percentage, or by both.  It’s my understanding that there is very little overhead in FileMaker Pro to have a field in a table that is empty.  For that reason, I think having both number and percentage is a good way to go.

Now we need to figure out how many fields we need.  For example, in John’s scenario he has a low and a high.  Moreover, his low number is 500 below the ordered number and 1,000 above.  So  we can’t have one field that is ±500 or ±1,000.  Two separate fields would work best here, one to capture the low value and one to capture the high value.  

If, on the other hand, the low was 9,500 and the high was 10,500 then we could get by with on field and that field would have a value of ±500.  This is true whether the acceptable difference is a number quantity or a percentage.  However, it is only true if it applies to all scenarios.  As soon as you have a low and high that are different, you’ll need two fields.

You may also want to store a field for the unit of measure.   In the demo files I presume that the unit of measure is the same for both the low and high value.  I also presume that unit of measure is the same for the order line item.  It may not be, of course, in which case you’ll need to adjust for that.  The [article on Unit of Measure](http://filemakerinventoryresources.com/Unit-of-Measure.html) may be helpful, particularly if you need to convert one unit to another.

We’ll also need two fields in the Purchase Order Line table.  This will be a low and a high field and should auto-populate based on the quantity and the acceptable number.  For example, if the acceptable numbers are 500 and 1,000 (low and high), then the Purchase Order Line tables would be:

PURCHASEORDERLINE::acceptableQtyLow = PURCHASEORDERLINE::quantity - 500

PURCHASEORDERLINE::acceptableQtyHigh = PURCHASEORDERLINE::quantity + 1000

or, if the acceptable value is a percentage, then:

PURCHASEORDERLINE::acceptableQtyLow = PURCHASEORDERLINE::quantity - (PURCHASEORDERLINE::quantity * .03)

PURCHASEORDERLINE::acceptableQtyHigh = PURCHASEORDERLINE::quantity + (PURCHASEORDERLINE::quantity * .03)

The next thing we need to figure out is where to put the number and percentage fields that will be used to determine the high and low range.  There are several options (not exhaustive):

**Admin table** - *percentage* - best when it applies to all goods received, regardless of item or Supplier.

**Purchase Order table** - *numerical value or percentage* - best when acceptable levels vary from order to order and consistent among all the line items.  If the acceptable level doesn’t apply to a particular line item, then a flag/boolean field in the Order Line table can be used to indicate that.

**Item table** - *numerical value or percentage* - best when the acceptable discrepancy is the same percentage for a particular item from all Suppliers.

**Supplier table** - *percentage*-  best when the acceptable discrepancy is the same percentage for all goods received from a particular Supplier.

**ItemSupplier table** - *numerical value or percentage* - best when a particular item has a different acceptable level among different Suppliers.

A demo file of each of these five scenarios is available for download (for free, of course).  A link to demo files is included in the email announcement of each article as well as the welcome email when you sign up.  If you can’t find either of these emails with the link, just let me know and I’ll make sure you get it.
