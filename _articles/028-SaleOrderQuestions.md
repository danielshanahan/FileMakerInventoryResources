---
layout: article
collectiontitle: 11 questions to ask before building your sales order module
title: Type Title Here
description: How to plan for your sales order module.
permalink: /11-questions-to-ask-before-building-your-sales-order-module.html
published: true
created: Created 2017-09-19
---
Ok, first of all, there might be 12 questions.  Or 16.  These are the ones I thought of but you might have more.  The point is, sales orders can be complex so it’s beneficial to think through the requirements before building.  Here are the ten questions I came up with:

1. How many sales channels do you have?
2. Do you have a push or a pull system?
3. When is a sales order closed?
4. Can a sales order be recurring?
5. Do you allow backorders?
6. Does your customer accept backorders?
7. Do you ship partially filled orders?
8. Are your items in lots?
9. Does your customer accept mixed lots?
10. From where will the order be fulfilled?
11. Do you have common requests?

## How many sales channels do you have?
A “sales channel” is how a customer conveys to you her/his intention to purchase your products.  They may include:

1. An internal sales team that receives orders via phone, email, or fax.
2. Sales reps in the field.
3. A shopping cart on your company’s web site.
4. Third party seller (e.g. Amazon, eBay, Houzz, Sears, etc.)

The internal sales team is the simplest method as all the item and quantity data can remain inside a single database.  For more on the complexity of the sales channel, see the [Sales Channel article](http://filemakerinventoryresources.com/Sales-Channel.html). 

## Push or pull system
A push system (as well as a push-pull hybrid) means that you will almost certainly have inventory in stock.  You’ll likely want to pass the quantity data to your sales channels in order to let customer’s know whether an item is in stock or not and if not, how long they’ll have to wait.

## When is a sales order closed? 
It may seem obvious that a sales order should be closed after all the requested line items have been shipped.  But what happens if some items are backordered?  Should the sales order remain open?  Would it make a difference if the backordered item has a lead time of 2 days?  What about 30 days?  What if your supplier is out of that item and the next shipment is expected in one year?  Is there any harm in keeping a sales order open?  Are commissions determined upon sales orders?  If so, does the sales order status help determine the commission?

I don’t think there are right or wrong answers.  The point is to ask these questions so you can configure the sales order the way that best supports your optimal processes.

## Can a sales order be recurring
Recurring sales orders are also known as standing orders.  Your customer requests a regular shipment every Thursday; or once a month; or once a quarter, etc.  The idea is that your customer doesn’t want to set a calendar task for him/herself to create the order.  She’s thinking of it now, so why can’t she just contact you and say, “We’d like 1,200 blue widgets this year.  Have 100 of them delivered on the first of every month.  Thanks!”

## Do you allow backorders
If you are not able to fulfill a sales order request, will you backorder the item?  There may be scenarios where you do not, such as limited manufacturing runs.  For ideas on how to handle backorders, see [6 ways to handle backorders](http://filemakerinventoryresources.com/6-ways-to-handle-backorders.html).

## Does your customer accept backorders
Even if you allow backorders, your customer may not want to wait for a backorder.  

## Do you ship partially filled orders
Image you sell large tents for outdoor events.  On one sales order you have all the posts, all the ropes, and all the stakes.  The only thing missing is - the tent!  Admittedly in this scenario, you’d probably bundle these items into a kit and with a missing tent you would not have any available kits.  But the point here is to ask the question, would you still ship the order?

## Are your items in lots?
If you manufacture items in lots (also known as batches), then you’ll need to have some way to select the lot as well as the item for a sales order.  Presuming that you want to sell the lots that will expire soonest (FEFO - First  Expiry First Out), you could automate this process.  But if you want the order entry team to view the lot information before committing it to a sales order, then you’ll need some type of user interface for selecting the lots.  That could be a portal in a popover, a modal window, or a card window which is new in FileMaker 16.

## Does your customer accept mixed lots?
You may have customers that accept mixed lots.  And some that don’t.  You might even have customers that will accept mixed lots but only on certain items.  You can set a flag field to store the information for each customer.  

Flag fields are boolean fields that can quickly classify the record into two categories.  I usually use a number field with 0 or 1 with a single checkbox on the layout but you can also use a text field with Yes or No and use drop downs, checkboxes, etc.  If you use the single checkbox options, then check out Mark Scott’s excellent article on [ways to use button bars with checkboxes](https://blog.beezwax.net/2016/04/28/fun-with-button-bars-check-please/).

The simplest scenario is that your customer either accepts or doesn’t accept mixed lots, regardless of the item.  In that case, the flag field can be on that customer’s record.  However, if it is acceptable to have mixed lots from some items but not others, then the flag field will need to be part of a Customer-Item join table.

## From where will the order be fulfilled?
There may be a number of ways for you to get your product to your customer.  They include:

1. Ship from a single warehouse.
2. Ship from multiple warehouses.
3. Have the supplier ship directly to your customer (a.k.a. drop ship).
4. Have the customer pick up

These options will likely have an impact on the sales order status.  And knowing the statuses of all your sales orders is important for operations.

## Do you have common requests?
If you have certain items that are commonly requested you may benefit from having a sales order template.  This can reduce the repetitive data entry.  Likewise, if you are requesting the same items on purchase orders, then you might like to have templates for those as well.

## Final thoughts
The point of this whole exercise is to determine how to build a sales order module that will best fit your business.  This information can clarify the types of layouts (e.g. desktop, mobil, web), what is on those layouts, and what buttons and/or triggers should be a part of the layout.

One of the major benefits of a customized inventory management system as opposed to an off-the-shelf system is that your processes determine the application behavior, not the other way around.  There is perhaps no other module that emphasizes this than the sales order module.
