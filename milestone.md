Real Estate Economic Analysis: Milestone Report

Intro 

Real estate markets continuously evolve in response to the economic climate. To make sound investment decisions in these markets, we must be aware of the major forces at work. Whether we take the point of view of a first-time homebuyer or a professional investor, we need intelligent analysis of the different market components. Given the current state of the market and projected future developments, there will be certain real estate properties which are more desirable than others; our goal is to identify these properties. The definition of ‘desirability’ changes with our investment goals and the point of view we take, but one thing is universal--the need for guidance through analysis. 

For this project, we will build an application in Python to automate different aspects of economic analysis which go into real estate investing. In particular, the project will focus on the Los Angeles county region and have three major components of analysis: growth, value, and liquidity. For our study of growth, we will analyze a tax dataset which contains income bracket counts by zip code throughout California. For our study of value, we will analyze a dataset which contains characteristic information for millions of LA county properties and their assessed values. For our study of liquidity, we will analyze a dataset containing quantity of sales information.  

II. 	Data & Preliminary Analysis

Zip codes will be used as the primary keys in this project, and will be used to relate data from separate datasets. This design choice was made to ensure that we had location-specific data (which is fundamental in real estate analysis) yet also sufficient data points to model with. For a few datasets, information will not be available at the level of specificity of zip code, and data representing a more general region will be used in place. These will be noted.

* Properties of these datasets are explained in detail on their corresponding EDA Jupyter Notebooks.

Growth 
1. California tax returns by zip code: 2004 – 2014 (.xlsx)

For the component of this project which studies growth, we will use a dataset containing counts of IRS tax returns by filers’ income brackets. By studying statistically significant changes in proportions over these brackets, we can identify areas with high potential for growth in demand. 

The data is contained in separate Excel files for each year, and the formatting slightly differs from year to year. We account for this and re-format the information so that we can consolidate the information to a single file, which in turn enables useful analysis. Details can be found in the Notebook, but the major components of cleaning includes: restructuring brackets for consistency, replacing symbolic values with numerical values, and normalization of proportions. Additionally, we create a ‘net growth’ table (Pandas dataframe) which contains values representing the net change in each bracket for each zip code, from 2004 to 2014. 

Using the specially constructed ‘net growth’ table, we can identify regions with big increases in the higher income brackets. At first thought, it may seem straightforward to spot growth opportunities: find regions with the most increase in high income brackets. Although this does find areas with significant growth, it approaches the problem in a black-and-white perspective. A better idea is to study the distribution of changes over each bracket, and assess the growth of a specific region via statistical metrics. The idea is similar, but this is a more informative way of analyzing this data and helps us develop different theories: is this growth level sustainable? What could be the underlying cause of this growth? And so on. 

The different brackets are identified by zip codes from California (i.e. for a given zip code, there will be a row for each bracket). We choose to study the brackets which represent potential participants in the real estate market--this turns out to be five brackets. Although this is enough of a differentiation to do analysis, we are limited in analyzing changes over this discrete domain. With specific income data from every individual, we would be able to more precisely analyze changing density in wealth distribution. Nonetheless, our tax dataset enables us to effectively study general trends in the market.

Another important caveat: the dataset contains information from 2004 to 2014. At the time that this project is being put together, there is no publicly available data for more recent years. Thus, our analysis of this dataset will guide us, but it is imperative that we also study more recent (2016; 2017) trends together with this analysis to gain a better understanding of the market. Analysis of this tax dataset is to gain a better “big-picture” perspective of the market. 

Pricing & Valuation
1. Properties LA County: 2015 (.xlsx)

	For the component of this project which studies value, we will use property information and the ‘comparable real estate’ approach for analysis. That is, we will create models out of data points (properties) with characteristics similar to the input, and use historical valuations to predict value. This dataset contains zip codes, which will be used to combine with the other components of analysis. The columns in this dataset separate into two main subsets: categorical information and feature information. (Details on the columns can be found in the EDA Notebook.)
	
	The original dataset is very substantial and we are able to reduce it with strict requirements while still obtaining sufficient data points. So, we take care to thoroughly filter the data such that the result requires minimal cleaning. We only keep columns which have at least 40% of their rows filled, and drop all rows with NaN values. This leaves us with a dataset of 1.6 million properties in LA county. For a given zip code, we are left with an average of about 6000 properties to model with.

Zip codes are not explicitly listed in this dataset--Zillow uses an ID to identify regions, and these need to be mapped to their actual zip codes. Finally, categorical columns contain numerical values representing categories--these will be left alone. In the class definition, we will automate a mapping using a data dictionary so that the client can input data in a human-readable format.

Categorical information will be used to filter the main dataset into a training set for our models. The main dataset does not have uniformly distributed data across categories, so these filters will be applied only to the extent that we are left with sufficient training data to model with. At the present iteration of this project, filters are applied so as to not reduce the training set below a threshold value. This threshold value is chosen as a reasonable value (albeit somewhat arbitrary). In a future update, a clustering algorithm will be used to create classes of properties within a zip code region, and a model will be generated such that the training size minimizes test error. Thus, we will circumvent the need to explicitly establish a threshold value. 

Feature information will be used in regression (i.e. the actual prediction) after categorical ‘pre-processing’ phase. In the current iteration of the project, this is done by choosing a threshold minimal absolute value for the correlation between a feature and the assessed value we are trying to predict. This is a valid approach--we consequently model the property with features which have a statistically significant relationship. However, the threshold value is again rather arbitrarily chosen. In a feature update, an automated process will remove the features iteratively such that we end with the features that minimize test error. 

2. RegionID to Zip Code Dictionary (.xlsx) (mini data set, will be described in more detail in full report)
This dataset is used to pre-process the properties dataset. In particular, the properties dataset contains identifiers used by Zillow which are not directly meaningful. We must map these region identifiers to their corresponding zip codes using this dictionary. 

3. ZHVI Growth (.xlsx) (mini data set, will be described in more detail in full report)
This contains growth measurements for zip codes in LA county from 2015 to 2017, based on Zillow’s ZHVI metric. These quantities are used in the final calculation for suggesting a sale price for today.

4. Sales Medians LA County (.xlsx) (mini data set, will be described in more detail in full report)
This contains median sale prices of real estate properties in various California regions. This is used in our models to calculate suggested sale prices.

5. Data Dictionary (Properties in LA County) (.xlsx)
This contains value mappings for categorical features in the properties dataset. (The properties dataset uses integers to represent categories: 2 = Central Heating, 261 = Single Residential Home, etc.)

Liquidity (TBA)
3. LA County Sales: 2017

By studying sales and measuring levels against averages, we can come to a conclusion about the liquidity of the property in question. (TO BE CONTINUED)



III. 	LOOKING AHEAD

	With the current draft of this project, a major limitation is the interjection of human-made decisions in both designing the entire system as well as optimizing the learning algorithms, i.e. the current method uses heuristics which reduce the reliability of the system. As we look forward to improving the application, the first priority seems to be to automate and optimize these decisions. (We want this project to be reminiscent of a product that could be used in industry, i.e. minimize the need for human intervention in any part of the system.)

Additionally, the question still remains of how exactly we will combine the analysis from the growth, value, and liquidity components into an overarching conclusion about the input property (as well as define what exactly the “output” is). This can be done in a variety of ways, and the specific approach  chosen will depend on the client using the application.

On one hand, we can allow for significant human interpretation. On this end of the spectrum, we suggest approaches to interpret the entire analysis on a case-by-case basis. The output of the application would be a set of statistical metrics, as well as information about the reliability of the models. This approach is more appropriate for the client who plans to use this software as a guide for human decision making, rather than an end-all prediction. In other words, what is done with the output of this system is not automated--it is a human receiving the analysis. 

On the other hand, we can design the system to be implemented within a larger, automated system. In this case, we define statistical metrics derived from the models and combine them with those from other components, while using a mathematically valid model. (We are able to do this because we have zip codes as keys to directly relate analysis between the data sets. In reaching this point of the project, it was a lesson hard-learned that a hypothetical model to combine analysis between datasets can only be effectively done if there is a means to directly relate the information.) The outputs can vary depending on the context. For example, it could be a simple buy/sell trading signal, or a complex combination of outputs that are input in different respective functions.

Thus, we have seen that the direction of this project can take different routes. It is a matter of context: the “bots working with humans” paradigm against the “bots working with bots” paradigm.
