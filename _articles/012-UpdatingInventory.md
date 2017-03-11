---
layout: article
collectiontitle: Updating Inventory Quantities
title: Updating Inventory Quantities
description: Methods for updating inventory quantities
permalink: /Updating-Inventory.html
---
To state the obvious: inventory is dynamic.  Products are constantly moving into, throughout, and out of your business.  If you are a manufacturer or you provide kits, then products are also getting bundled with other products.

The movement itself is tracked in a <a href="http://filemakerinventoryresources.com/Transactions.html">Transaction</a> table.  When that happens there needs to be a running count of inventory.  For example, lets say you have the following quantities for a Red Widget:

On Hand: 100<br />
Allocated: 0<br />
Available: 100<br />
On Order: 500<br />
On Backorder: 0

(For a refresher on common inventory quantity fields, see the article [Quantity Fields](/Quantity-Fields.html)).<br />
And then an action occurs - someone sells Red Widgets!  This action requires a modification in our quantities.  We need to ensure that the quantity promised to our customer is available (there are a lot of presumptions in that statement.  Lets make it simple and presume that we have enough to fulfill the order of our customer).

Lets break this down even further.  Think about the process of selling Red Widgets (or any good).  Generally speaking, it goes something like this:

1. The customer places an order (on a web channel or by contacting our company).
2. The order is placed in the inventory management database system.
3. A picker is notified that an order has occurred and that s/he needs to pick the items.
4. The picker goes through the warehouse to get the items.
5. The items are sent to a packing area and packed.
6. The items are sent to a shipping area and shipped.

The act of placing an order and having it picked, in our example, takes four steps.  If we dont mark the ordered quantity there is a chance that a salesperson will re-sell the promised item.  To help prevent this, the quantity fields can be updated throughout the various steps of the process.  Again, in our example, as soon as the customer places the order, it would be ideal to have a process (e.g. a FileMaker script) change the quantity levels of the Red Widgets:

On Hand: 100<br />
Allocated: 30<br />
Available: 70<br />
On Order: 500<br />
On Backorder: 0

When the salesperson takes the next sales call, she knows that she no longer has 100 Red Widgets at her disposal, because her previous customer wants 30 and the order of 500 has not yet arrived.  Therefore, she knows that she can only fulfill an order of 70 or less.

When updating quantities there are three things to address:

1. What quantity fields are triggered by a particular process?
2. Where are those quantity fields location?  In other words, the quantity fields are an attribute of what table?
3. How can the data be updated?

### What Quantity Fields are Triggered?

Not all processes affect all the quantity fields.  In our example above, creating a sales order affects Allocated and Available but it doesnt change On Hand, On Order, or On Backorder.  Here, then, is a general list of processes and the affected quantity fields:

**Receiving**<br />
On Hand +<br />
If receipt is against a purchase order: On Order -<br />
If receipt is against a loan: On Loan -<br />
A receipt can also be against a sales order (a.k.a. a returned good).  The quantity fields affected will depend on what happens to that good (restocked, returned to vendor, discarded, etc.).

**Putaway**<br />
Available +

**Assembling** (for manufactured items or bundled items as kits)<br />
Allocated +<br />
Available -

**Selling**<br />
Allocated +<br />
Available -

**Picking**<br />
No changes.

**Packing**<br />
No changes

**Shipping**<br />
On Hand -<br />
Allocated -

**Transferring**<br />
Transferring items can affect different quantity fields, depending on the type of transfer.  For more information, see the article in [Transferring Inventory](/Transferring-Inventory.html). 

Before doing any scripting in FileMaker, run through your particular processes and make adjustments as necessary.

### Where to Locate Quantity Fields?

Quantity fields are an attribute but of which table?  You have options!

1. The ITEM table.
2. The TRANSACTION table.
3. A QUANTITY table.

**1. ITEM table**<br />
The most obvious place to locate the quantity field attributes is in the ITEM table.  It is, after all, an attribute of that table.  How many units of that particular item are on hand, allocated, etc.  The issue with keeping the quantity fields in the ITEM table is the chance of running into a record lock.

Record locking is a database functionality that aims to prevent multiple users simultaneously editing the same record.  Without record locking, databases run the risk of storing unintended results.

In FileMaker, a record is locked when a user edits a field.  On the item record, if someone is changing a price, updating a description, making a note, etc., that could potentially lock the record.  Now imagine a scenario where someone is changing prices on several items.  For some reason, the person is drawn away from the database (the phone rang, a meeting occurred, it was lunch time, etc.) while s/he is in the middle of editing a record.  That records is now locked.

Lets stay with this scenario and imagine that another user is on a sales call and one of the requested items is the one the other user left while changing the price.  The user creates a sales order, which will affect the Allocated (+) and Available (-) quantity fields.  However, since the item is locked, those fields are not updated.

There are ways to check for a locked record and youll have to have some business rule in place to handle it (e.g. make a note of the quantities and go back to manually update the record, find the computer/device that is locking the record and commit the record, force the user out of the record [function of FileMaker Server], ask the customer to call back, etc.).

**2. TRANSACTION table**<br />
Another option is to place the quantity fields in the TRANSACTION table.  Transactions record the movement of items and are therefore tied to the items quantities.  The advantage is that each transaction is a new record and highly unlikely to incur record locking.  The disadvantage is that you will need to create a process for retrieving the item quantities from the last transaction record.

**3. QUANTITY table**<br />
A third option is to have a QUANTITY table for the quantity fields.  The table is a one-to-one relationship with the ITEM table.  This is sometimes called a Skinny, Narrow, or Thin table.  That simple means that some attributes are placed in another table with a one-to-one relationship.  The advantage here is that the previous scenario of a record lock will not affect the related record in the QUANTITY table.  The user above can have the item record locked and other users will still be able to modify the quantity fields.

And unlike the TRANSACTION table approach, there is no need to search for the last record to find the latest quantities.

One requirement of this approach is to only modify the quantity fields via a script.  If you allow users to manually edit the quantity fields, then you run into the issue of record locking again.  By updating the field via script there is still the possibility that a record could be locked but that lock would only happen with a script.  And presuming the script is not paused, the time it takes a script to update the quantity fields is very quick.

If a record is locked under these conditions you could loop through the process until the record is free.  The following script excerpt is one way to do this:

````
Set Variable [ $qtyOrdered ; SALESLINEITEM::qtyOrdered ]
Set Variable [ $exit ; Value: False ]

Loop
    Exit Loop If [ $exit = True ]
    Set Field [ QUANTITY::allocated ; QUANTITY::allocated + $qtyOrdered ]
    Set Variable [ $error ; Value: Get ( LastError ) ]

    #check for record lock
    If [ $error &lt;&gt; 301 // 301 = "Record is in use by another user"
        Set Variable [ $exit ; Value: True ]
    End If
End Loop        
````

This example is a fairly simple one in that we are only talking about item quantities.  However, if you have multiple locations, youll have location quantities.  If you have lots/batches, youll have those quantities.  And of course, you may have multiple lots located over multiple locations.  The three placement strategies listed above (Item, Transaction, Quantity) will still work.  For the Item strategy, you would have quantities on the LOT table and the LOCATION tables: <br />
LOT::onHand<br />
LOT::allocated<br />
LOT::available<br />
LOT::onOrder<br />
LOT::onBackorder<br />
LOCATION::onHand<br />
LOCATION::allocated<br />
LOCATION::available<br />
LOCATION::onOrder<br />
LOCATION::onBackorder.

These are all unique fields and need to be updated along with the item quantity fields ( e.g. ITEM::onHand, etc.).

In the Transaction strategy, you would have additional fields to clarify the quantities:<br />
TRANSACTION::itemOnHand<br />
TRANSACTION::lotOnHand<br />
TRANSACTION::locationOnHand<br />
etc.

These would connect to the item, lot, and location via foreign keys:<br />
TRANSACTION::itemID<br />
TRANSACTION::lotID<br />
TRANSACTION::locationID

The Quantity strategy can have a single instance of the quantity fields:<br />
QUANTITY::onHand<br />
QUANTITY::allocated<br />
QUANTITY::available<br />
QUANTITY::onOrder<br />
QUANTITY::onBackorder

Along with the foreign keys:<br />
QUANTITY::itemID<br />
QUANTITY::lotID<br />
QUANTITY::locationID

A key component of the Quantity strategy is that there will be records that total all the quantity fields when there are multiple lots and/or multiple locations.  For example, ITM-345 has 8 lots spread over 3 locations.  The QUANTITY table for ITM-345 might look like the following mockup:

![enter image description here]({{ site.baseurl }}/assets/images/QuantityTable.png)



### How to Update

Once weve decided where to locate the quantity fields, well need to have a strategy for updating them.  Here are two possibilities:

1. Traverse a Portal.
2. Find in Table.

As you can imagine, each has its own advantages.

**Traversing a Portal**<br />
As its name implies, this method isolates related data via a portal and loops through those records, making the updates.  The advantage is that it is not necessary to navigate away from the current record.  In FileMaker, context is very important.  Remaining on the same record reduces layout and window management.

This method can be implemented in a couple of ways.  The first way is to have a portal on every layout where the quantity fields will be updated (see above).  The second way is to have the portals only on the TRANSACTION table.  Since quantity changes all require a transaction record, every change process will include the TRANSACTION table.

**Find in Table**<br />
The second method involves leaving the current record and navigating to other tables in order to update the quantity fields.  The advantage of this method is that it requires fewer table occurrences.

Ive not done any benchmarking on either process but I suspect the difference in speed between the two would not be enough to promote one over the other.

A .zip file is available for download that illustrates the various updating strategies mentioned in this article.  For more information, go to [File: Updating Inventory Quantities](/Download_UpdatingQuantities.html).