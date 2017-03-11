---
layout: article
collectiontitle: Transactions
title: Transactions
description: Creating a life cycle for your inventory
permalink: /Transactions.html
---
Inventory goods that move in and out of an organization are often tracked in a transaction log table.  The movements are a kind of story.  The table records shows the lifecycle of inventory from the time it arrives until the time it leaves the organization.  Not all inventory is logged in a transaction table.  Service items and drop ship items are not logged.  Only goods that are received and shipped are logged.

## More than Movement

Transactions are certainly helpful to see the movement of items.  However, they can track more than movements.  Neither Purchase Orders nor Sales Orders involve inventory movement.  Yet, both can be added to a Transaction log.  That way, reading the log shows why a certain item and quantity were received as well as why a certain item and quantity was shipped.

The next two images show just the movements.  The top image shows all the activity and the bottom image filters on one item (20-0024).

![enter image description here]({{ site.baseurl }}/assets/images/Transaction_Movement_Only.png)

Now we’ve added Purchase Order and Sales Order information.  This can greatly help users determine the correct number of items were received/shipped.

![enter image description here]({{ site.baseurl }}/assets/images/Transaction_All.png)

## Tracking Item History

We can expand the Transaction information by grabbing the item quantities at the time of data entry.  This can provide a broader picture of the item.  In the image below, the filtered list shows transactions for item 20-0024.  This time, the item quantities are included.

The first line shows that a quantity of 100 was created on a purchase order.  At the time of receipt the On Hand increased by 100 and the On Order decreased by 100.  They were still at the dock and not available to be picked yet, so the Available count remained the same.  Availability only increased at the time of Putaway.

Then a Sales Order was created for a quantity of 3 and the Allocated increased and Available decreased by that number.  Picking in this example didn’t change any of the quantities though it certainly could.  Which brings up a great point - the way Transaction data is organized largely depends on the needs and wants of the organization.

To complete our history of 20-0024, the Shipped line shows that On Hand and Allocated were both reduced by 3.

![enter image description here]({{ site.baseurl }}/assets/images/Transaction_ItemHistory.png)

Adding the item quantities (On Hand, Allocated, Available, and On Order) provides a snapshot of the inventory quantities at the time of the Transaction. 

## Tracking Location History

We can expand our snapshot by including location quantities.  Let suppose the following:

1. The organization has multiple warehouses: A, B, and C.
2. In each warehouse, one item may be stored in multiple bins.
3. In warehouse A, item 20-0024 is stored in bin 003, 004, and 005.
4. Warehouse A doesn’t have any quantity of item 20-0024 in bins 003, 004, 005.

Our table is getting pretty wide so I’ve abbreviated the quantity fields:

* OH = On Hand
* AL = Allocation
* AV = Available
* OO = On Order<br />(for a review of commonly used quantity fields, see [6 Common Quantity Fields](/Quantity-Fields.html))

We now have two sets of quantities for each row.  One for that item across all locations (all warehouses, all bins, etc.) and another for a particular location (warehouse and sometimes bin).  Thus, the Purchase Order row shows that a quantity of 100 have been ordered and the organization has 12 On Hand and 12 Available.  We also see that Warehouse A has 0 On Hand and 0 Available, so those quantities must exist in either Warehouse B, Warehouse C, or a combination of the two.

The location quantities aggregate from row to row in the same manner as written above.

![enter image description here]({{ site.baseurl }}/assets/images/Transaction_LocationHistory.png)

The Transaction table now provides the organization with a comprehensive view of how its inventory moves in and out of its locations.
