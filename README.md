# Wine Analysis - Tailored Choices for Every Budget using SQL
Data source: Kaggle - [Wine Reviews] (https://www.kaggle.com/datasets/zynicide/wine-reviews?resource=download)

## Background 
Wine is more than just a beverage; it is an experience that embodies the interplay of culture, geography, and craftsmanship. As the global wine industry thrives, consumers are increasingly looking for high-quality wines that provide value for money. This study leverages a dataset of wine reviews to analyze critical aspects of wine quality, pricing, and regional trends. The insights aim to empower consumers to make informed decisions about wine purchases while enhancing their appreciation of wine diversity.

## Objective 
The goal of this study is to provide practical guidance for wine enthusiasts by addressing:
1. Wines offering the best value for money.
2. High-quality wines categorized by price, region, and variety.
3. Regions and varieties that excel in quality-to-price ratios.
4. Characteristics of highly rated wines to inform consumer choices.

## Data Overview
- Price: Retail price of the wine.
- Country: Country of origin.
- Quality (Points): Wine reviewer scores.
- Province: Wine-producing region.
- Variety: Type of grape used to make the wine.
- Designation: Specific vineyard or sub-region within the winery.

## Data Preparation and Cleaning 
- Handling duplicates : there are no duplicates data from this dataset.
- Handling missing value :
  
```
#check the missing values
select count(*) as null_count
from WineAnalysis.wine_data2
where price is null or country is null or points is null;
```

Approximately 6.96% of the data contained missing values for critical variables (price, country, points).
To ensure data integrity, rows with missing values were removed, and the new table created that contained clean data resulting in a cleaned dataset of 129971 rows.
```
 #create cleaned table from wine_data
create table WineAnalysis.cleaned_wine_data as 
select * 
from WineAnalysis.wine_data2 
where price is not null and country is not null and points is not null;
```
``` 
#check null values in cleaned data table
select count(*) as null_count
from WineAnalysis.cleaned_wine_data
where price is null or country is null or points is null;
```


