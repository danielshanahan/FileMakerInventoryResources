---
layout: article
collectiontitle: 7 Potential Sales Order Statuses
title: 7 Potential Sales Order Statuses
description: The status field allows you to track the process of a sales order.  But what should those statuses be?  Here are 7 suggestions.
permalink: /7-Potential-Sales-Order-Statuses.html
published: true
created: Created 2017-09-17
---

Having a field for the sales order status is important as it gives users the ability to get an overview of all the sales orders (e.g. a chart or aggregate number on a dashboard) as well as query the database to select only sales orders of a certain status.  There are a lot of moving parts in a sales order, which means the statues can be complex.  These are just a few examples.  To optimize your system, you may want to check with the various teams involved (sales, fulfillment, management, etc.) to chart and label the various statuses in the sales order process.

1. New
2. Que to Pick
3. Picking
4. Que to Pack
5. Packing
6. Que to Ship
7. Partially Completed
8. Completed
9. On Hold
10. Cancelled

### New
A new sales order that has not yet been processed.  Data is still being entered into the header or line items.  Changes can be made to the order while the status is new without a change order record.

### Que to Pick
Ready for the next available picker but not yet in the process of being picked.

### Picking
The order is in the process of being picked.

### Que to Packing
Ready for the next available packer but not yet in the process of being packed.

### Que to Ship
Ready to be loaded onto the shipping vehicle (truck, trailer, scooter, etc.).

### Partially Completed
A portion of the customer’s order has been shipped.  The whole order was not processed due to stockout. 

### Completed
The whole order was processed and shipped to the customer.

### On Hold
Something came up and either you or the customer is not ready to proceed with the order nor are either of you ready to cancel the order.

### Canceled
The order was canceled.

## Purpose of the statuses
The status enables users to track the progress of an order at any given time.  But you may not need all of these progress points.  Or, you may need more.  It depends on a number of things such as the size of your location, your current business process, and current issues.  For example, if you have an issue with the length of time it takes to pick an order, you may want to be able to track where that order is in the process at all times.  Similarly, if orders progress well and tracking any given order is not cumbersome, you may want to replace these:

Que to Pick
Picking
Que to Pack
Packing
Que to Ship

with

In Progress.

## Multiple Status at Once
Each status represents a state in the process.  It may be possible for a sales order to be in two states at the same time.  Take, for example, a sales order that can only be partially fulfilled and the customer will accept a back order.    The sales order goes through the process and the order is partially filled.  The sales order has the status of “Partially Completed”.  Then, a new shipment arrives and the order can be completed.  The sales order will go through the process again.  “Partially Completed” is still an accurate status but so is “Que to Pick”.  Likewise, “Picking”, “Que to Pack”, “Packing”, etc. will also be accurate during that time, as will “Partially Completed”.   Which status should the sales order have?   Here are two options:

1. Over-ride any past status with the status that most accurately reflects the current process.  In other words, once the sales order goes through the pick-pack-ship cycle again, use those statuses (“Que to Pick”, “Picking”, etc.).

2. Close each sales order at the end of a process and open a new one, if necessary.  In our example from above, where the order could only be partially fulfilled, you would close the original sales order (leaving the status as “Partially Completed”) and open a new sales order with the unfulfilled items/quantities.  It would be helpful to relate the second sales order to the first sales order, so having a parent-child relationship would be required in the data structure.  This would enable a user to navigate through the life cycle of a sales request by a customer.

Of course, these are not the only two options.  Discuss with your team and find the best method for your business.

## Final thoughts
The point is to consider the status demarkations you’ll need before you build the sales order module.  You can always change them later and going through this exercise can help you clarify and refine your current processes.
