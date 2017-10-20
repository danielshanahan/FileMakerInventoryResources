---
layout: article
collectiontitle: Stock Adjustments
title: Stock Adjustments
description: Eventually, your physical inventory count will differ from your database.  Here is one way to manage that discrepancy.
permalink: /Stock-Adjustments.html
published: true
created: Created 2017-09-07
---

Every inventory application will have a wrong count at some point.  That may seem discouraging to the database developer who spent long hours coding an air-tight system.  It may also seem an affront to the operations manager who put into place the proper procedures to ensure quantity data integrity.  But alas, we live in an imperfect world and physical inventory quantity (i.e. what is actually in the bin) and book quantity (i.e. what is recorded in the database system) may, at times, be off.  In this article, I’ll look at the following:

1. Why are the numbers not the same?
2. How does it get adjusted?
3. Who can make the adjustments?
4. Recording the changes.

## Why are the numbers not the same
There are a number of reasons the physical inventory count is different the computer inventory count.  They may include:

* Shrinkage (liquids evaporate, powders stick to scoopers, wood shrinks, etc.)
* Damage
	* Sent to repair
	* Sent to discount area
	* Returned to vendor
	* Thrown away
* Obsolescence
* Missing
	* Wrong quantity has been received/picked/created
	* Wrong item has been received/picked/created
		* Error
		* Unmarked substitutions
	* Theft

This is not an exhaustive list.  The first thing you’ll need to do is figure out what your list should be.  If you need a description with each item in the list, then this will become a table.  Otherwise, you can store it in a value list.

## How does the quantity get adjusted
The manner you employ for adjusting the quantity will depend, in part, on where the quantity fields are stored.  If the inventory quantity fields are part of the item table, then you may simply update the quantity fields affected by the discrepancy the way you would any other field.  But there is a catch: record locking.  Modifying the item record will lock the record for other users.  While you are updating the On Hand quantity, someone else may be filling out a sales order.  If the item record is locked, the other person won’t have access to the correct On Hand quantity (c.f. “1. ITEM table” in the [Updating Inventory](http://filemakerinventoryresources.com/Updating-Inventory.html) article).

If you store the item quantities in a quantity table then updating the affected fields should follow the same  scripted process.  One way to do this is to create a set of global fields for the user to enter the data.  Once the data is entered, the user presses a button which ensures that the updates will check for record locks and loop until the lock is released.  Since the only way to update the field is via script, any record lock should be brief.

In the images below, you can see an example of a manual adjustment that uses a popover button, global fields, and scripts (Cancel and Continue):

![](http://newleafdata.com/images/FMIR_ManualAdjustment.png)

The script would look similar to the one in the [Updating Inventory](http://filemakerinventoryresources.com/Updating-Inventory.html) article under the section “3. QUANTITY table”:

Set Variable [ $qtyOnHand ; GLOBAL::g_AdjProductOnHand ]
Set Variable [ $exit ; Value: False ]

```
Loop
    Exit Loop If [ $exit = True ]
    Set Field [ QUANTITY::onHand ; QUANTITY::onHand + $qtyOnHand ]
    Set Variable [ $error ; Value: Get ( LastError ) ]

#check for record lock
    If [ $error <> 301 // 301 = "Record is in use by another user"
        Set Variable [ $exit ; Value: True ]
    End If
End Loop 
```

## Who can make the adjustment
You’ll need to have a clear set of rules to determine who can make adjustments to inventory quantities.  Giving access to all employees may or may not be a good idea.  It all depends on your company.  Having people close to the inventory (e.g. pickers) making adjustments may be optimal for your company.  Or, you may want to reserve that process to a shift or operations manager.  The point is, don’t simply create the means of quantity adjustment without giving consideration to who may make those changes.

## Recording the changes
Manual adjusting inventory quantities is a transaction and should be recorded in the transaction table.  Over time, this information may provide valuable insight into process assessment.  For example, if you have a lot (admittedly, a relative term) of adjustments due to obsolescence then you may want to take a look at your purchasing rules.  You may be ordering too many items that will never sell.  Likewise, if you log a lot of damage adjustments, then it can help to alert you about a potential problem with mishandling inventory.  Make sure to review the logs on a regular basis (monthly, quarterly, annually, etc.).

![](http://newleafdata.com/images/FMIR_Transaction_StockAdjustments.png)
