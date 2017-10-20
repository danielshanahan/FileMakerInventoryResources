---
layout: article
collectiontitle: Transferring Inventory
title: Transferring Inventory
description: Tracking internal inventory movements
permalink: /Transferring-Inventory.html
created: Created 2017-03-11
---
![enter image description here]({{ site.baseurl }}/assets/images/MorgueFile00073412185.jpg)

*Image source: [Morguefil](https://morguefile.com/search/morguefile/6/warehouse/pop)*

Inventory that moves within an organization is a transfer.  “Within” an organization can mean within the same building, within the same campus, or within the same company even though the origin location and destination point are many miles away.  I like to think about transfers in two different types:

1. Internal
2. External

The processes and layouts are different for the two types.  Let’s start with the external transfers.

**External Transfers**<br />
External transfers are movements of inventory that require the Pick, Pack, and Ship process.  To that end, the External Transfer process is very similar to a Sales Order process:

Sales Order → Pick → Pack → Ship → Invoice<br />
Transfer → Pick → Pack → Ship

Once at the destination, the External Transfer is received much like a Purchase Order:<br />
Receipt → Putaway

External transfers have a header/line data model:

![Transfer Data Model]({{ site.baseurl }}/assets/images/Transfer_Data_Model.png)

A detail view may look something like this:<br />
![External Transfer Detail View]({{ site.baseurl }}/assets/images/Transfer_External.png)

**Transfer ID**: the unique record id.

**Date**: date that the transfer record was created.

**Status:** Requested, In Process, Completed, Canceled.  You may have different phases of a status, but having a status is very useful in order to determine where in the process the transfer is.

**Source ID:** where the product currently is.

**Destination ID:** where you would like the product to end up.<br />
Note that the source and destination in this example are given as a warehouse.  A warehouse is a general location for the product and usually sufficient for external transfers.  The exact location of the product is recorded in the Pick process for sending out and the Pack process after receiving.

**Employee ID:** the person who created the warehouse order.  Alternatively, you may wish to have this as the employee who requested the transfer.  Often times in FileMaker solutions, developers will have a set of fields that go into every table.  These are sometimes called “Housekeeping fields.”  Each developer is different, so the housekeeping fields may vary from developer to developer.  However, more often than not, they include fields for who created the record, when the record was created, who modified the record, and when it was modified.

**Why Not Use a Sales Order to Transfer**<br />
Since the External Transfer is very similar to the Sales Order process, you may ask, “Why not use the Sales Order layout to make transfers?”  While the two are similar, I like to separate the process for a couple of reasons:

1. Rules for editing and/or deleting a Transfer may be different than a Sales Order.  If you use the same table and layouts then the scripting process will first need to determine if the record is a Sales Order or a Transfer.
2. Sales Orders should result in an Invoice.  This can provide an internal audit, of sorts.  By checking the Invoices against the Sales Order, you can spot any inconsistencies or errors in the Sales Order process.  If you include Transfers in the Sales Order process, you’ll have to account for that, such as creating a condition in a script that omits certain customers (i.e. Warehouse #4).

**Internal Transfers**<br />
Internal Transfers are different than their external counterparts.  An Internal Transfer is movement within the same complex.  This might be the same building or the same campus.  The product is moved without going through the Pack and Ship processes.  For example, you may have a pick area with several bins of blue widgets.  And you also have an overstock or replenishment area of blue widgets.  This allows you to purchase blue widgets in bulk.  When the bins reach their re-order point, a notification is created and someone transfers blue widgets from the overstock area to the bin.

The overstock area may be in the same building or it may be in another building on campus or close by.  Where ever it is, the distinction is that it does not need to be packed and shipped.  The transfer process can happen with a forklift, hand truck, cart, etc.  And the movement can happen from point to point - that is, from the overstock location directly into the bin without going through the shipping and receiving areas.

Since Internal Transfers are slightly different from External Transfers, you may benefit from a different layout.  There are a couple of ways to approach this:

**Option 1**<br />
![Internal Transfer Detail View - Option 1]({{ site.baseurl }}/assets/images/Transfer_Internal.png)

Highlighting the differences from the External Transfer Detail View above:

1. The Source and Destination are now Locations and not Warehouses.  The location is specific.  It could be a bin or a shelf or an area, but the point is that the location ID takes the someone directly to that product in a given facility.
2. One product per transfer.  Unlike External Transfers, this Internal Transfer does not require child records.  All the information is held in the parent or header record.  The idea is that a Transfer Order is created immediately (and preferably automatically) upon a re-order point, when a picker removes product from a bin.

The Status field can be updated automatically, depending on the Received and Pick Status fields.

Status = Requested when:<br />
Received = NULL and Pick Status = NULL

Status = In Process when:<br />
Received = NULL and Pick Status = Picked

Status = Completed when:<br />
Received = any quantity and Pick Status = Picked

Status can manually be set to Canceled.  Your organization will need to determine the rules for allowing a cancelation (e.g. can it happen after an item was picked or only before?  Does it happen only if overstock cannot fulfill the request? etc.).

**Option 2**<br />
![Internal Transfer Detail View - Option 1]({{ site.baseurl }}/assets/images/Transfer_Internal2.png)

The key differences here are:

1. The Transfer Order does utilize a child table (Transfer Lines) and enables the organization to make multiple transfers.  This may be ideal if you re-stock your picking locations at certain times (noon and 5pm, for example).  It may mean that a Sales Order cannot be fulfilled even though you have plenty of product in the overstock area.  However, it may reduce traffic in the warehouse.
2. The Source and Destination Location IDs are now part of the Transfer Lines instead of the Transfer Header.

**Selecting Internal or External**

Presuming that there are, in fact, two different types of Transfers, the user will need a way to select one of the two.  Here are a couple of ways to do this in FileMaker:

**Selection Method 1 - Custom Menu**<br />
Creating a custom menu allows you to control functions such as Add, Delete, Duplicate, etc. (Custom Menus do MUCH more than this, but it is outside the scope of this article).  With Custom Menus you can assign a script to a process, so you

1. Create a script that shows a custom dialog, asking the user what kind of Transfer s/he wants to create.
2. In Custom Menus (File/Manage/Custom Menus… or Tools/Custom Menus/Manage Custom Menus…) go to the Custom Menus tab, click Create and select Records.
3. Give the newly created Menu Name a unique and identifiable name such as Records - Transfers.
4. Double click Record - Transfers, select New Record (I’ve changed it here to read “New Transfer”) and assign it the script you just created.
5. Click the Custom Menu Sets tab and click the Create… button on the lower left to create a new Custom Menu.
6. Name the new Custom Menu Transfer in the Menu Set Name field.  Click the Add button on the lower left to add menu items.  Again, this is beyond the scope of this article, so make sure to read up on creating Custom Menus in FileMaker.  A good place to start is their [help documentation](https://www.filemaker.com/help/15/fmp/en/index.html#page/FMP_Help/custom-menu-items.html).

![Custom Menu Flow]({{ site.baseurl }}/assets/images/CustomMenu_Transfers_flow.png)><br />
*This is the process flow of forcing a custom dialog when the user creates a new transfer record.*

![Transfer Type Dialog]({{ site.baseurl }}/assets/images/TransferTypeDialog.png)<br />
*This is what the user will see when clicking the New Transfer button (changed from New Record)*

**Selection Method 2 - Button Bar with Popover**<br />
Another method is to add a Popover button to a navigational button bar.  A button bar can be a combination of buttons and popover buttons, which is a powerful feature in FileMaker.  While the navigational button to Items would take the user to the Item lTEM layout, the Transfers popover button would create additional options:

a. Create a new Internal Transfer record.<br />
  b. Create a new External Transfer record.<br />
  c. Go to the Transfer layout.

![Navbar Transfers]({{ site.baseurl }}/assets/images/Navbar_Transfers.png)<br />
*An example of using a button bar with a popover button to create transfers.*
