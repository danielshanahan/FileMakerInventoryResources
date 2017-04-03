---
layout: article
title: Unit of Measure
description: Understand the meaning of item quantities
permalink: /Unit-of-Measure.html
published: true
---
git
Everything we do with inventory is done with a quantity and a unit of measure.   The unit of measure might measure weight, length, area, volume, time, temperature, etc.

 If you’re currently not recording the unit of measure then you’re likely dealing with inventory on an individual basis with an implied unit of measure such as “each”.

There are a number of places where a unit of measure is used such as item quantities, purchasing units, selling units, work order units (for assemblies), etc.

### Implied Unit of Measure
If you are not storing the unit of measure in your item table then the unit is implied.  Inventory quantities with an implied unit of measure might look something like this:

On Hand: 100
Allocated: 20
Available: 80
On Order: 1,000

Without a specified unit of measure the assumption is that the number is a single unit.  That might mean 100 each or 100 pairs or 100 boxes or 100 pallets.

If everything moves in the same unit of measure - that is, everything is purchased and consumed in the same unit of measure as your current inventory count - then you may not have run into any issues. 

### Units of Measure Issues
You may count your widgets as “each” but purchase them by the case.  This creates an issue.  Really, there are two issues here so let’s simplify this by changing the item from widget to cocoa powder (I’m drinking a glass of chocolate milk as I write this and felt inspired!).  

**Issue 1 - Different Units of Measure**
Let’s say we count our cocoa powder in grams.  Our inventory count might look like this:

On Hand: 100 grams
Allocated: 20 grams
Available: 80 grams
On Order: 1,000 grams

We purchase the cocoa powder from our supplier in kilograms but we sell it in grams.  This is our first issue.  One way to resolve this is to use a Unit of Measure Conversion table.  We can call upon this reference table to ensure we have the proper quantity and unit of measure for the process we need.

An example of a row in the Unit of Measure Conversion table might look like this:


| UofM 1     | Qty 1        | UofM 2     | Qty 2        |
|--------|--------|--------|--------|
| Grams       |  1              |   Kg	    |          .001  |


In our example, we want to show that we have 1,000 grams on order.  We want to see that number in grams because it will help the sales people, especially if we’re out.  However, the purchase order line will have the unit in kilograms because that’s how our supplier sells it.  To get from 1,000 grams to kilograms, all we need to do is go to our reference Unit of Measure table, find grams and convert it by multiplying the value in Qty 2 field:

1,000 grams * .001 = 1 kilogram.

When the product is received and put away, another conversion occurs, changing 1 kilogram into 1,000 grams.  Since a conversion is required back and forth, I like to have two entries for most units:

| UofM 1     | Qty 1        | UofM 2     | Qty 2        |
|--------|--------|--------|--------|
| Grams       |  1              |   Kg	    |          .001  |
| Kg       |  1              |   Grams	    |          1,000  |


**Issue 2 - Generic Names**
Converting a gram to a kilogram is pretty straight forward because it is clear what a gram is.  The same is true for kilograms, teaspoons, second, minutes, liters, gallons, etc.  However, not all units of measure are so clearly defined.  Let’s return to our widget.  We sell our widgets individually but we buy them by the case.  And let’s say there are 6 in the case.  So far, so good.  Our Unit of Measure conversion table would have the following rows:

| UofM 1     | Qty 1        | UofM 2     | Qty 2        |
|--------|--------|--------|--------|
| Each         |  1               |   Case	    |      .166      |
| Case         |  1               |   Each	    |       6          |


Now the issue is not that each unit = 0.166 of a case.  Although 6*0.166 does not equal a whole number, we can easily get a whole number by using FileMaker’s Round () or Ceiling () functions.  No, this second issue has to do with the generic name of the unit of measure.  To illustrate, let’s say in addition to widgets we also sell whatsits.  Like widgets, we sell whatsits individually (each) but we buy them in a case.  However, for whatists, 8 come in a case.  Now our Unit of Measure table looks like this:


| UofM 1     | Qty 1        | UofM 2     | Qty 2        |
|--------|--------|--------|--------|
| Each         |  1               |   Case	    |      .166      |
| Case         |  1               |   Each	    |       6          |
| Each         |  1               |   Case	    |      .125      |
| Case         |  1               |   Each	    |       8          |


Now, when we go from each to case and case to each, which record are we going to use?  We need a way to distinguish the two sets of each/case.  We could do this a couple of ways:

**Solution 1**
The relationship between item and unit of measure is a many-to-many relationship.  One item can have many units of measure (e.g. we sell it in grams and buy in kilograms).  And one unit of measure can belong to many items.  Many-to-many relationships are connected by a join table.

(Note:  The term “join table” seems to be unique to FileMaker.  If you’re coming from another relational database background, you may know these tables as “associated table” or “junction table.”)

The data model would be:
ITEM ——< ITEMUofM >——UofM

**Solution 2**
If you’re not doing any reporting on the different units of measure - in other words, you don’t need to report on all the items that are sold in units of each, all the items that are sold in units of grams, etc. - then you could put the item ID in a column in the Units of Measure table:


| UofM 1     | Qty 1        | UofM 2     | Qty 2        |  ItemID               |
|--------|--------|--------|--------|
| Each         |  1               |   Case	    |      .166      | 102, 106, 113   |
| Case         |  1               |   Each	    |       6          | 102, 106, 113   |
| Each         |  1               |   Case	    |      .125      | 16, 23                |
| Case         |  1               |   Each	    |       8          | 16, 23                 |


I recommend using the item’s primary key (ItemID here) rather than the item name or SKU as those may change.  (I know, I know, I know - you’re item names and SKUs never change.  I can only say that I’ve seen it enough times now that I only rely on the primary key to be immutable).
