---
layout: article
collectiontitle: Sales Order Channels
title: Sales Order Channels
description: There are a number of ways for sales request to reach you.  This article discusses some of those ways and the impact it may have on building your sales order module.
permalink: /Sales-Order-Channels.html
published: true
created: Created 2017-09-19
---

From a sales perspective, it is great to have multiple channels for selling your products.  However, from a software application perspective, various channels introduce new challenges.  This article is not a definitive tome on integrating various sales channels with FileMaker Pro.  It is more of an awareness tool.  The article is meant to help you think about your process and the various benefits/challenges each channel offers your business.

Some common sales order channels are

1. An internal sales team that receives orders via phone, email, or fax.
2. Sales reps in the field.
3. A shopping cart on your company’s web site.
4. Third party seller (e.g. Amazon, eBay, Houzz, Sears, etc.)

## An internal sales team
From a software application perspective, the easiest way to handle sales orders is to have your customers call, email, or fax in their orders to your staff.  It is not necessarily the most efficient way, because your staff will need to key or re-key in data.  However, the software application is accessed directly and there is no need to sync data or push/pull data via ODBC/JDBC or API.

## Sales reps in the field
### Device
What device do the sales reps have?  Laptops?  iPads?  iPhones? Windows tablets?  Android phones?  Each of these will require different layouts.  Moreover, FileMaker Inc. has a mobile app for the iOS devices (iPad and iPhone).  At the time of this writing, all other tablets and phones can only access a FileMaker database via a web interface.  The quickest and easiest way to do that is with [WebDirect](http://www.filemaker.com/support/technologies/webdirect.html).

### Connection
You’ll need to know how your sales reps will connect to the database when they are out in the field.  The best way for them to connect is to have a live connection.  That is, when they create orders, they are logged into the hosted FileMaker database, even though they are off-site.  This method enables sales reps to access the latest data.  

Consider this scenario: a customer calls your company and places an order.  The call is received inside your building (or somewhere on your campus).  The staff member creates the order while speaking with the customer on the phone.  At the same time, a sales rep is on-site at a customer’s location, discussing the customer’s needs with the customer.  When it is time to place the order, the sales rep opens FileMaker Go on her iPad and connects to the hosted database via wi-fi or cellular network.

When the staff member on the phone call makes a change to inventory quantities, the field rep sees those changes almost immediately (depending on the distance and connection of the WAN).

Unfortunately, not all locations have wi-fi or cellular access.  So you may run into a scenario where your field rep has to take orders off-line.  This increases the complexity of the process.  Now, the sales rep will need to open her database and download the item and quantity data when she has wi-fi or cell access.  Then she can work off-line with that data set.  After she has completed her orders (e.g. end of the day), she can re-connect to the hosted database once she is back to a place with inter/intranet connectivity.

## A shopping cart on your company’s web site
### ODBC/JDBC
If you’re running a shopping cart on your website, you may have several options for connectivity.  FileMaker supports connectivity with a number of [SQL based data sources](http://www.filemaker.com/support/technologies/sql.html), which can be accessed via ODBC/JDBC (Open DataBase Connectivity/Java DataBase Connectivity).  ODBC/JDBC requires a certain amount of access and control of the SQL database, which you may have if it is open source and you host it on your own web server.

### API
Another option is to use the shopping cart’s API, if it has one.   API calls are usually returned in an XML or a JSON format.  Newer API’s are likely to have JSON and FileMaker 16 has a new set of JSON functions to help parse the API return.  Unfortunately, FileMaker Pro doesn’t have any native XML functions for parsing, so you’ll need to use a plugin or a custom function.

## Third party seller
Third part sellers can be powerful because they have two things every online sales platform needs (including your shopping cart):
1. A positive reputation.
2. Traffic

As a seller on their platform you have very little access to their data structure (and understandably so).  So third party sellers will likely have an API for users to push and pull data.

## Staging table
When ordering takes place online, such as with a shopping cart on your website or a third party seller, you may want to bring that data into a staging table rather than directly into the sales order tables (ORDER and ORDERLINE).  The staging table provides an intermediary place for the data.  Data can be checked, verified, and if necessary, cleaned while in the staging table.  It also servers as a type of log.  Once the data is copied over to the sales order record, then it may be possible to make changes on that record.  Having the data on a staging table also enables users to go back and see the data as it was entered by the user.
