---
layout: article
collectiontitle:  Quantity Level Warnings - Part 2
title:  Quantity Level Warnings - Part 2
description: Setting up the user alerts for quantity level warnings.
permalink: /Quantity-Level-Warnings-Part-2.html
published: true
---
## Intro
This is the second of a two part series on creating a quantity warning system for your inventory management system.  The first part focused on time periods for low/high data and where to store that data.  In this second part, I’ll discuss when to trigger a warning and options for notifying users.

## When to trigger a warning
It may be best to trigger a warning at the same time that an item’s quantity fields (e.g. On Hand, Allocated, Available, etc.) are updated.  There are at least three different times this might happen:

1. Upon order quantity modification.
2. Upon order status modification.
3. A scheduled routine.

Each has its own advantages and disadvantages.  I use the generic term “order” as it might apply to multiple orders such as Sales Order, Loan Order, Work Order, etc.  For the sake of simplicity, we’ll take a scenario where a Sales Order is created and thereby reducing inventory quantities.  We’ll also presume a sales rep is doing the data entry.

### Order Quantity Modification
The most difficult method (in my opinion) is to trigger a warning at the level of the sales order line item.  This means you’d need to check whether the level is too low when:
1. A sales order line item is created.  More accurately, when the quantity requested by the customer is added to the line.
2. That sales order line is modified.
	1. The quantity is changed.
	2. The item is changed.
3. That sales order line is deleted.
4. The entire sales order is deleted.

And it can get more complicated from there.  You’ll want to make sure you have a clear understanding of how the organization processes orders - the rules, the exceptions, etc.  The disadvantage is that there are “a lot of moving parts” (an expression meaning a lot of different scenarios to consider).  The advantage is that the quantity fields are up-to-date almost instantly.  A manager could theoretically watch the item quantities change before her eyes (though she’d need to refresh the window; Install OnTimer Script anyone?).  The point is that the data would be as fresh as possible.

### Order Status Modification
Another option is to check the quantity level when the sales order has a certain status.  For this example, let’s say our sales order can be in one of the following statuses:
1. New
2. Processing
3. Partially Completed
4. Completed
5. Cancelled

Let’s presume the following definition for each status:

**New** = a sales order has been created and is still in the creation phase.  No new process (such as picking) has been requested of this sales order.

**Processing** = the line items have been sent to be picked.

**Partially Completed** = only some of the line items could be picked (e.g. stock outs).

**Completed** = the sales order has been picked and shipped

**Cancelled** = the sales order has been cancelled.  Nothing has been shipped.  If anything has been picked, it is returned to its warehouse location.

(I suggest working with the sales order and fulfillment teams in order to come up with a list of statuses that work best for your organization.  As a general rule, I make status updates a part of a script process and do not allow users to manually change the status.)

In this scenario, the item quantities and quantity level would only be updated when the status changes from New to Processing.  The advantage is that line items can be added, modified, and deleted while the status is  “New” and nothing needs to be changed in the item quantities.  Changing a line item quantity from 8 to 10 has no effect of the quantity level because it has not yet been recorded.

The disadvantage is that the quantity data is not as up-to-date as possible.  So if another sales rep is writing another sales order with the same items, one of you will be working from wrong quantities.

### Scheduled routine
A scheduled routine looks at a clock (an internal clock on the computer) and runs a script to make the updates.  You write the script in Filemaker Pro/Pro Advanced and connect that script to a schedule that is created in FileMaker Server.  One method is to have the schedule run once after hours (e.g. when no one is adding, editing, or deleting data).

The disadvantage is that the data is not up-to-date.  The advantage is that it can create a simplified scenario in which to update the item quantities.  For example, a script might include something like:

```
Set Error Capture [ On ]

Enter Find Mode [ Pause: Off ]
Go to Layout [ “Sales Order” ; Animation: None ]
Set Field [ ORDERLINE::zzCreated ; “=“ & Get ( CurrentDate ) ]
Set Field [ ORDERLINE::type ; “Sales Order" ]
Set Field [ ORDERLINE::qtyUpdatedFlag ; 0 ]
Perform Find [ ]

If [ Get ( FoundCount ) > 0 ]
# loop through found set and update item quantities and qty level
End If
```

You could decide to run the script multiple times during a 24-hour period (e.g. twice a day, hourly, once every 15 minutes, etc.).  That will keep your quantity fields and quantity level more fresh.  However, you’ll need to have rules on how to determine if a sales order line has already been counted.  For example, in the code example above, I use the flag field ORDERLINE::qtyUpdatedFlag

## Alerts
Once you’ve decided on when/how you will get the quantity level data, then next thing to do is decide what to do with that information.  Again, presuming we are doing a simple track of when an item quantity is below a certain level, we have a number of options:

1. Update a dashboard
2. Conditionally format the quantity level field and or text
3. Hidden Objects
4. Show an alert inside FileMaker Pro
	1. Show Custom Dialog
	2. Card Window
5. Send out an email
6. Notification in task management

These options are neither exhaustive (there may be more) nor mutually exclusive (we can use several of them at once).

### Dashboard
One options is to update a dashboard.  This is somewhat of a passive alert in that it requires the user to have the database open and to occasionally visit the dashboard.  You can create an opening script that lands the user to the dashboard,  which would give her at least one time of viewing the statuses.

You could also create buttons on the dashboard numbers so that clicking any of the number (e.g. “Below Level Items”) would take the user to a list of those items.

![](newleafdata.com/images/FMIR_dashboard_demo1.png)

### Conditional Formatting
You can use FileMaker’s powerful conditional formatting to change a layout object such as a field.  In this example I’m changing the Available field but you could just as easily change the item field, the SKU field, etc.  This method is even more passive than the dashboard because a user may never navigate to that record to see the formatting.  For this reason, I suggest that this method only be used in conjunction with a more active method.

In the screenshot, notice that the On Hand value is greater than the Level - Low value.  However, 5 are allocated (perhaps on a Sales Order).  That means only 7 are available, which is below the Level - Low field.  Therefore, the conditional formatting on the Available field appears.  In this example I show two: a background format and a text format.  You can combine the two, incorporate font sizes, font styles (e.g. Bold, Italic), or even a separate font altogether.  You have a lot of options.

![](newleafdata.com/images/FMIR_Item_conditionalFormat.png)


### Hidden Objects
Similar to conditional formatting, another passive method is to place objects on the layout that only show when the available level is below the Level - Low.  In this image, there is an alert icon in the upper right corner of the Item layout window with a text about the low levels.  These two objects - the icon and the text - can be hidden while the Level - Low quantity is equal to or above the Available quantity.

![](newleafdata.com/images/FMIR_Item_hiddenObjects.png)

### Alert Message
An alert message can be triggered to show when an item dips below the desired quantity.  This method is more active then the previous ones mentioned.  As a Show Custom Dialog script step, you can create any message that best solves the issue or at least moves it to the next step.  One thing to note is that if items are often going below the low level quantity then you may end up with more Alert Messages than you want.  This could hinder a person’s work flow, so use judiciously.

![](newleafdata.com/images/FMIR_LowLevel_Alert.png)

In addition to the Show Custom Dialog, FileMaker 16 now has the option to display a window in a Card mode.  This offers more options for the user since the entire window can be designed with text, fields, and buttons.  Moreover, unlike the Show Custom Dialog window, a Card window can have buttons of varying length.  Thus, you can have more text inside a button than you can in the fixed-width buttons of the Show Custom Dialog window.

### Send Email
A more active alert is to send a person or group of persons (e.g. the purchasing manager, the manufacturing team, etc.) an email as soon at the Level - Low field drops below the Available field.  FileMaker has the ability to natively send emails using the [Send Mail script step](https://fmhelp.filemaker.com/help/16/fmp/en/#page/FMP_Help%2Fsend-mail.html), which requires the user to have an email application set up on her/his computer.  Note that this is a client application such as Microsoft Outlook or Apple Mail and not a web based email application like Google Mail.

An email alert can be sent at the moment of quantity discrepancy.  However, this may bombard a co-worker with emails - rarely enjoyable for anyone!  Another option is to set up a server-side schedule and have FileMaker Server run a script that looks for discrepancies, create a list of those SKUs and send them via SMTP on FileMaker Server.  Since the script is scheduled as part of FileMaker Server, you can arrange to have them sent at fixed times (e.g. end of a shift, every hour, end of the day, once a week, etc.).

### Notification in Task Management
A task management application might be a better tool to notify someone that an inventory item is below an acceptable level.  Task management tools have flourished in the last few years and most of them enable users to add tasks via email.  Here is a short list with a link that navigates to their task creation via email page.  Please note that this list is neither exhaustive nor a recommendation	from me.

[Asana](https://asana.com/guide/help/email/email-to-asana)
[Trello](http://help.trello.com/article/809-creating-cards-by-email)
[Wrike](https://help.wrike.com/hc/en-us/articles/210324185-Email-Integration)
[Todoist](https://support.todoist.com/hc/en-us/articles/205749772-Todoist-Your-Email)
[Hitask](https://hitask.com/tour?q1)

If your task management tool doesn’t allow the creation of tasks via email, check to see if it has an API.  FileMaker 16’s improved Insert from URL and cURL capabilities makes it easier to communicate with many applications via their API and often times does not require a plugin.

## Conclusion
Setting a quantity level, low and high, are only helpful if some action is taken to alert users about the discrepancies.  There are a number of actions you can take - some more active and some more passive - to help you keep track of your inventory quantities.  Moreover, many of these actions can be used simultaneously.
