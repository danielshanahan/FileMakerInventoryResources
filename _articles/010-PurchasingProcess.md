---
layout: article
collectiontitle: The Purchasing Process
title: The Purchasing Process
description: 8 Scenarios you may experience with Purchase Orders
permalink: /The-Purchasing-Process.html
created: Created 2017-03-11
---
The Purchasing Process is the first part of the Inventory Process.  It involves purchasing, receiving, and putaway.  (Watch my [YouTube video](https://youtu.be/7w20yKzoOiE) for a walk through the process with a FileMaker demo file.  The demo file is available in the File section of FileMaker Inventory Resources.)

## 8 Scenarios of Purchasing


1. 10 line items ordered. 10 line items received complete.
2. 10 line items ordered. 9 line items received complete. 1 line item not received, vendor does not do backorders.
3. 10 line items ordered. 10 line items received, but 1 line item only received partial quantity. Vendor does not do back orders.
4. 10 line items order. 9 line items received complete. 1 line item not received, vendor backorders. Need ability to receive this line item at later date.
5. 10 line items ordered. 10 line items received, but 1 line item only received partial quantity. Vendor back orders remaining quantity. Need ability to receive remaining quantity at later date.
6. 10 line items ordered (i.e. PO sent to vendor). Modification made to one or more of the line items. 10 line items received complete including the line item(s) from the modification.
7. 10 line items ordered (i.e. PO sent to vendor). Additional item ordered to the same PO number (over the phone, via email, etc.). 10 line items received complete plus additional line items from change.
8. 10 line items ordered (i.e. PO sent to vendor). Deleted line item conveyed to vendor. All line items received complete reflecting the deleted line item from verbal conversation.


To handle these different scenarios we need the following tables: <br />
1. Purchase Order <br />
2. Purchase Order Line <br />
3. Receipt <br />
4. Receipt Line <br />
5. Item <br />
6. Change Orders

And here is how those tables relate to one another:

![enter image description here]({{ site.baseurl }}/assets/images/PO_ERD.png)

We also need a status field on the Purchase table and one on the Purchase Line table. The status fields may vary for you according to your business flow.  I start with these and change them as the project requires:

##### Purchase Order Status

* New
* Sent
* Partially Received
* Completed
* Cancelled

I don’t use “Open” as a purchase order (PO) status because I think its meaning can be unclear.  Does “Open” mean it has been created but not sent to the vendor?  Does it mean that it has been sent to the vendor but not fulfilled?  Does it mean that it has been fulfilled but your organization allows you to create multiple orders with that one PO?

If “Open” works for your organization, then by all means, use it.  Now, let’s move to the status fields of the Purchase Order Line table:

##### Purchase Order Line Status

* NULL
* Complete
* Pending
* Partial

Here is what they mean: <br />
NULL = empty field when PO status is New or Sent. <br />
Complete = all line items were received. <br />
Pending = zero items were received. <br />
Partial = a line item was only partially received.

With these in place, we can easily handle the 8 scenarios above.  Let’s take a look at mockups for each of these scenarios.  For simplicities sake, I’ll only show 4 items ordered instead of 10.

## Scenario One: 4 items ordered, 4 items received.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario1.png)

The first mockup window shows a purchase order (PO) that has been created.  It is still within the organization and hasn’t been sent to the vendor yet.  This is important because changes can be made to the PO but do not require a change order record (as seen in the Changes tab in the mockup.  We’ll get to those further down this article).

You may have a PO that is created and left in the “New” status for an entire day.  Anytime anyone needs an item from that vendor, they can just go that PO.  The length of time can be longer, of course.  The same thing can be said if your company creates a PO and hangs onto it for a week.  Of course, you could also create multiple POs to the same vendor throughout the day, sending them as soon as they are made.  In any case, the first two mockup windows show the PO with a status of New and Sent respectively.

The third mockup window shows the Receipt layout when an order has been received.  In this scenario, everything ordered has been received.  The fourth mockup window returns to the PO layout, showing the status of Complete on both the PO Lines as well as the PO, since everything ordered has been received.  The Receipts tab on the PO layout is shown in the fifth and final window, showing all the receipt records that were needed to fulfill the order.



## Scenario Two: 4 items ordered, 3 items received, no backorders.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario2.png)<br />
If your vendor cannot fulfill an order, you may not want to keep it on backorder (meaning the vendor will send it when they receive or make it).  Or, it could be that a vendor doesn’t track backorders so they require their customers to re-order at a later date.  To move things along, we’ll start with the Receipt layout.

In the first mockup window, the Red Widget did not come in with the rest of the goods from the vendor.  And since there are no backorders, we are not expecting any more shipments from the vendor regarding this PO.  Therefore, the PO is complete, even though everything ordered wasn’t received.  The second mockup window shows the status for both the PO Line and the PO and like scenario 1 above, everything is complete.  The third mockup window, like the first scenario, shows the related Receipt records.



## Scenario Three: 4 items ordered, 4 items received but one only partially received, no backorders.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario3.png)

This is like the previous scenarios except our vendor has sent some Red Widgets, but not all.  Since there are no backorders, anything received is going to complete the PO, which is what we see in both the PO line and PO statuses.



## Scenario Four: 4 items ordered, 3 items received and one backordered.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario4a.png)<br />
The shipment once again arrives without the Red Widgets but this time the quantity not received is marked in the Backorder (B.O.) column.  The PO status has been changed to “Partially Received” and the PO line status for the Red Widgets is “Pending”.  For now, the Receipts tab on the PO layout looks like all the other Receipt tabs we’ve seen so far.  But since backorders are accepted, we’re expecting another shipment, which is what we see in the next set of mockups.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario4b.png)<br />
A new shipment has come in and notice that we have a new Receipt record (REC10018).  The only line item on the receipt is the Red Widget.

**Note: One thing I don’t show in the mockup is how the Receipt Line Items are populated.  One way to do this in FileMaker is to create a filtered popup menu that displays POs that are either Sent or Partially received.  A script trigger on the popup field (OnObjectModify) can pull in the PO Lines that are not Complete**

Once the Red Widget is received then its PO Line is updated to “Complete”, which makes the PO complete.  Looking at the Receipt tab now shows that two Receipt records were needed to fulfill this PO.



## Scenario Five: 4 items ordered, 4 items received but one only partially received and backordered.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario5a.png)<br />
Similar to the previous scenario except that the Red Widget was partially received on the first shipment, as shown in Receipt record REC10012.  The PO Line is marked as “Partial” and the remaining quantity is received in a different Receipt record, REC10018.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario5b.png)

In the first scenario I wrote how a PO can be changed multiple times before it is sent to the vendor.  You may have a reason to keep track of those changes, but I’ve found most companies don’t.  However, once the PO is sent to the vendor, changes must be tracked.  You’ll need to know who changed the order, what was the change, and who approved the change.  The next three scenarios deal with change orders which can occur in one of three ways:

1. Modification (increase or decrease) to an existing PO Line item.
2. Addition of a PO Line item.
3. Deletion of a PO Line item.

## Scenario Six: 4 items ordered and 1 of those items was modified.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario6.png)<br />
In this example, the Yellow Widget was initially ordered at a quantity of 200.  At some point - after the PO was sent to the vendor and before it was received - it was decided to order an additional 300 for a total of 500 Yellow Widgets.  The first two mockup windows show the Yellow Widget highlighted with the different quantities.  The third mockup window shows the Changes tab where a record is created to record the change from 200 Yellow Widget to 500.



## Scenario Seven: 4 items ordered and an additional item ordered.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario7.png)<br />
After the PO was sent but before it was received, the customer added an additional item to the PO.  In this case it is 500 units of Purple Widgets which are marked on the Changes tab.



## Scenario Eight: 4 items ordered and one of them deleted.

![enter image description here]({{ site.baseurl }}/assets/images/PurchasingProcess_Scenario8.png)<br />
The final scenario removes one of the line items from the PO after it has been sent to the vendor but before it has been received.  The Yellow Widget in the first mockup window is no longer among the PO Line items in the second mockup window.  This is recorded in the Changes.

## Conclusion

Though a bit lengthy and somewhat redundant, hopefully this article explains the purchasing process and PO tables, Receipt tables, and a Change Order table can work together to track the various ways an item may be ordered and received.
