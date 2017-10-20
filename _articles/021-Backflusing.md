---
layout: article
title: Backflushing
description: Another method of updating inventory quantities.
permalink: /Backflushing.html
created: Created 2017-08-07
---
# Updating Inventory - Backflushing
![Assembly Flow](http://newleafdata.com/images/Backflush_AssemblyFlow.png)

In a previous article I wrote about various methods for [updating inventory quantities](http://filemakerinventoryresources.com/Updating-Inventory.html).  There is an accompanying demo file with six different methods for updating inventory quantities (to receive this and all the demo files, sign up on the Updates and Downloads form on the home page).  The thing these methods have in common is that inventory quantities are managed at every transaction.  That is, every time inventory is received, picked, used, assembled, shipped, etc., it is recorded.

The main benefit of recording at the time of transaction is that the [book inventory](https://www.merriam-webster.com/dictionary/book%20inventory) (that is, the recorded quantity in the database software) is very close to the actual inventory.   However, that is not the only way to track inventory quantities.  Another method, primarily used in manufacturing or kitting, is to update the quantities at the end of a shift or production.  This method is often referred to as “Backflushing”.

As a simple comparison, let’s say we have a chair company and we produce 1,000 chairs in an 8 hour shift.  The chair has four subassemblies:
1. Seat (1)
2. Back (1)
3. Arms (2)
4. Legs (4)

In turn, each subassembly is made up of raw materials.  The seat, for example might be:
1. 18” x 18” 1/2 playwood (1)
2. 18” x 18” cushion material (1)
3. 24” x 24” mesh fabric - Yellow (1)
4. Screws (12)

We need to take the raw materials and send them to the work station.  A work order is generated along with a pick order.  Raw materials are transported from the stock shelves to the work station.

The idea of backflushing is to look at a finished good and update the inventory quantities based on the Bill of Materials (BOM) that were used to make the finished good.  For example, let’s say that our company produces 1000 chairs during an 8-hour shift.  

## Transactions in Real Time
Tracking the stock via real-time might create transaction records for each step in the process.  For example:
1. Pick the raw material.
2. Transport the raw material to the work station.
3. Assemble the raw materials into a subassembly.
4. Transport the subassembly to the next work station.
5. Repeat steps 3 and 4 as necessary.
6. Assemble the subassemblies and any raw materials into the finished good.
7. Transport the finished good to the packing area.
8. Pack the finish good.
9. Transport the packaegs to the shipping staging area.
10. Load the packages onto the shipping vehicle.

## Transactions Backflushed
On the other hand, if we updated the item quantities via backflush we could go through a tree view of the finished good’s bill of materials and deduct the appropriate quantity from the raw materials.  Subassmeblies (a.k.a. Works in Progress, Works in Process, and WIP) would not have any transaction records..

## Panacea of Inventory Tracking?
Like anything else, there are trade offs with backflushing.  It certainly reduces data entry, which would likely improve the time it takes to perform a process (e.g. pick, transport, assemble, etc.).  However, the raw material quantity fields will be wrong until the backflush is performed.  This could be at the end of a production cycle, a shift, a day, etc.  It all depends on your comfort level with having incorrect inventory quantity data.

The other issue with backflushings is that you won’t have a record of where a part is.  It might be in the picker’s basket, on the assembler’s workbench, in a subassembly on another workbench, etc. 

And subassemblies?  You won’t need to track them.  Since you don’t track the quantities of subassemblies, there is no need to keep them as an inventory item in your database.  So, instead of a BOM tree like this:
Chair
	Seat
		18” x 18” 1/2 playwood (1)
		18” x 18” cushion material (1)
		24” x 24” mesh fabric - Yellow (1)
		Screws (12)
	Back
		etc.

You would simply have
Chair
	18” x 18” 1/2 playwood (1)
	18” x 18” cushion material (1)
	24” x 24” mesh fabric - Yellow (1)
	Screws (12)

Is this a problem?  Again, it depends on your particular circumstances.  Do you need to track subassemblies?  Do you need to know where all the raw materials are at any given time?  Is your operation such that someone will know, even if the data isn’t in the database?  There are a lot of things to think about before taking the backlashing route, but it always good to know your options.
