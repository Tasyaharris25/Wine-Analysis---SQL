# Wine Analysis - Tailored Choices for Every Budget using SQL
Data source: Kaggle - [Wine Reviews](https://www.kaggle.com/datasets/zynicide/wine-reviews?resource=download)

## Background 
Wine is more than a beverage; it is an experience that reflects culture, geography, and craftsmanship. As the global wine industry grows, consumers seek high-quality wines that deliver value for money. This study uses wine reviews to analyze critical aspects of wine quality, pricing, and regional trends, empowering consumers to make informed wine purchases and appreciate wine diversity.

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
<img width="137" alt="image" src="https://github.com/user-attachments/assets/6e8a0eb5-2b89-4ba0-bdf5-fe730485fc32" />

Approximately 6.96% of the data contained missing values for critical variables (price, province, points).
To ensure data integrity, rows with missing values were removed, and the new table created that contained clean data resulting in a cleaned dataset of 129971 rows.
```
 #create cleaned table from wine_data
create table WineAnalysis.cleaned_wine_data as 
select * 
from WineAnalysis.wine_data2 
where price is not null and country is not null and points is not null;
```
## Analysis 
### Price vs Quality Analysis 
- Affordable high-rated wine
```
#identify high-rated wine with affordable price
select price, points
from WineAnalysis.cleaned_wine_data
where price < 20 and points >= 90
order by points desc, price asc;
```
<img width="659" alt="image" src="https://github.com/user-attachments/assets/a7863584-4d94-4fa9-8c90-4a9a1ca5f9dc" />
Wines priced under $20 with scores of 90+ were identified. These represent high-quality options for budget-conscious consumers.

- Best value by provinces
```
select province, variety, round(avg(points / price),2) as avg_points_per_dollar
from WineAnalysis.cleaned_wine_data
where price > 0
group by province, variety
order by avg(points / price) desc
limit 10;
```
<img width="661" alt="image" src="https://github.com/user-attachments/assets/0bb4255e-2869-4f51-8da8-bcaa65ffc7b6" />
Based on this dataset, these are the top 10 provinces with the highest average points, offering the best value for money. Provinces offering the highest average points-per-dollar include Idaho and Ville Tumusulli. For consumers, it is valuable to identify provinces and varieties where they can discover the highest quality wines at the lowest cost.

### High Quality Wine 
- Most Common High-Rated Varieties
```
#analyze common characteristic
#identify most common varieties among high-rated wines (top 10 varieties)
select variety, count(*) as count_high_rated
from WineAnalysis.cleaned_wine_data
where points >= 90
group by variety
order by count_high_rated desc
limit 10;
```
<img width="672" alt="image" src="https://github.com/user-attachments/assets/2152c16d-e3e1-47bb-9353-f171d53a6c8e" />
The result of analysis revealed the most frequently high-rated grape varieties, giving consumers an idea of dependable options. Popular varieties like Pinot Noir, Chardonnay, and Cabernet Sauvignon dominate the list of highly rated wines.

- Price ranges of high-rated wines
```
#analyze average price of high-rated wine
select variety, round(avg(price),2) as avg_price, round(avg(points),2) as avg_points
from WineAnalysis.cleaned_wine_data
where points >= 90
group by variety
order by avg_price desc;
```
<img width="666" alt="image" src="https://github.com/user-attachments/assets/43ffd9fb-b99b-4c95-95a4-05200eb5a5b1" />
The above result shows typical price ranges of high-rated varieties, offering guidance for consumers to understand what to expect for their budget when choosing premium wines.

- Top provinces for high-rated wines
```
#high rated wine by province
select province, count(*) as count_high_rated, round(avg(price),2) as avg_price
from WineAnalysis.cleaned_wine_data
where points >= 90
group by province
order by count_high_rated desc
limit 10;
```
<img width="376" alt="image" src="https://github.com/user-attachments/assets/99c7090c-7074-4bb6-a1a8-92519c104943" />
Regions such as California, Washington, and Oregon produce the highest number of highly rated wines, ensuring consistency in quality.

- Impact of designation on quality
```
#relationship between designation and points
select designation, round(avg(points),2) as avg_points, count(*) as count_high_rated
from WineAnalysis.cleaned_wine_data
where points >=90
group by designation
order by avg_points desc
limit 10;
```
<img width="664" alt="image" src="https://github.com/user-attachments/assets/081b68a7-166d-483d-87d7-3fdd3b817402" />
Specific designations like Barca Velha and Cerretalto are associated with top-rated wines, reflecting adherence to high-quality standards. This can provide insights for wine enthusiasts on which designations to seek for high-rated wines and helps them understand the value of a designation when selecting wines.

### Variety Performance Trends
This analysis explores how different wine varieties perform in terms of price and quality, helping uncover insights about consumer value, market positioning, and production strengths.
- Most expensive varieties
```
#identify most expensive varieties
select variety, round(avg(price),2) as avg_price, round(avg(points),2) as avg_points
from WineAnalysis.cleaned_wine_data
group by variety
order by avg_price desc
limit 10;
```
<img width="662" alt="image" src="https://github.com/user-attachments/assets/43d38367-5cf7-4b89-b743-5b5307abe4c8" />
These are the names for premium wine options, such as Ramisco, for customers who are seeking luxurious wine experiences.

- Underrated varieties
```
#underrated varieties
select variety, round(avg(price),2) as avg_price, round(avg(points),2) as avg_points
from WineAnalysis.cleaned_wine_data
group by variety
having avg(points) >= 90 and avg(price) < 20
order by avg_points desc;
```
<img width="379" alt="image" src="https://github.com/user-attachments/assets/4bcc5053-8b31-42fe-a2dc-91545e8ebd7b" />
Grape varieties with scores of 90+ and average prices under $20 were highlighted as excellent yet less-recognized options. Consumers can find these varieties to experience high-rated wine with affordable prices. Also, these provide outstanding value for consumers looking to discover hidden gems.

### Wine Profiles by Province:
- Common varieties by province
```
#most common varieties by province
select province, variety, count(*) as count
from `WineAnalysis.cleaned_wine_data`
group by province, variety
order by province, count desc;
```
<img width="667" alt="image" src="https://github.com/user-attachments/assets/490b2b68-04a6-4221-9d55-48863d2baab7" />
The most popular grape varieties in each province were identified, offering insights into local specialties and helping consumers explore regional characteristics.

- Average points and prices by prince
```
#Average Points and Prices by province
#Compare provinces based on average quality and price.
select province, round(avg(price),2) as avg_price, round(avg(points),2) as avg_points
from WineAnalysis.cleaned_wine_data
group by province
order by avg_points desc, avg_price asc;
```
<img width="667" alt="image" src="https://github.com/user-attachments/assets/71cd75fd-9fc5-4b4c-8c46-22d708d7c54a" />
Analysis of provinces highlights regions delivering high-quality wines at reasonable prices, guiding consumers to regions that align with their preferences and budgets.

- High-performing designations
```
#high performing designation by province
select province, designation, round(avg(points),2) as avg_points, count(*) as count
from WineAnalysis.cleaned_wine_data
group by province, designation
ORDER BY avg_points DESC;
```
<img width="662" alt="image" src="https://github.com/user-attachments/assets/2a8e96f3-2e7e-4532-a8c5-4bf51abb9e50" />
Above listed the reputable designations based on the province. Seeking wines with reputable designations ensures a higher likelihood of quality.

## Insights and Recommendations
1. Best Value: Wines under $20 with 90+ scores represent excellent choices for consumers who want quality at an affordable price. Provinces with the best points-per-dollar ratios should be a priority for value-seeking enthusiasts. For example, Idaho, Ville Tumusulli, and America. 
2. High-Rated Wines: Look for popular high-rated varieties and provinces with a strong reputation for quality like in California, Washington, and Oregon. Certain designations guarantee higher standards and are worth seeking out such as Barca Velha, Cristal Vintage Brut, and Cerretalto with the highest rate which is 100. 
3. Hidden Gems: Underrated varieties, offering high scores at reasonable prices, are perfect for adventurous consumers looking to explore beyond mainstream options.
4. Regional Specialties: Provinces excel in unique grape varieties and designations. Exploring these regions can enhance your wine experience and help you discover distinctive flavors.





