---
layout: article
collectiontitle: Kitting
title: Kitting
description: Kitting is the process of taking finished goods and bundling them together to make a new product offering.
permalink: /Kitting.html
---
Kitting is the process of taking finished goods and bundling them together to make a new product offering.  Gift baskets and tool kits are an example of kitting.  Each of the items can be sold individually.

![Tool Kit]({{ site.baseurl }}/assets/images/toolKit.jpg)

*Photo from [http://morguefile.com/p/131402](http://morguefile.com/p/131402)*

## Kitting requires a Bill of Materials (BOM)

Just like a manufactured product, the kit will require a BOM, listing the component items, the quantity required for the kit, and sometimes the unit of measure of each item.

## Kitting is an assembly process

You can think of kitting as a manufacturing process, which is to say that kitting involves assembly.  Component items that have an expiration date means that the kit will also have an expiration date.  A work order is the usual means for tracking the assembly process. 

## Displaying the Kit items to the buyer

The kit appears as a single line item on the sales order.  The picking, packing, and assembly (where work orders are used) list the kit components and provide traceability.  It may be beneficial to include this list when the product is shipped to the customer so that s/he knows what is included in the kit.

## Component Item Locations

Since a kit is an assembled product, the items composing the kit would come from the same warehouse location.  If one or more items are missing they may need to be transferred from one warehouse to another or ordered from the supplier if none exist in stock.



## Partial Shipments

If a kit cannot be assembled but you still want to ship the available component items to your customer, it may be best to list those items on the sales order and invoice.  This will enable the database to keep track of which items have shipped and which are on backorder.

Technically, they are not receiving a kit but the component parts.  Since a kit cannot be partially shipped (only its available components can be partially shipped) it may be beneficial to have a script that gathers the available quantities of component items and provide a **can sell** and **can make** number:

**Can Make Quantity**: the number of kits that can be assembled based on the available component parts.

**Can Sell Quantity**: Can Make Quantity + the number of available kits *already assembled*.