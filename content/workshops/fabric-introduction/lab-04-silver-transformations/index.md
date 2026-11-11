+++
title = "Lab 4: Silver Transformations with Data Wrangler"
weight = 40
+++

## Introduction

In the previous lab, you copied the products API response into your lakehouse as `Files/products.json`. In this lab, you’ll use a Fabric notebook to explore and clean that JSON data, then save the refined result as a Delta table named `products`.

This is the **Silver layer** of our medallion architecture: data is refined from its original Bronze form so it is easier to use in later analysis. You’ll use Spark to read and write the data, and Data Wrangler to create cleaning steps without writing them all by hand.

## Create a Notebook

Create a notebook from the **Get data** task in your workspace.

> [!NOTE]Instructions
> 1. Open the workspace and view the task flow you created in Lab 1.
> 1. On the **Get data** task, select **New item**.
> 1. Select **Notebook**.
> 1. Name it `Products_Silver_Notebook`
> 1. Select **Create**.

![Screenshots of the above instructions](<create notebook.png>)

## Organise the Notebook

Notebook cells can contain executable code or Markdown. Add Markdown cells to label the notebook and its steps and build the table of contents navigation. This is best practice and should be part of your standards.

> [!NOTE]Instructions
> 1. In the first cell, change the text to `# Load products JSON into a table`
> 1. Then convert this cell to a Markdown cell by clicking the M with a down arrow button.
> 1. Add a new markdown cell by hovering your mouse below the new title and selecting + Markdown
> 1. Enter in a heading 2 `## Open File`.
> 1. From the **View** ribbon, select **Table of contents** to get the navigation pane.

![Screenshots of the above instructions](<Organise notebook.png>)

Markdown is a lightweight markup language that uses # ## etc for headings. See references for a guide to the syntax.

## Add the Lakehouse

Attach your lakehouse to the notebook so it can read the JSON file and write the output table.

> [!NOTE]Instructions
> 1. In the notebook, select **Add data items**.
> 1. Choose **OneLake catalog**.
> 1. Find and select your workshop lakehouse, then select **Add**.
> 1. Lakehouse appears in the Explorer pane.

![Screenshots of the above instructions](<attach lakehouse.png>)

## Open JSON file

>[!NOTE]Instructions
> 1. Expand the lakehouse in the notebook's data pane. 
> 1. Click on **Files** and a pane expands to show the files.
> 1. On `products.json`, select the **...** menu, then choose **Load data** > **Spark**.
> 1. Click the run cell button to execute the code.

![Screenshots of the above instructions](<Open File.png>)

Fabric adds a code cell that reads the JSON file and displays it as a table. The first run can take a few minutes while Fabric starts the processing service. The final line ```display(df)`` will put a table of the data in the output

## Why Spark Instead of pandas?

Both **Spark** and **pandas** let us work with data in rows and columns, but they are designed for slightly different jobs.

- **Spark** is designed for processing large amounts of data. It can spread the work across multiple computers and is widely used in Fabric for working with data in a lakehouse.
- **pandas** is designed for working with smaller amounts of data on a single computer. It is often simpler and very useful for exploring and analysing data.

For this exercise, our Products file is small enough that either could work. We are using Spark because it is easier to write to the lakehouse using Spark. It also means that the same approach can continue to work as our data gets larger.

## Clean the Products Data using Data Wrangler

In the output of the cell run in the previous steps, take a look at the displayed DataFrame. The source data has a `Product ID` column with a space in its name and a `Base Price` column containing prices as text with a pound sign. We’ll rename the ID column, remove the currency symbol, convert the price to a decimal, and remove the original text column. These are all transformations that Data Wrangler can help us write the code for.

### Fix Product ID

> [!NOTE]Instructions
> 1. In the output above the code cell, select **Data Wrangler**. Wait for the preview to load.
> 1. Open the **...** menu for `Product ID` and select **Rename column**.
> 1. Enter `Product_ID` into **New column name** in the Operations pane on the left
> 1. Check the coloured columns, red is the before and green the result
> 1. Click Apply to keep the green column
> 1. Check the Cleaning steps in the bottom left to see Rename column has been added

![Screenshots of the above instructions](<Data Wrangler1.png>)

You can only edit or delete the last step in Cleaning steps. You can click on them to view the changes on each step

### Fix Base Price Column

The first fix is to remove the £ from the Base Price. There are multiple ways to do this, we are going to use a feature worth knowing about called New Column by Example.

> [!NOTE]Instructions
> 1. In the Operations pane, choose **New column by example**.
> 1. For the Columns to derive from select **Base Price**
> 1. In the derived column name enter `Base_Price`. 
> 1. In the data table, a new column appears coloured green and with the name **Base_Price**
> 1. In the first row of this new column, enter the numeric value without the pound sign (for example, enter `2.5` for `£2.5`), then press **Enter**.
> 1. Wait a few moments for the column to fill, then check that the preview derives the numeric values correctly.
> 1. Check the code in the code pane to see it has worked out a replace function does the job
> 1. Click Apply to create a new cleaning step.

![Screenshots of the above instructions](<Data Wrangler2.png>)

### Final Cleaning Steps

The new Base_Price column is still text and the old Base Price column is no longer needed.

> [!NOTE]Instructions
> 1. On the new `base_price` column, open the **...** menu and choose **Change column type**. 
> 1. In the Operations pane, select **float32** for New type.
> 1. Click Apply.
> 1. Open the **...** menu for the original `Base Price` column, choose **Drop columns**
> 1. After checking the right column is highlighted red, select Apply.

![Screenshots of the above instructions](<Data Wrangler3.png>)

The data should now have the right column names, data types and data content. It should look like this.

![Table of data with 5 columns](<data fixed.png>)

### Load Code into the Notebook

> [!NOTE]Instructions
> 1. Select **Add code to notebook** from the top ribbon. 
> 1. In the Add code to notebook dialog, choose **Add**
> 1. A code block appears containing a function called clean_data to do the transformation steps.
> 1. Add a markdown block above it with the contents ```## Clean up data```

![Screenshots of the above instructions](<add code.png>)

## Exploring the Data Wrangler Code

Data Wrangler works in pandas code and then translates into Spark because we had a Spark DataFrame. It writes a function and then executes that function on the DataFrame we created in the first code block. Your code might be slightly different but using the image as a guide, we can break down the code as follows:

- Line 1 is a comment
- Lines 3 & 4 import libraries needed later in the code. 
- Lines 6 - 19 defines a function called clean_data that takes a DataFrame and returns a DataFrame.
- Line 21 executes the function passing in the DataFrame ```df``` and saves the result in ```df_clean```
- Line 22 displays the ```df_clean``` DataFrame.

Run the code and check the output.

![The code and the output from running the code](<data wrangler run.png>)

## Select and Order the Columns

The columns are in reverse order compared to the JSON file with the new Base_Price added on the end. This will be the same in the table if we just save the table. So the next task is to fix the order.

Add a markdown cell with the content ```## Sort Columns```

Add a code cell after the cleaning cell to give the output a consistent column order:

```python
df_clean = df_clean.select(
    "Product_ID",
    "Product",
    "Category",
    "Base_Price",
    "Active"
)

display(df_clean)
```

Run the cell and confirm that the displayed DataFrame contains the five columns in that order.

![ Table with reordered columns to match the code above](<right order.png>)

## Write the Silver Table

Add a Markdown cell with the heading `## Write data to table`, then add and run this code cell:

```python
df_clean.write.format("delta").mode("overwrite").option("overwriteSchema", "true").save("Tables/dbo/products")
```

This writes the cleaned DataFrame as a Delta table in the attached lakehouse. The overwrite mode replaces the table if you rerun the notebook, rather than appending duplicate rows. The overwriteSchema will allow you to make column name or datatype changes.

Open the lakehouse and refresh **Tables** if needed. Open the `products` table and check that the cleaned columns and values are present.

## Fixing Base_Price



## Lab 4 Complete

You should now have a `Products Silver Notebook` in your workspace and a `products` Delta table in your lakehouse. The table contains the cleaned product data, with a decimal `base_price` and columns in the intended order.

## Useful References

- [Markdown Basic Syntax](https://www.markdownguide.org/basic-syntax/)
- [Microsoft Learn - Explore and transform Spark data with Data Wrangler](https://learn.microsoft.com/en-us/fabric/data-science/data-wrangler-spark?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - Data Wrangler for pandas DataFrames](https://learn.microsoft.com/en-us/fabric/data-science/data-wrangler?wt.mc_id=DX-MVP-5003563)
- [Microsoft Learn - Apache Spark in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-overview?wt.mc_id=DX-MVP-5003563)
