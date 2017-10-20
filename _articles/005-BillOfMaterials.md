---
layout: article
collectiontitle: Bill of Materials
title: Bill of Materials
description: A Bill of Materials is a recipe for creating other products.
permalink: /Bill-Of-Materials.html
created: Created 2017-03-11
---
## Bill of Materials (BOM)

A Bill of Materials is a list of items needed to create another item.  At a minimum the list will include the component items, quantity needed, and unit of measure.  The BOM is a recipe, of sorts.

The tricky thing about the BOM table is that it serves as a join table between the item table and the item table.  That might sound a little like [Who’s On First?](https://youtu.be/kTcRRaXV-fg) The item table needs to refer to itself.  It needs to gather data from other items.  One item can have many BOMs and one BOM item may be a component of many other items.  That creates a many-to-many relationship.  Since we can’t have a many-to-many relationship directly, we need a join table in between.  That gives us a model like so:

![Bill of Materials data model]({{ site.baseurl }}/assets/images/itemBOMRel.png)

## What in a Name?

You may notice in the data model that there are two itemID fields, but a database cannot have the same name for two fields.  This is true for any relational database software, not just FileMaker Pro.  You may already have a naming convention for foreign keys, but what happens when you need two foreign keys from the same table?  You’ll need to be a little creative!  Here are a few suggestions:

itemID<br />itemIDInfo

itemID1<br />itemID2

item1ID<br />item2ID

parentItemID<br />childItemID

I use the first one but there’s no hard and fast rule.  You just need a name that you will understand and fits with your naming convention.  The FileMaker Relationship Graph (RG) might look like this:

![FileMaker Bill of Materials Relationship Graph]({{ site.baseurl }}/assets/images/fmp_itemBOMRel.png)

One temptation is to use the item number and/or the SKU (Stock Keeping Unit) as a foreign key.  This is not a best practice as the item number and SKU can change (I’ve seen it happen more than once!).

## BOM Layout

The list is easy enough to create, using a portal from a related table.  If you allow the addition of related records through the relationship then a user simply selects the new item in the portal.   If you have a long list, you may want to add the fields in another manner, such as globals.  That way the user doesn’t have to scroll down to create a new record.

Something else to consider is who may create or change a BOM.  Depending on the size of your organization (remember, FileMaker Pro can handle a couple hundred concurrent users) it may be smart to lock down the BOM so non-authorized people can only read the list.

![Creating a BOM in a portal]({{ site.baseurl }}/assets/images/fmp_BOMDataEntry1.png)<br>
*Checking “Allow creation of records” on the “Many” side of the relationship enables users to easily add items to the BOM.*

![Create a BOM from global fields]({{ site.baseurl }}/assets/images/fmp_BOMDataEntry2.png)<br />
*Another method is to use global fields to populate the portal.*

## Traveling through the BOM

As mentioned previously, the BOM is composed of other items.  It makes sense, then that a user might want to look at that item’s information.  Having a navigation button in the BOM portal allows a simple and intuitive way for the user to navigate through the BOM’s items.  

Going from the parent to the child record may be likened to moving from the branch to a leaf.  Of course, what goes down must come up, so it may be helpful to have another list that shows all the items to which a particular item belongs.  I call this the Found In table.  That is, this particular item is found in these other items.

![Found In]({{ site.baseurl }}/assets/images/fmp_FoundIn.png)<br />
*The Found In relationship indicates where a particular item is used.*

Again, having a navigation button in the portal makes it easy for the user to move up the tree (from leaf to trunk, as it were).

## BOM Tree in Whole

You may have noticed that while we can traverse the tree in both directions, we the current relationships do not allow the user to see the whole tree at a glance.  To do that, take a look at the article on the BOM Tree.
