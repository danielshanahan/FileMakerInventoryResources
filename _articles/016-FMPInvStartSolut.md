---
layout: article
collectiontitle: FMP Inventory Starter Solutions
title: FMP Inventory Starter Solutions
description: Inventory Starter Solutions from FileMaker Pro v11-15.
permalink: /FMP-inventory-starter-solutions.html
published: true
created: Created 2017-06-06
---
I was curious about the FileMaker Starter Solution for Inventory management in recent versions and I thought it would fun to create an article showing its evolution (at least from version 11 through 18).  There's a change in version 17, which I describe below.

I think all the files are interesting to open, reverse engineer, see different techniques.  **However, I do not recommend starting an inventory project with these files!**  They simply aren't robust enough.

The screenshots are only in Mac, but of course, FileMaker also runs on Windows.

Enjoy!

### FileMaker 11 (.fp7)

![](assets/images/fmp_Inventory_Starter_Solution_11.png)

### FileMaker 12 (.fmp12)

![](assets/images/fmp_Inventory_Starter_Solution_12.png)


### FileMaker 13 (.fmp12)

![](assets/images/fmp_Inventory_Starter_Solution_13.png)


### FileMaker 14 (.fmp12)

![](assets/images/fmp_Inventory_Starter_Solution_14.png)


### FileMaker 15 (.fmp12)

![](assets/images/fmp_Inventory_Starter_Solution_15.png)


### FileMaker 16 (.fmp12)

![](assets/images/fmp_Inventory_Starter_Solution_16.png)


## Starter vs. Sample
Starting with version 17, FileMaker Inc. (FMI) made two inventory files available.  One called a Starter and the other called a Sample.  
![](assets/images/fmp_Starter_Sample.png)

The distinction, according to FMI,is that the Sample apps show the possibilities of what can be done.  The presumption, then, is that the Starter is a file to base your project.

Frankly, I don't find this distinction helpful at all.  The sample file appears to be the same as version 15 (not 16), with three tables: Inventory, Company Info, and Stock Transactions.  The Starter file only has two tables: Products and Inventory Transactions.  By contrast, my ![FM Inventory Starter file](https://www.fminventorystarter.com/) has 40 tables (plus a table template, which I didn't count).

The noticeable difference is that one seems to be designed for the desktop (Sample) and the other for the iPad (Starter).  There's also a slight difference in table and field names.

The selector wizard has a nice interface, which shows the main detail view of the file.  In previous versions, if you wanted to look at the user interface (UI) you had to download the file and open it.

![](assets/images/fmp_Inventory_Starter_Select.png)

![](assets/images/fmp_Inventory_Sample_Select.png)

### FileMaker 17 Sample (.fmp12)
![](assets/images/fmp_Inventory_Sample_Solution_17.png)

### FileMaker 17 Starter (.fmp12)
![](assets/images/fmp_Inventory_Starter_Solution_17.png)


