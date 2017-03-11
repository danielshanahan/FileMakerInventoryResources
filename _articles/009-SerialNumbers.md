---
layout: article
collectiontitle: Serial Numbers
title: Serial Numbers
description: How to handle product serial numbers
permalink: /Serial-Numbers.html
---
Some products - either purchased or manufactured - may require a serial number.  The purpose of a serial number is to track a unique instance of a product.  To illustrate, let's suppose you have an Asset Management Inventory system and some of the items you track are phones.  You buy phones in bulk and distribute them to your team.  You carry both iPhones and Android phones.  You also purchase phone cases for those phones.  You'll want the phones to have a serial number but you don't need to have a serial number for the cases.

One way to model this data need is to have a serial table:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_erd.png)

Notice the circle to the left of the “crows foot” in the ERD.  This indicates that a serial number is not required for all items.  (Technically, it means that an item must have at least 0 serial numbers).  There isn't a way to diagram the data model to indicate which items require a serial number.

## Mockup

Let's take a look at some mockups of simplified user interface layouts to see how the Item and Serial Numbers might be displayed on screen to a user.  The first mockup shows a list of items:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_ItemListView.png)

Clicking the left caret navigates the user to the detail view:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_ItemDetailView1.png)

Notice that the item record for the iPhone has related Serial Numbers.  There is also a button to add more serial numbers.  Ideally, you would want to have some kind of automated process to enter serial numbers rather than manually enter them.  In our example, we only have 10 iPhones, but if you have 1,000 iPhones, then manually entering that data will take time.  More on that below.

Let's look at the detail view for the iPhone case.  Since we don't need to track serial numbers for this item, the portal is empty.  Moreover, the button is disabled.  Starting with FileMaker Pro 13 we have the ability to hide objects, so this button could easily be hidden instead of disabled.

![enter image description here]({{ site.baseurl }}/assets/images/Serial_ItemDetailView2.png)

You may not need a user interface layout for the serial number table, but if we look at that table in a list view, it might look like this:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_SerialNumberListView.png)

## Selecting Serials

Now that we have a general idea of how to set up serial numbers, we need to turn our attention to how serials are selected upon a sale.  That is, when a customer purchases one or more phones, the serial numbers for those phones need to be specified or selected.  Let's presume that orders have the following flow:

Sales Order –&gt; Sales Order Confirmation –&gt; Pick –&gt; Pack –&gt; Ship –&gt; Invoice

The first possibility is to assign the serial numbers during the sales order.  You could then pass those serial numbers to your customer in the Sales Order Confirmation document, in the event that your customer wants/requests a certain series of serial numbers.  Assigning the serial number during the sales order process would also show up on the Pick layout (either a pick sheet if you use paper or a pick layout if you use a mobile device).  This lets the picker what know the specific phones to pick.

A second possibility is to let the picker select the serial numbers.  S/he will receive the pick request and select the item.  If the picking process is digital, then the picker can scan in the serial number at the time of picking.

## Conveying Serials

Once the serials are selected, the serials need to be conveyed to the customer.  As previously mentioned, if you send a Sales Order Confirmation to your customer, you can send the serials as part of that document.  Another option is to include the serials in the Packing or the Shipping document.  And finally, you could include the serials in the Invoice.

Regardless of which method you use (Packing, Shipping, or Invoice document) there are a number of places to include the serials on the document.  For example, you could include the serials in the description:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_InvoiceDoc1.png)<br />
In this example I've abbreviated the serial numbers to 72335-7 instead of 72335, 72336, 72337.  This saves space, which can be helpful on a PDF or Word document that mimics an 8.5x11 paper.  Obviously, this only works if the serials are sequential.  Otherwise, the serials will need to be listed individual, like the following example:

![enter image description here]({{ site.baseurl }}/assets/images/Serial_InvoiceDoc2.png)

In addition to listing the serials, example 2 has a description for the Android phone.  The point here is that the line item record may have data in it (in this case, the description content).  To set the serials apart but in the same row, you would may want to add a carriage return before adding the serials.  For example:

````
If [ not isempty ( LINEITEM::description ) ]
  Set Field [ LINEITEM::description; LINEITEM::description &¶& $serials)
````

Obviously, you would need to define the `$serials` variable, by using a Loop, List (), or ExecuteSQL().  The point is, a script would see if there is already something in the Description field and if there is, append the serials using a new line but maintaining the same row.

Another option is to list the serials in a notes section.  As with the description field, you may want to put the serials above or below any other message (e.g. “Thanks for your order”, “Save 20% on…”, etc.).

![enter image description here]({{ site.baseurl }}/assets/images/Serial_InvoiceDoc3.png)

## Receiving Serials

When purchasing products that have a serial number, your vendor will need to provide those serials numbers to you (similar to what we did in the Conveying Serials section above).  The important point here is to try and incorporate the serial number data into your system *without* re-typing it in.  Data entry is expensive.  Your vendor already spent resources to get the serials into their data system.  Surely, there is a way to capture that data!

Too often, I encounter people who re-type data simply because they never stop to consider that someone has already done that task.  When you purchase a product that requires a serial number, ask your vendor if they will email you the serial numbers.  At least that way you can copy and paste those numbers.  You'll still need some kind of process for looping through the serials and creating records in the SERIALNUM table but you won't need to re-type.