---
layout: article
collectiontitle:  Universal Data Model - Keys
title:  Universal Data Model - Keys
description: Creating human readable keys for the order table.
permalink: /Universal-Data-Model-Keys.html
published: true
created: Created 2017-08-11
---
## Intro
One of the things many organizations want to have is a reference number that conveys meaning.  For example, PO20999 tells the organization that they are likely in the Purchase Order table and that there are 20,998 previous purchase orders.  In this article I’ll review the need for primary keys and the techniques I use to add another sequential field with meaning.

## Primary Keys
One of the rules of Relational Database Management Systems (RDBMS) is that every table has a field (or column) that has these four characteristics:
1. Not Null
2. Immutable
3. Unique
4. Sole Key

This field is known as the primary key.  And the primary key must have some kind of value (Not Null), it cannot be changed once it is set (Immutable), it must be different from every other primary key in the same table (Unique), and only one field can be considered a primary key (Sole Key).

## Intelligent Keys
Sometimes people like the primary key to convey information.  For example, take these six primary keys:
1. PO20998
2. PO20997
3. SO20117
4. INV60442
5. REC32199
6. REC32200

Even though you don’t know anything about the database you could probably make a few guesses based on these keys:
1. PO values are primary keys in a Purchase Order table.
2. SO value is a primary key in a Sales Order table.
3. INV value is a primary key in an Invoice table.
4. REC are primary keys in a Receipt table.
5. PO20997 was created before PO20998, even though it appears after PO20998 and even though you cannot see the date/time it was created.
6. REC32199 was created before REC32200, even though it appears above REC32200 and even though you cannot see the date/time it was created.

An alternative to an intelligent primary key is a surrogate key.  This is a value that in and of itself, conveys no meaning.  This can be a number:
1. 20998
2. 20997
3. 20117
4. 60442
5. 32199
6. 32200

When it is a number, it is usually sequential and increments by 1.  We can no longer tell which table the key is from, and in fact, since none of these keys are duplicated in the list, they could all be from the same table.

Another type of surrogate key is the UUID (Unique Identifier ID).  In FileMaker, this can look like this: 
BF7BA7C4-D7CC-4F05-8D58-4833977EEC13
A UUID is unique across all tables, not just unique within a single table.

## Intelligent Keys in Action Field
Intelligent keys can be very helpful, especially for customers and vendors.  Not the customer and vendor table, mind you, but the actual people who make up your customers and vendors.  They will call you.  Or at least, call your business.  Or email.  Or fax (I have one customer that sends and receives orders via fax.  I can’t believe it, but that’s where their industry is).

When people contact you about a purchase order or an invoice, it is nice for them to have a manageable text string as a reference.  20998 is helpful.  PO20998 is even better because it tells your customer service rep that they are dealing with a purchase order and not an invoice.  BF7BA7C4-D7CC-4F05-8D58-4833977EEC13 is terrible.  What person wants to read this back to someone over the phone just so they can get an order record up on their computer screen?  So intelligent keys can be very helpful for both your organization and the people you serve.  

## Intelligent Keys in Sequential Order
I have found that organizations want their intelligent key to be without gaps.  That is, if you have a purchase order of PO20998, then the next one better be PO20999.  If, instead, the next purchase order is PO21000, then someone in purchasing might raise a stink “WHERE THE HECK IS PO20999?”, even though it never existed.  That’s how we are, us humans.  We like things in sequential order.

In FileMaker, you can create an intelligent key using the auto-enter serial field definition

![](http://newleafdata.com/images/Intelligent_key.png)

The problem is, records get deleted.  You definitely need to have rules in place to prevent unwanted deletions (e.g. Purchase Orders with line items cannot be deleted).  But records will be deleted.  It will happen.  And when that happens, the auto-enter serial number does not automatically re-set. And then you will have a gap between your keys.  A dreaded, awful gap.

My solution to this is to have an intelligent key if the organization wants it, but it is never the primary key.  For that, I use a surrogate key (either a sequential number or a UUID).  I don’t care about gaps in the primary key.  Gaps in key values do not break any rules in RDMS.  Gaps don’t prevent the primary key from doing its job.  The intelligent key becomes an intelligent reference, but not a key.  I never use them in connecting table occurrences on the relationship graph.

## Orders Table - Intelligent Reference
In the Universal Data Model where we have one order table, it is no longer possible to set the intelligent key as a serial number because the prefix (e.g. PO in PO20999) needs to change based on the type of order record.  You can still use the auto-enter section, but instead of the Serial number you would use the Calculated value.  Another way to set the intelligent key is to use a script trigger on the order type.

In the demo file for this series, the ORDER table has an intelligent reference field (remember, its not a “key”) in addition to the primary key (ID field in the demo file).  The intelligent reference field has two rules:
1. It needs to be prepended with a code to indicate what type of order it is.  For example PO=Purchase Order, SO=Sales Order, WO=Work Order, etc.
2. There can be no gaps between order records of the same type.

There are a number of ways to approach this.  In the demo file the way I solved this was to create a new Table Occurrence (TO) and relate it to the ORDER table occurrence.

![](http://newleafdata.com/images/UniversalDataModel_RG.png)

Then, on the ORDER layout I put an OnObjectSave script trigger on the orderType field.  Through this relationship I can then determine what the last number of that type of order record is.  Adding a 1 to that number gives me the next sequential value, without any gaps.  If that record gets deleted (and records will get deleted), then that number gets reused.

Now, since we have a number that can be reused, it bears repeating:
1. Use a surrogate key for your primary key field and only use the intelligent field as a reference.  Do not build any relationships from the intelligent field.
2. Create strict rules on when a record can be deleted and who can delete them.  It is better to have existing records with a status of “Cancelled” then to have records deleted.

The script that generates the next value for that intelligent reference field has two methods.  There are, undoubtedly, more.
