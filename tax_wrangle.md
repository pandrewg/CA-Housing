## Data Wrangling for California Tax Returns: 2007-2014

*This dataset consists of 8 separate excel files containing tax return information by zipcode and income bracket. The formatting and listed information varies slightly from year to year; we will extract relevant data that is available across all the files, aggregate the data, and reformat the tables within Pandas dataframes so information is consistent between all the years. Lastly, we will perform some miscellaneous cleaning so the tables do not contain missing values.*

**1. Header rows to skip**

Each file contains a few header rows containing information about the dataset, but which do not actually contain data we are interested in. (These files are not formatted for efficient computer analysis, they are formatted for human perusal.) We must manually inspect and note the header rows to skip for each file.
  
**2. Column labels**

The files contain between 30-70 columns (each file is different) for various different pieces of information included when CA residents fill out their tax returns. For the purposes of this analysis we are only interested in two columns: number tax returns and number of joint tax returns.These two columns are consistently listed across all the files; we will extract those columns for each year and create a separate dataframe for each file. Columns are not descriptively labeled in the files, so we will also rename the columns.

**3. Aggregate**

We will use outer joins to aggregate the information from different years into a single table to avoid losing information about any zipcodes/income brackets. For years 2007 and 2008, the files contain income brackets "under 10,000" and "10,000 - 25,000." Year 2009 onwards, there is a single bracket "under 25,000." (i.e. they changed the formatting of the tables beginning in 2009.) Since we used outer joins, the "under 25,000" bracket is added to years 2007 and 2008 with NaN values. We will sum the "under 10,000" and "10,000-25,000" values and set them to the "under 25,000" row for each corresponding zipcode. 

**4. Renaming income brackets**

We will rename income brackets so they are properly ordered from lowest to greatest within each zipcode. This will help make it easier to understand the data when doing exploratory analysis down the line. 

**5. Indices**

We will set zipcode and income brackets as indices for the dataframe. This will make it easier to do analysis across different regoins in CA using the .loc[] function. 

**6. NaN values**

  * There are zipcodes which are only listed for a few years. They contain many NaN values from when we performed the outer join, and do not contain enough useful information for us to conduct analysis on. We will not retain these rows.
  
  * The "under 10,000" and "10,000-25,000" rows contain NaN values for years 2009 onwards. Since we already reformatted the table to move information from those rows into the "under 25,000" row, we will remove these rows.
  
There are no other NaN values in the dataset: the zipcodes and income brackets which are included in all the years have information filled across all the columns. Thus, a dropna() operation is performed to removed the above-mentioned rows.