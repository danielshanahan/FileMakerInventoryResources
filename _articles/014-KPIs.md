---
layout: article
collectiontitle: KPIs
title: KPIs
description: More than just for dashboards
permalink: /KPIs.html
created: Created 2017-03-11
---
{% include figure.html
    url="/assets/images/fmp_dashboard.png"
    alt="Dashboard in FileMaker Pro"
    caption="A dashboard in FileMaker Pro" %}
	
KPI means Key Performance Indicator. They are often a collection of numbers that can give a high view of the performance of the company. KPIs are often displayed in one location: the ubiquitious dashboard!

I don’t know when dashboards became the thing in FileMaker solutions but I do remember when they weren’t common. My solutions and most everything I remember from the time had a “Home” layout that was simply a collection of buttons to navigate through the system. That’s not a bad design by any means, especially when you want to limit who can see the dashboard. But it is just as common today to have a Dashboard layout in lieu of a Home layout.

What is less frequent (at least in my experience) is displaying KPIs throughout the database. An Inventory Management System has a lot of opportunities for KPI displays. Here are a few suggestions to get your creative juices flowing!

### Inventory Measures

The most obvious place to gather KPI data is the product data itself. You may want to know things like:<br />• Frequency of stock-outs<br />• Safety stock quantity<br />• Lead time from preferred vendor<br />• Lead times from alternative vendors<br />• Carrying cost<br />• Purchasing cost<br />• Frequency of damage<br />• Frequency of theft<br />• Frequency of returns<br />• Length of obsolesence
### Customer Measures

Some customer KPI might include:<br />• First contact with our organization<br />• First order from our organization<br />• Time between first contact and first order<br />• Method of introduction<br />• Highest order in dollar amount<br />• Lowest order in dollar amount<br />• Average order in dollar amount<br />• Number of orders placed<br />• Average order placed by (year, quarter, month, week, day)<br />• Products ordered by category (c.f. Product Categorization article)<br />• Number of times an order went unfulfilled (e.g. not available in stock)<br />• Number of times they waited for unavailable stock<br />• Number of times they did not wait for unavailable stock
	
{% include figure.html
    url="/assets/images/fmp_CustomerKPI_All.png"
    alt="Example of a customer KPI"
    caption="An example of a customer KPI" %}
	
### Vendor Measures

Some possible vendor KPIs:<br />• Number of SKUs (stock keeping units) this vendor currently provides<br />• Number of SKUs this vendor has the potential to provide<br />• Number of times they’ve had a stock out<br />• Number of times you’ve received damaged goods<br />• Number of times their product has been returned to you by your customer<br />• Availability of sales rep<br />• Turnaround time for purchase orders

### A Few Caveats

Before you start to sift through all your data, aggregating numbers in order to display bold numbers in 72pt font and colorful charts, consider the following:

**Data Overload**<br />Even though many people would not equate a FileMaker solution with “Big Data”, there may certainly be a lot of data in a FileMaker solution. The trick is to determine if it or its aggregate can serve as a meaning KPI. Just because you have it doesn’t mean you should aggregate it and put it on a layout.

**Table Overload**<br />When creating fields to store the aggregated data it might seem obvious to do that in the table you are measuring. For example, all the customer KPIs in the CUSTOMER table, all the vendor KPIs in the VENDOR table, and all the product KPIs in the PRODUCT table. And it may in fact be the right place. But you may also want to think about creating a separate table that has a one-to-one relationship with the time in question. For example:<br />CUSTOMER::ID = CUSTOMERKPI::customerID

The advantage of a separate table is that it keeps your main table small or “thin”. This is helpful when FileMaker pulls data down from the server. When a record is requested from a FileMaker client then FileMaker Server sends the entire record. If you have more fields in the table then more information needs to come down from the server to FileMaker Pro. Keeping the KPI in a separate table means that the data is does not come down until you need it (e.g. clicking a button, opening a tab control, etc.).

**Meaningful Time**<br />Measurements include the element of time. But which time? Year? Semi-annual? Quarter? Month? Week? Day? Hour? Certainly minute, second, and millisecond could be included for theoretical reasons. As an aside, if you are taking measurements by the minute, then FileMaker may not be your best tool. I know an engineer who receives data on rolled steel. Measurments are taken evey second in order to catch problems. They don’t use FileMaker!
Time measurements can have another dimension. You may track quarters on a horizontal axis but have three lines that display the last three years. Figure out what time measurments are the most meaningful.
	
{% include figure.html url="/assets/images/Multi_dimensional_time_chart.png" alt="A chart covering months and years" caption="This demo chart shows time along the horizontal axis in months and 3 different years within the chart itself." %}
	
**Determine the Need**<br />The first thing to do is ask questions. For example, how long does it take a company to go from a potential customer to a customer? Who are our top 25 customers by volume? Who are our top 25 customers by revenue? How often do they place orders?

After you brainstorm questions (I recommend doing it with others) then go through them and ask why it matters. For example, If you knew how long it took for a company to go from a warm lead to customer, what would you do? Would you change your marketing strategy? Would you change your sales strategy? How much change would you need to make in order to make a difference?

Sometimes there is no obvious answer. Sometimes you just have to see the KPI before you can determine the benefit. Nonetheless, it may be better to start small. The data will always be there for aggregation later.
