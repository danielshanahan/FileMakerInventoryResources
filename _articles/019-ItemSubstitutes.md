---
layout: article
title: Item Substitutes
description: How and why to create a list of substitues for products.
permalink: /Item-Substitutes.html
created: Created 2017-06-13
---
![Item Substitute in FileMaker Pro](http://newleafdata.com/images/fmp_ItemSubstitutes.png "Substitution Options for an Order Line Item")

There are a couple of reasons to track substitutes for inventory items:

1. Opportunity
2. Alternative

## Opportunity
The first reason to track an item substitute is to offer customers a higher valued product at a different price point.  For example, you may have a steel plated widget.  For $n more, a customer could get a bronze plated widget and for another $n more, a customer could get a gold plated widget.  Obviously, you’ll need to make the case to the customer that the gold plated widget provides them with more value than the bronze and the bronze plated widget provides more value than the steel plated widget.

In the opportunity scenario, all three items are available at the same time.  They may be items in stock or they may be ordered as needed, but the main point is the value to the customer is not necessarily availability.

## Alternative
A second reason for offering an item substitute is to provide your customers with an alternative in the event that their primary choice is not available.  For example, you may have a yellow widget, a green widget, and the wildly popular chartreuse widget.  In our example, all widgets are the same price.  A customer requests a green widget but you’re out of stock.  At this point, there are three options:
1. **Create a backorder**.  The customer is willing to wait until your next shipment arrives.
2. **Offer an alternative**.  The customer really wants green and isn’t willing to wait until the next shipment.  However, they are willing to go with the yellow widget or the wildly popular chartreuse widget instead, since those are readily available.
3. **Lose the sale**.  There’s not much to that can be done about this option, but if you offer an alternative then you give the customer one more step before they decide to go elsewhere.

## The Gradual Importance of Item Alternatives
In your inventory, it may be more important for some items to have an alternative than others.  Some items are a type of “magnet” product.  That is, when the customer purchases this item, they are likely to purchase other items, often of less value.  For example, if a customer would like a carton of paper, they may also purchase paper clips and staples.  The carton of paper is a magnet product as it “attracts” other items.  If you’re out of paper, it may be more likely that the customer will not purchase the paper clips and staples and instead go to another store and purchase them all together.  If you’re out of paper clips, the customer may still purchase the paper and staples.

On the other hand, if a customer is purchasing paper clips, they may not be looking for a carton of paper.  Paper clips, in this example, is not a magnet product.  

For this reason, it is more important to have item substitutions for your magnet items.  You certainly can also have substitutions for non-magnet items but the since the potential revenue lost is greater for a magnet item, that’s a good place to start.

## Substitute Table
![Substitute Table in the Relationship Graph](http://newleafdata.com/images/fmp_ItemSubstituteRG.png "Substitute Table in the Relationship Graph")

Storing item substitutes is a very similar process to creating a [bill of materials (BOM)](http://filemakerinventoryresources.com/Bill-Of-Materials.html).  Like the BOM, the substitute table is a join table between an item self-join.  One way to express the data model is this:

ITEM ——< SUBSTITUTE >---- ITEM

where the ITEM table is one and the same entity and simply duplicated to show the relationship.  The trick, then is to figure out what to call the foreign key.  As mentioned in the [BOM](http://filemakerinventoryresources.com/Bill-Of-Materials.html) article in the “What’s in a Name” section, we can’t have two itemID fields:

ITEM::ID = SUBSTITUTE::itemID
SUBSTITUTE::itemID = itemID

So we have a need for two itemID fields in the SUBSTITUTE table and, of course, each field must have a unique name.  I use itemID and itemIDInfo, but there are many options (again, refer to the [BOM](http://filemakerinventoryresources.com/Bill-Of-Materials.html) article for some name alternatives and data model graphs.

*For the section on alternative substitutions, I drew upon the writings of Edward Frazelle and his book [Inventory Strategy](https://www.amazon.com/Inventory-Strategy-Maximizing-Operations-Performance/dp/0071847170/ref=sr_1_1?s=books&ie=UTF8&qid=1497279817&sr=1-1&keywords=inventory+strategy)*

## Universal Table
As mentioned above, the substation table is very similar to the BOM table.  So much so, that you could use one table to for both purposes.  You’ll need to have a “type” field to indicate whether this is a BOM or a substitution.  And obviously, you would want to name the table with a generic name like RELATEDITEM:

RELATEDITEM::ID
RELATEDITEM::itemID
RELATEDITEM::itemIDInfo
RELATEDITEM::type

Doing this would likely reduce the number of table occurrences on your graph.  However, it would also increase the query and/or filter criteria whenever you want to find a particular BOM or substitution.

#FMIR
