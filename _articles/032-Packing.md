---
layout: article
collectiontitle: Packing
title: Packing
description: The Packing portion of the Pick-Pack-Ship process.
permalink: /Packing.html
published: true
created: Created 2017-12-22
---
![](Packing/newleafdata.com/images/FMIR_file0001118206238.jpg)
*Image by npclark2k, [Morguefile free photographs for commercial use](https://morguefile.com/photos/morguefile/44/boxes/pop)*

Packing is part of the inventory cycle that is often mentioned in relationship to picking and shipping: Pick-Pack-Ship.  It’s a tough topic in FileMaker for two reasons:
1. Packing is so closely related to shipping, making it hard to discuss one without the other.
2. The shipping strategy can determine how much data you need to keep for packing.

Beyond that, there’s also an assumption that your organization needs to know all the packing data.  As we’ll see a bit later, you can package an item and put that item inside another package and another and so on, like a Matryoshka doll.  

![](Packing/http://newleafdata.com/images/FMIR_DSC04228.JPG)
*Image by laramusikanski, https://morguefile.com/photos/morguefile/1/Russian%20dolls/pop*

But you may not need to track each part in each package.  For example, let’s say a customer calls you and says she is missing a piece from her order.  You could say, “That widget should be in envelope A, which came in envelope B.  Envelope B should have been in the small box A, which was in the medium box B, which, of course, was in the main box.”

Alternatively, you just might say, “Ok, we’ll ship the missing piece to you.”

All of that is to say that you may not need to have a separate packing module in your inventory management system (IMS).  Nonetheless, if you were to put one into your IMS, here’s what it might look like.

## Handling Units
When you package something for shipment, the packaging can take several forms.  It might be an envelope, a box, or even a pallet.  The technical term for these is “Handling Unit”.  One handling unit can be put in or on another handling unit, as with the example above.  Presuming you need to track which item is in which handling unit and which handling unit is in or on any other handing unit, you’ll need some way to assign the item to the handling unit.  Here is one example of what a packing layout might look like:

![](Packing/http://newleafdata.com/images/FMIR_Pack.png)

Let’s take a look at the various sections.

## Packing Header
![](Packing/http://newleafdata.com/images/FMIR_PackingHeader.png)

The header table would include items like the following:
 
1. Packing record ID
2. Date
3. Pick record ID.  This is a one-to-one relationship.  In other words, one pick record is processed in a single pack record.  This means you could go to the pick record and store the pack record ID in that table as well.  This would provide “breadcrumbs” through the process.
4. Sales order ID.
5. Shipping address.
6. Packer; i.e. who packed this record.

## Selecting the Handling Units
![](Packing/http://newleafdata.com/images/FMIR_SelectHandlingUnits.png)

The next step is to select the handling units needed.

1. A drop down list of all the available handling units.  You can use either a drop down or a popup menu.  Either way, you’ll want to add a trigger on the field so that when a user selects a handling unit from the list, it populates the handling unit portal below.

![](Packing/http://newleafdata.com/images/FMIR_HandlingUnitsDropdown.png)

2. This portal is a list of all the handling units for this particular record.  Note that it is NOT a list of all the available handling units.  That information is in the drop down list above.  This list indicates the handling units that are needed to ship all the items that were picked for this particular sales order.  In the graphic, the blue highlighted handling unit (HU0124) is the selected handling unit, which is used below.

## Putting the Pick in the Handling Unit
![](Packing/http://newleafdata.com/images/FMIR_PickedIntoHandlingUnit.png)

The final step is put the picked items into the handling units.  This might include the following elements:

1. A portal of all the picked items for that sales order.  The portal includes the quantity picked (Qty), the quantity that is already packed into a handling unit (Packed), and the quantity that is yet to be packed into a handling unit (Bal).
2. A Select popover button in the portal of picked items that assigns a quantity of picked items into a selected handling unit.  In this layout example, a button “Pack Balance” allows the user to assign the remaining balance to the selected handling unit.  Below that, an option exists for the user to enter a quantity and click the “Pack” button.  Obviously, rules should exist somewhere (field level or script level) that do not permit a user to pack a quantity greater than the quantity balance field (Bal).


![](Packing/http://newleafdata.com/images/FMIR_SelectPopover.png)

3. At the bottom of the Picked portal might be a button that can take all the remaining balance quantity and put it into the selected handling unit.  This is especially helpful when there is only one handling unit for a pack record.

![](Packing/http://newleafdata.com/images/FMIR_PackAll.png)

## The Packed Handling Unit Portal
![](Packing/http://newleafdata.com/images/FMIR_NowFillingHandlingUnit.png)

You’ll need a way to visually monitor which handling unit is being packed.

1. The “Now filling…” indicates which handling unit is currently being filled.  This should match the highlighted portal row from above (HU0124).
2. The portal indicates what is inside the handling unit.  In this example, notice that two other handling units (HU0123 and HU0125) are inside HU0124.
3. The Packed column indicates the quantity of those items.  In our example, there are two manilla envelop 1013, one box 080808 (which must logically be smaller than the current handling unit box 221712), and three medium white widget series 1 which is an item.
4. The weight and the weights unit of measure are also part of the portal columns.
5. The packer may need to add packing material (e.g. bubble wrap, paper, etc.) to the handling unit to decrease shifting of objects during shipping.  A dropdown at the top of the packed handling unit portal is one way of selecting the material to add.  A trigger on the field can add the selected material to the portal.

![](Packing/http://newleafdata.com/images/FMIR_SelectPackingMaterial.png)

![](Packing/http://newleafdata.com/images/FMIR_PackingMaterialAdded.png)

## Add to Transaction Table
The last step is to log the movement of an item into a handling unit by creating transaction records.  There are several ways to do this.  In this example, there is a button on the lower left of the layout called “Post to Shipping” which can make the all the necessary transactions at once.

## Handling Units and Packing Material as Inventory
You’re going to go through handling units and packaging materials.  You’ll need to purchase them, keep track of quantities available, and know when to re-order.  One way to do this is to include handling units and packing materials in your item table.  These are inventory items.  If you’re a manufacturer, you already have an item field of type for Raw Material, Work-in-Progress, and Finished Good.  If you need to track or report on handling units and packaging materials separately, then you can add each as an inventory type.  Otherwise, declaring both as a package type of inventory may be sufficient.
