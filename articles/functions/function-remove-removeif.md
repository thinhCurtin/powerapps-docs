<properties
	pageTitle="PowerApps: Remove and RemoveIf functions"
	description="Reference information for the Remove and RemoveIf functions in PowerApps, including syntax and examples"
	services=""
	suite="powerapps"
	documentationCenter="na"
	authors="gregli-msft"
	manager="dwrede"
	editor=""
	tags=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2015"
   ms.author="gregli"/>

# Remove and RemoveIf functions in PowerApps #

Removes [records](working-with-tables.md) from a [data source](working-with-data-sources.md).

## Description ##

### Remove function ###

Use the **Remove** function to remove specific records from a data source.  

For [collections](working-with-data-sources.md), the entire record must match.  You can use the **All** argument to remove all copies of a record, otherwise only one copy of the record is removed. 

### RemoveIf function ###

Use the **RemoveIf** function to remove records based on a set of conditions.  The conditions can be any PowerApps formula that results in a **true** or **false** and can reference columns of the data source by name.  Conditions are evaluated individually for each record, and if all result in **true**, the record is removed.

**Remove** and **RemoveIf** return the modified data source as a table.  Both functions can only be used in [behavior](file-name.md) formulas. 

You can also use the **[Clear](function-clear.md)** function to remove all of the records in a data source.

## Syntax ##

**Remove**( *DataSource*, *Record1* [, *Record2*, ... ] [, **All** ] )

- *DataSource* – Required. The data source that contains the records that you want to remove.

- *Record(s)* – Required. The records to remove.

- **All** – Optional. In a collection, the same record may appear more than once.  You can add the **All** argument on the end to remove all copies of the record.

**Remove**( *DataSource*, *Table* [, **All** ] )

- *DataSource* – Required. The data source that contains the records that you want to remove.

- *Table* – Required. A table of records to remove. 

- **All** – Optional. In a collection, the same record may appear more than once.  You can add the **All** argument to remove all copies of the record.

**RemoveIf**( *DataSource*, *Condition* [, ... ] )

- *DataSource* – Required. The data source that contains the records that you want to remove.

- *Condition(s)* – Required. A formula that evaluates to **true** for records to remove.  You can use column names from the *DataSource* in the formula.  If multiple *Conditions* are provided, all must evaluate to **true** for the records to be removed.

## Examples ##

#### Remove records from a data source ###

In these examples, you'll modify or create a record in a data source that's named **IceCream**. The data source begins with the data in this table:

![](media/function-remove-removeif/icecream.png)

| Formula | Description | Result |
|---------|-------------|--------|
| **Remove(&nbsp;IceCream,<br>First(&nbsp;Filter(&nbsp;IceCream,&nbsp;Flavor="Chocolate"&nbsp;)&nbsp;) )** | Removes the "Chocolate" record from the data source. |![](media/function-remove-removeif/icecream-no-chocolate.png)<br><br>The **IceCream** data source has been modified. |
| **Remove(&nbsp;IceCream,<br>First(&nbsp;Filter(&nbsp;IceCream,&nbsp;Flavor="Chocolate"&nbsp;)&nbsp;) First(&nbsp;Filter(&nbsp;IceCream,&nbsp;Flavor="Srawberry"&nbsp;)&nbsp;) )** | Removes two records from the data source. |![](media/function-remove-removeif/icecream-only-vanilla.png)<br><br>The **IceCream** data source has been modified. |
| **RemoveIf(&nbsp;IceCream, Quantity&nbsp;>&nbsp;150 )** | Removes records that have a **Quantity** that is greater than 150. |![](media/function-remove-removeif/icecream-only-chocolate.png)<br><br>The **IceCream** data source has been modified. |
| **RemoveIf(&nbsp;IceCream, Quantity&nbsp;>&nbsp;150, Left(&nbsp;Flavor,&nbsp;1&nbsp;) = "S" )** | Removes records that have a **Quantity** that is greater than 150 and **Flavor** starts with an "S". |![](media/function-remove-removeif/icecream-no-strawberry.png)<br><br><br>The **IceCream** data source has been modified. |
| **RemoveIf(&nbsp;IceCream, true )** | Removes all records from the data source. |![](media/function-remove-removeif/icecream-empty.png)<br><br>The **IceCream** data source has been modified. |

### Step by step ###

1. Import or create a collection named **Inventory**, and show it in a gallery as [Show data in a gallery](show-images-text-gallery-sort-filter.md) describes.

1. In the gallery, set the **OnSelect** property of the image to this expression:<br>**Remove(Inventory, ThisItem)**

1. Press F5, and then click an image in the gallery.<br>The item is removed from the gallery and the collection.