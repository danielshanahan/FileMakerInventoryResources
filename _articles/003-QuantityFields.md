---
layout: article
collectiontitle: Quantity Fields
title: 5 Common Quantity Fields
description: 5 common inventory quantity fields.
permalink: /Quantity-Fields.html
---
Inventory Management Systems (IMS) usually have multiple fields for counting inventory.  The quantities have different meanings and provide a fuller picture of how many items exist than one field can.  Here are 5 common inventory quantity fields:

## On Hand

This is the quantity that you physically have at your location.  They may be in a bin, in a replenishment area, in the receiving staging area, the packing staging area, or the shipping area.

## Allocated

This is the amount of items promised.  They may be promised to a customer via the Sales Order process.  Or it could be promised to an Assembly via a Work Order process.

## Available

Available = On Hand - Allocated.  This is what you can sell or use to build.

## On Order

The quantity of items that have been sent to a vendor via the Purchase Order process but not yet received.

## On Backorder

The quantity customers ordered but your organization was not able to fulfill.

Other fields may include **On Loan**, **In Repair,** **In Transit**.

An item's quantities might look something like this:

![Item Quantity Fields]({{ site.baseurl }}/assets/images/InventoryQties.png)

And here is what it might look like if you have multiple warehouses:

![Item Quantity Fields with Multiple Warehouses]({{ site.baseurl }}/assets/images/InventoryQties2.png)

Keeping track of these different quantities is obviously more involved then just tracking one number.  However, it provides a more complete picture of your inventory quantities.