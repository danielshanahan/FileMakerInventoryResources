---
layout: article
collectiontitle: Lots and Batches
title: Lots and Batches
description: A discussion about product that is created in lots (or batches).
permalink: /Lots-and-Batches.html
---
Products that are assembled (manufactured items and kitted items) are often done in lots or batches.  The two words will be used interchangeably for this discussion.  When a lot is created the product is given a lot number.  If you go to your medicine cabinet and look at the bottom of your aspirin, ibuprofen, cough medicine, etc., you’ll likely see a lot number.  This enables provides traceability, in cases such as a recall.

There are three important things to keep in mind when working with lots:

1. Assembly Process
2. Expiration Date
3. Sales Order Selection

## Assembly Process

The core recipe of an assembly process is the item’s Bill of Materials (BOM).  The items used to manufacture the new item may itself be a lot.  For example, let’s take chocolate chips cookies with homemade chocolate chips.  The BOM Tree might look like this:

Flour <br />
Baking Soda <br />
Eggs <br />
Butter <br />
White Sugar <br />
Brown Sugar <br />
Vanilla <br />
Chocolate Chips<br />
—Baker’s Chocolate <br />
—Sweetener

The chocolate chips are assembled in a separate process with a lot number.  So the stock quantity might look something like this:

![enter image description here]({{ site.baseurl }}/assets/images/chocChipLotQties.png)

The first thing to understand about lots is that they are often made from other lots.  For our batch of chocolate chip cookies, let’s say we need 200 oz. of chocolate chips.  There are three lots that can provide that quantity so which one do we choose?  The oldest lot?  The freshest lot?  The most convenient lot?

All things being equal you will likely choose the lot that expires soonest.  This is often known as FEFO - First Expired, First Out.  In our example that means choosing lot 200142.  In FileMaker, we can programmatically go down the BOM list and find the lots of each item and, in turn, find the one that will expire soonest.

Let’s take our same example of chocolate chip cookies and instead of needing 200 oz. we need 500 oz.  Now we have a new question.  Can a lot be built from multiple lots of a single item?  That is, can we take 200 oz. from lot 200142 and 300 oz. from lot 200143?  If the answer is no, then we’ll need to take 500 oz. from lot 200143.  But that means that lot 200143 might expire before we have a chance to use it.

The answer to that question will depend on your organization, your product, and possibly even your customers.  If the user creating the assembly order needs to clarify what lot is selected, then a user interface will be needed, like so:

![enter image description here]({{ site.baseurl }}/assets/images/Assembly.png)

## Expiration Date

In our example, we’re creating a batch of chocolate chip cookies.  That batch will have an expiration date.  As mentioned above, many or all of the items that make this final product have an expiration date.  To determine the expiration date of the chocolate chip cookie batch, all we need to do is figure out the earliest expiration date in items used.  This can be done using the Min () function.

As a simple example, given two tables: TABLE1 and TABLE2, the Min () function can “look through” from TABLE1 - the parent table - down to TABLE2 - the child table - and calculate the minimum value.  In our example, we want the minimum date.

![enter image description here]({{ site.baseurl }}/assets/images/Fmp_expirationDate.png)

The expiration date for the cookies is 11/30/2016 because it is the earliest date in the related list (Vanilla expires on that day).

## Sales Order Selection

Lastly, let’s consider lots from the Sales Order process.  We need to know if our customer will accept mixed lots.  Let’s say we have 30 cases of Chocolate Chip Cookie in the following lot quantities:

|    Lot   | Expiration Date | Qty of Cases |
|    ---   |            ---: |         ---: |
| chc89117 |      01/15/2017 |           10 |
| chc89116 |      03/15/2017 |           15 |
| chc89115 |      05/15/2017 |            5 |

If a customer orders 11 cases we need to know if that customer will accept mixed lots.  If she does, then we we sell her 10 from lot chc89117 and 1 from lot chc89116, which is great because we’re selling the first expired.  That means we’re less likely to have outdated inventory that will have to be reduced in price or thrown out.  However, if our customer does not accept mixed lots, then to fill 11 cases, we’ll have to sell her lot chc89116.