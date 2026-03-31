# Exploring House Prices in Ames, Iowa
**Mazen Abdullah Binafif | ML Foundations Capstone | Class 1/1**

## 1. Introduction
For this project, I used the Ames Housing dataset, which contains 2,930 residential property sales in Ames, Iowa. The dataset includes more than 80 features about the houses, such as size, quality, neighborhood, and final sale price. My goal was to answer three simple questions: (1) What makes a house more expensive in Ames? (2) Which features are most strongly connected to sale price? (3) Are there any interesting patterns hidden in the data?

## 2. Cleaning Summary
Before starting the analysis, I cleaned the dataset to remove the biggest problems. A few columns had too many missing values, so I dropped them because filling so many blanks would not be very reliable. These included Pool_QC, Misc_Feature, Alley, Fence, and Fireplace_Qu. For important numeric columns such as Lot_Frontage, Mas_Vnr_Area, Total_Bsmt_SF, and Garage_Area, I filled missing values with the median. For text columns, I used "None" when a missing value usually meant that feature was not present.

I also fixed one data type issue. The column MS_SubClass was stored as a number, but it really works like a category, so I converted it to text. I checked for duplicates, but there were no full-row duplicates in the data. To handle outliers, I used a boxplot on SalePrice and capped the very highest values at the 99th percentile, which was about 456,183. I thought this was better than deleting those rows because the houses were real sales, just unusually expensive.

At the end, I put the steps inside a `clean_data()` function and added simple checks to make sure SalePrice had no missing values, all prices were positive, and the cleaned dataset had the expected number of columns.

## 3. Feature Engineering Summary
After cleaning the data, I created a few new features to make the analysis more useful. First, I created `price_per_sqft` by dividing SalePrice by Gr_Liv_Area. This helps compare houses of different sizes in a fairer way. Second, I created `total_bathrooms` by combining full and half bathrooms into one number. Third, I created `quality_x_area`, which multiplies Overall_Qual by Gr_Liv_Area. I liked this feature because it combines two important ideas: size and quality.

I also ordinally encoded Exter_Qual using a simple order from poor to excellent, one-hot encoded Neighborhood and MS_Zoning, scaled Gr_Liv_Area and Total_Bsmt_SF, and log-transformed SalePrice using `np.log1p()` because the price distribution was right-skewed. Finally, I removed a few features that were almost duplicates of each other based on very high correlation.

## 4. Key Findings
**Finding 1 - Overall quality is the strongest predictor of price.**  
The correlation heatmap showed that Overall_Qual has one of the strongest relationships with SalePrice, with a correlation of about 0.81. The grouped boxplot also showed a clear pattern: houses with higher overall quality usually sell for much more.

**Finding 2 - Living area matters a lot, but not perfectly.**  
The scatter plot between Gr_Liv_Area and SalePrice showed a strong positive trend. In general, bigger homes cost more. However, for some very large houses, the price does not rise at exactly the same rate, so size is important but not the only factor.

**Finding 3 - Neighborhood has a major effect on average price.**  
The groupby summary showed large price differences between neighborhoods. For example, NoRidge and NridgHt had some of the highest average prices, while MeadowV was one of the lowest. This tells me that location is a major part of housing value in Ames.

**Figure 1: Boxplot of SalePrice by Overall Quality**

## 5. What I Would Do Next
If I had more time, I would build a simple machine learning model to predict house prices using the cleaned and engineered features. I would also test whether quality has a different effect in different neighborhoods. Finally, I would explore time trends to see whether sale prices changed from year to year.
