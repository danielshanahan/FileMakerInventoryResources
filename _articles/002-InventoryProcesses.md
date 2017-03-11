---
layout: article
collectiontitle: Inventory Processes
title: Inventory Process
description: A succinct list of the processes within an inventory management system.
permalink: /Inventory-Process.html
---
For those new to the inventory process it may be a bit overwhelming.  The goal in this article is to list the processes that go along with many inventory systems.

![Inventory Process]({{ site.baseurl }}/assets/images/Inventory_Process.png)

<p class="citation">Icons from <a href="https://icons8.com">www.Icons8.com</a></p>

Though largely self-explanatory, the processes have the following purposes:

**Purchasing**: buying items from a vendor that you will either sell or assemble and sell.

**Receiving**: items sent to you by the vendor that you purchased.  This process enables your database to keep track of the item quantity at the warehouse level.

**Putaway**: putting items into their pre-designated warehouse location.  If your individual locations have barcodes, the putaway process is where the stock keeping unit (SKU) is assigned a barcode.  This process enables your database to keep track of the item quantity at the location level.

**Selling**: selling the items you’ve purchased from a vendor or that you have manufactured.  If you sell more items than you have available than the sales order may have a backorder quantity.  You may choose not to have backorders or your customer may choose not to accept backorders.

It is also possible to sell items you do not physically have.  Depending on your customer, your vendor, the item, and the generally acceptable wait time, you could purchase items *only after* customers place their order.  If assembly is required, you’ll need to receive and assemble the item before packing and shipping it.  However, if no assembly is required you may be able to have your vendor ship the item directly to the customer.  This is known as a *Drop Ship*.

**Picking**: picking items is the opposite of putaway.  Like putaway, this process enables your database to keep track of the item quantity at the location level.

**Packing**: packing tracks the item into its shipping container e.g. envelops, boxes, pallets, etc.).  Tracking packing data in your database is most beneficial when a single sales order requires multiple packing containers (e.g. box 1 of 2 and box 2 of 2).

**Shipping**: shipping is the process of sending your item to your customer.  This may involve a professional service like FedEx or UPS.  This process will likely produce a tracking number which can be used to loosely monitor the location of your shipment while in transit.
The process list is not exhaustive.  For example, if your organization has multiple warehouse locations you may want to include a **Transfer** process.  Similarly, if you store items in bulk as well as a bin for picking, you may want to include a **Replenishment** process.

**Invoicing**, **Accounts Receivable (A/R)** and **Accounts Payable (A/P)** are also important inventory processes, though each organization will need to determine whether to include them in the inventory database or create a link with their accounting software.

## Data Model

Many of the processes have multiple tables to store data.  For example, the purchasing process has a purchase order (PO) header table and purchase order lines table.  The header contains information that pertains to this entire purchase such as the PO number, the vendor’s information, the date of the order, the person who placed the order, etc.  The line table contains information on items being ordered such as the item number, item description, quantity ordered, unit of measure, etc.

A simple entity relationship diagram (ERD) for the purchasing process can look like this:

![enter image description here]({{ site.baseurl }}/assets/images/PurchaseERD.png)

## Data Model Continued - Header and Lines

Each of the 8 processes can be modeled as a header table and a lines table:

![Inventory Process Tables]({{ site.baseurl }}/assets/images/processTables.png)

Having the proper data structure can greatly enhance your inventory database in the FileMaker platform.
