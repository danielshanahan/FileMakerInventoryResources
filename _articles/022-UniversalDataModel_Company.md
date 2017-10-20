---
layout: article
collectiontitle: Universal Data Model - Company
title: Universal Data Model - Company
description: Reduce your data schema by combining vendors and customers.
permalink: /Universal-Data-Model-Company.html
created: Created 2017-08-09
---
*Note - in this article I write about companies, vendors, customers, and organizations.  It can get a little confusing, so here a dictionary of terms.* 

**Organization** - Your company.  The company that is building a database, has need for a database, is interested in the FileMaker platform.  In other words, you.

**Vendor** - Companies that you buy from.  Synonymous with supplier.

**Customer** - Companies that buy from you.

**Company** - Any organization that is not yours.  This term will be used to merge the Vendor and Customer table - but I’m getting ahead of myself.
- - - -
## Intro
For the relational database management system (RDBMS) novice it can be tricky to figure out what tables are needed for a project and what fields should go into those tables.  Once you get the hang of RDBMS, particularly the [normal forms](http://www.studytonight.com/dbms/database-normalization.php), then it becomes easier to map out the tables and fields.

Another level of understanding for RDBMS is something known as the Universal Data Model.  The idea is that some entities (i.e. data tables) are so similar to each other that they can be combined into one entity.  For example, most inventory systems will have a table for vendors and a table for customers.  They are clearly two distinct things.  We buy from one and sell to the other.  An yet, when we look at the fields for these two entities, there is a lot of overlap:

Company Name
Company Address
Company Phone
Company Contact

These fields (which are just a sampling) are in both the vendor table as well as the customer table.

## Reduce Data Redundancy
Before I describe one potential action to take based on the the similarities of the vendor and customer table, let’s back up a bit.   And by “back up” I mean let’s go back to the late 60’s/early 70’s.   Prior to RDBMS it was difficult (if not impossible) to reference data which meant that data was often duplicated.  (It should be noted that I’m going off of research and not personal experience.  I was but a wee laddie in the 1970s!).  Duplicate data caused issues with data integrity.

For example, in a flat-file system (like a spreadsheet), you might have a purchase order sheet like this:

![](http://newleafdata.com/images/Flat_File_PurchaseOrder.png)

There are several data integrity issues:
1. Widgets, Inc. is spelled two different ways (PO1009 and PO1007).
2. Widget World’s address is spelled two different ways (PO1006 and PO1005).
3. Widget items are sometimes separated by a comma (PO1009) and sometimes by a carriage return (PO1006 and PO1005).
4. Order quantities are sometimes given in parenthesis (PO1009, PO1008, PO1007, PO1006) and also as the end of a set of values (PO1005).

One of the key goals of RDBMS was to [reduce data redundancy](https://en.wikipedia.org/wiki/Database_normalization) and organizing data relationally helped clean up these sorts of data integrity issues.

## Reduce Schema Redundancy
Although I haven’t read this explicitly, my take on the Universal Data Model is that just as it is beneficial to reduce data redundancy, it may also be beneficial to reduce schema redundancy.

To give an example of schema redundancy, let’s look again and vendors and customers.  I’ve already mentioned that there is overlap in the fields for each of these tables.  By combining these two tables into one we’ve reduced the schema data.  I suspect this has more organizational benefits than performance benefits, but I’ve not performed any benchmark tests, so this is purely speculation.

![](http://newleafdata.com/images/vendorCustomerTables.png)


We can call this newly combined table “Company” because both vendors and customers are companies.  Once we have the two tables together we’ve solved a data issue that I haven’t presented yet.  That is, if we have one table for vendors and one table for customers, what do we do if we have a company that is both a vendor and a customer?  Admittedly, not every organization will have this need.  But in those instances where a vendor can also be a customer, you would need to add that company’s data in two different places: the vendor’s table and the customer’s table.  That causes data duplication, which is not terrible in and of itself.  But it does become a problem when the data needs to be updated.  Updating duplicate information requires the user (the one doing the updating) to remember to make the same changes in two different places.

By combining the vendor table and customer table, we’ve solved that issue.  Data is only listed once.  But now we have a new issue - how do we designate a customer and a vendor?  

## Customer or Vendor or Both?
There are several methods we can employ in order to designate a company record as a vendor, a customer, or both.  But before we do that, the first question to ask is why do you need to identify them as such?  It might be that the desire to separated them is simply a by-product of initially having them in two different tables.  So the fist thing to do is to come up with a business reason to mark a company record as a vendor, customer, or both.

Another thing to consider is that there is a built-in method for finding vendors and customers.  We can find that information via related orders data (I’ll cover the Universal Data Model for orders in another article).  If a company record has related purchase orders then they are, by definition, a vendor.  Similarly, if a company record has related sales orders then they are a customer.  If a company record has both purchase orders and sales orders then they are both a vendor and a customer.  That means that the company type can be determined without any additional fields on the company table.

A script to find vendors might contain something like this:
```
Set Error Capture [ On ]

Enter Find Mode
Set Field [ ORDER:customerID ; CUSTOMER:ID ]
Set Field [ ORDER:type ; “Purchase Order” ]
Perform Find

If [ Get ( FoundCount ) > 0 ]...
	# this company must be a vendor
End If
```

## Customer or Vendor or …
However, it is not fool-proof.  Sometimes, organizations will keep their potential customers in their inventory management system.  This creates a new category: Prospect.

So, with three categories: Vendor, Customer, and Prospect we could use the relationships to figure out the customer type:

**Vendor** - has purchase order(s)
**Customer** - has sales order(s)
**Prospects** - does not have any orders

That might work.  Or, you might also keep potential vendors in the inventory management system as well.  That muddies the waters, as they say.  And you may even have other types of companies in the database as well.  That makes determining the company type a bit more difficult.

## Marking Company Type
If you need to mark the company type there are several ways to do it, including:
• Use a Drop-down list (or a Pop-up menu).
• Use a flag field.

The Inventory Starter File, which is part of the [FileMaker Pro Inventory Starter Kit](https://gum.co/jiphP), uses flag fields:

COMPANY::customer
COMPANY::vendor

These are both number fields which are empty (or NULL) by default.  When a company becomes a vendor (e.g. after their first purchase order), the flag field can be set to 1.  This can be performed via script which would keep the company type update to date.  You can easily add other fields such as prospects or potential vendors, etc.  Just make sure to adjust those flag fields once a condition changes.  For example, you’d want to remove the 1 from COMPANY::prospect as soon as that company had a sales order.

Another thing to keep in mind - how long is a company a customer?  This is a business question that only you and your organization can answer for yourselves.  For example, if someone ordered once from you 10 years ago, are they still a customer?  Should they still be treated like a customer?  Or would they be better treated like a prospect?  (This is presuming there are different actions based on where a company is in your sales funner).

## Keeping Separate Tables
Merging the vendor and customer table makes sense when there is a large overlap of fields.  That is, the same fields or the same data is stored on both tables.  However, if one or both have a lot of specific fields then you may be better off keeping the tables separate.

## Where’s the Party?
People familiar with the Universal Data Model may be wondering where the party is.  “Party” the term frequently used when combining the company and person tables.  The party model, as it is often called, works best when you want to keep track of persons and their companies.  It allows for a many-to-many relationship between persons and companies (which, of course, requires a join table).  The party model enables an organization to keep track of a person’s affiliation with many companies, even when s/he is no longer associated with that company.

I did not incorporate the party model into the Inventory Starter File.  In my experience, mosts organizations just need to keep track of a contact at a company.  The contacts change and there is no need to retain data in the event that the contact goes to another customer or another vendor.  If that is important for you, then you may want to consider implementing the party model to replace the company and person tables.  See NLD Encounters below for an open file that implements the party model.

## Resources
If you’re interested in the Unified Data Model, I highly recommend [The Data Model Resource Book, Revised Edition, Volume 1: A Library of Universal Data Models for All Enterprises](https://www.amazon.com/Data-Model-Resource-Book-Vol/dp/0471380237/ref=sr_1_1?ie=UTF8&qid=1502210469&sr=8-1&keywords=universal+data+model) by [Len Silverston](https://www.linkedin.com/in/len-silverston-4845188/).  This is my “go to” book.  While I don’t re-read the whole book every year, I often revisit various sections in the book.

[What are Universal Patterns for Data Modeling?](http://www.b-eye-network.com/view/9644)

[Party Domain Model](https://www.ibm.com/support/knowledgecenter/en/SS2U2U_10.0.0/com.ibm.mdmhs.data.dict.doc/c_PartyDomainModel.html)

[Party Data Model](https://docs.oracle.com/cd/E63029_01/books/Secur/secur_accesscontrol022.htm)

If you’d like to see the Party Model incorporated in a FileMaker database, I have a couple of options:

[NLD Encounters](https://gum.co/Jbkz) ($19.99 USD)
This is an open contact management file that I created a few years ago in FileMaker Pro 13.
