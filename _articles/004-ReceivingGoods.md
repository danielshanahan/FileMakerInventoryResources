---
layout: article
collectiontitle: Receiving Goods
title: Receiving Goods
description: 5 ways in which your organization might receive goods.
permalink: /Receiving-Goods.html
created: Created 2017-03-11
---
There are multiple scenarios in which your organization might receive goods.  Here are five:

1. Receiving against a purchase order.
2. Receiving a returned item.
3. Receiving a loaned item.
4. Receiving an unordered item.
5. Transfers.

### Receiving against a purchase order

The most common receipt record is against a purchase order.  Ordered items arrive from a vendor and are put into stock or immediately used to fill backorders.  It may happen that a vendor's shipment does not completely fulfill the purchase order.  For that reason, a purchase order may have many receipts.

And since a purchase order line may be received in multiple shipments from the vendor, the purchase order line may have multiple receipt lines.  The data model for this looks like this:

![PO/Receipt ERD]({{ site.baseurl }}/assets/images/polineReceiptline.png)

### FileMaker's Relationship Graph

This, of course, is a cyclical relationship which cannot be replicated in FileMaker's relationship graph (RG).  That is why the RG is not an entity relationship diagram.  To get the same functionality out of this relationship we'll need to duplicate one of the table occurrences.  Here is one way of expressing this in the RG:

![FMP PO/Receipt Relationship Graph]({{ site.baseurl }}/assets/images/fmp_poLineReceiptLine.png)

A layout might have a portal for both the purchase order lines as well as the receipts:

![FMP PO/Receipt Layout Mockup]({{ site.baseurl }}/assets/images/poMockup.png)

![FMP PO/Receipt Layout Mockup]({{ site.baseurl }}/assets/images/poMockup2.png)

### Receiving a returned item

Items may be returned for several reasons:

1. Wrong item was sent.
2. Wrong item was ordered.
3. Item was damaged upon receipt.
4. Item was damaged within a warranty period.

It may be helpful to mark the receipt line record with the reason for the return.  That way, you can create a report on the data.  For example, how often is the wrong item sent?  How much money does it cost your organization to send the wrong item?  How much would it cost to improve upon the wrong item sent?  These type of questions can only be answered with data.

Once an item is returned, what do you do with it?  Your options may include:

1. Return the item to the vendor for a full refund.
2. Return the item to the vendor for a partial refund.
3. Return the item back into your stock and sell at full price.
4. Return the item back into your stock and sell at a discounted price.
5. Alter the item (cut it, use it to create a new item, etc.) and determine a price.
6. Scrap the item.

Again, tracking this on the receipt line can provide data for future reports.

### Receiving a loaned item

Loaned items are, by definition, expected back to your organization.  Receiving those items is similar to receiving against a purchase order.  However, loaned items would have originated in a sales order or loan order.

Like items received against a purchase order, a loaned item may be returned incrementally.  That creates a data model like so:

![SO/Receipt ERD]({{ site.baseurl }}/assets/images/solineReceiptline.png)

### Receiving an Unordered Item

There are a number of ways to receive an unordered item:

1. A vendor sends more than the quantity ordered.  You may choose to send the unordered quantity back to the vendor or you may decide to accept the overage.  A lot will depend on your business, the item in question, how fast/slow it moves, the vendor, shipping methods, and possible a few more factors.
2. A vendor sends sample units to distribute to your customer.  Vendors may be incentivized to send unrequested items to their customers (i.e. you) in order to introduce new product or even re-ignite interest in underperforming product.

Unordered items occupy a gray area. Since these items were not ordered from the vendor there is no purchase order associated with it.  You may choose not to enter those quantities in the system at all.  Or, you may enter them, knowing that it will cause a discrepancy between what was ordered and what was received (which can happen when a vendor sends fewer than the ordered quantity.

### Transfers

Transfers are another gray area for receiving.  Let's say a warehouse has a bin location from which all the pickers draw that item.  In another part of the same warehouse there is shelving to stack pallets of overstock.  In that case, an internal transfer may be sufficient.

In another example, your organization has several warehouses and those buildings are not on the same campus.  Transferring items may involve going across town, across the state, the country or the world.  In this instance, a transfer would benefit from going through the shipping and receiving process.  I discuss transfers in more detail in [this article](/Transferring-Inventory.html).

### FileMaker Pro Layout

There are many ways you can design a layout in FileMaker that enables a user to receive from various scenarios.  A demo file is available in the "Files" section on the Home page of FileMaker Inventory Resources.  The method I use in that file is to create multiple objects of the same field.  Now you may have multiple fields for the various foreign keys in the RECEIPT table.  For example, you may have purchaseOrderID, salesOrderID, loanOrderID, and transferOrderID.  In the demo, I simply use the orderID field to store the foreign key for all of these related table.  The technique is the same.

The key is to show the appropriate field based on the type of receipt.  I've added a orderType field with a drop down list.  Once the user select the field the appropriate Order Number field displays.  Now in my case, as I mentioned, this is the same field but each field has a different popup menu, or in the case of Sales Order, just an editable field.

![Loan Receipt]({{ site.baseurl }}/assets/images/fmp_receipt_loan.png) *Order Type = Loan Order only displays available loan order numbers.*

![Sales Order Receipt]({{ site.baseurl }}/assets/images/fmp_receipt_so.png) *Order Type = Sales Order is an editable field.*

![Transfer Receipt]({{ site.baseurl }}/assets/images/fmp_receipt_transfer.png) *Order Type = Transfer Order only displays available transfer order numbers.*

The sales order is used to capture returns.  You could just as easily use an invoice instead.  The reason it has an editable field instead of a popup menu is that presumably your organization would have a lot of completed sales.  A popup could be way too long to be useful.  Since it is an editable box you'll likely want to have some checks to make sure the sales order exists.

The method I use to hide these field in the demo is by using a Slide Control.  Each Slide Control has an object name.  The Order Type popup menu has a trigger so when a certain order is selected the appropriate Slide Control Object appears.  Removing the Slide Control's navigation dots ensures that users don't inadvertinly select the wrong Order Num field.  Finally, in the Enlightened theme there is a Minimal Slide Control style.  This essentially makes the object invisible to the user.

![Minimal Slide Control style]({{ site.baseurl }}/assets/images/fmp_receipt_style.png)
*Making the Slide Control "Invisible"*

![Slide Control in Layout ModeReceipt]({{ site.baseurl }}/assets/images/fmp_receipt_layout.png)
*Slide Control in Layout Mode using the Default style in order to show the object more clearly.*

You can download a demo file of this technique [here]({{ site.baseurl }}/Download_poProcess_v2.html).  To see more downloadable resources, go to the File section on the home page.
