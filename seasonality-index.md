---
description: seasonality of products in ecommerce business
---

# Seasonality Index

## What is seasonality and why it is important for inventory planning

Seasonality of products plays a crucial role in the e-commerce business. This refers to the variation in demand for a particular product or service over different seasons of the year. For example, demand for swimsuits and beachwear is higher during the summer season, while demand for winter jackets and boots is higher during the winter season.

One way to manage the seasonality of products in ecommerce is by creating a seasonality index of categories. This index can be used to identify the seasonality trends for different product categories and help to plan the inventory accordingly.

To create a seasonality index, ecommerce businesses can collect data on the sales of different product categories over a specific period of time. This data can be used to calculate the average monthly sales for each category and identify seasonal trends.

For example, if the average monthly sales for swimsuits are higher during the months of June to August, it indicates that there is a seasonal trend for this product category. Based on this trend, ecommerce businesses can stock up on swimsuits during the months leading up to the peak season, and reduce the inventory during the off-season.

Seasonality is an important factor for ecommerce companies when inventory planning. However, if a product's category is not detailed enough, it can be difficult to accurately calculate its seasonality index.

For example, if regular leggings and fur leggings are under the same category "Leggings," it is difficult to determine how they behave during different seasons. Regular leggings are more popular in the spring and fall, while fur leggings are more popular in the winter. However, since both types of leggings are grouped together under the same category, it is difficult to accurately determine their seasonality and plan inventory accordingly. I used an auto-classification technique to solve this category problem and created seasonality index data for an ecommerce company.

There are some assumptions I made to develop seasonality index data.&#x20;

* Weather, holidays, marketing anniversaries, and other special days affect the demand for products, causing them to exhibit seasonal behavior.
* Products can be grouped into a limited number of seasons based on their seasonal behavior, such as Spring, Summer, Fall, Winter, Spring\&Fall, Spring\&Fall\&Winter, etc.
* Products have a linear trend in their demand over time (which is probably not true)

## How I created seasonality index data

1. Calculate product level seasonality index (SI). To calculate the product level SI, the following steps can be taken:
   1. Detrend SKU (Stock Keeping Unit) level demand to remove any overall trends and focus on seasonal patterns.
   2. Aggregate detrended SKU level demand data into product groups to make the seasonality index of each product.
2. Classify products into seasons based on their sales patterns, such as Spring, Summer, Fall, and Winter.
   1. Make the product level SI binary by setting a threshold of 0.8, so that products with an SI greater than or equal to 0.8 are considered seasonal.
   2. Compare the binary SI of each product with predefined monthly SI values for each season.
   3. Calculate the error rate by dividing the number of matching months by the total number of months (up to 12. It can be less than 12 if it's a new product)
   4. Map each product to the season with the lowest error rate (final SI bucket)
3. Find valid combinations of category and season using product SI data
   1. Filter out invalid combinations of category and season.
      1. Remove products with less than 13 months of data and those with an error rate greater than 30% to prevent overfitting.
      2. Aggregate the data by category and season to create a category & season mapping table.
         1. Remove if a season's category unit sold penetration is less than 5%.
         2. Remove if a season's annual average unit sold is too low (i.e. annual unit sold sum < 1000 units).
      3. Manually check categories that are known not to have seasonality and exclude them from the mapping table.
4. Map products to their final season bucket using category and season mapping table.
   1. For products with high sales on specific holidays, such as Christmas or New Year's, the following steps can be taken:
      1. Identify products with the demand that is highest on a specific holiday and calculate SI for that holiday.
      2. If the holiday SI is greater than or equal to 150%, the product's SI bucket is set to "{category}-{holiday\_name}"
   2. For other seasonal products, the following steps can be taken:
      1. Make the product level SI binary by setting a threshold of 0.8, so that products with an SI greater than or equal to 0.8 are considered seasonal.
      2. Compare the binary SI of each product with category-level SI values and predefined category & season-level SI values.
      3. Calculate the error rate by dividing the number of matching months by the total number of months (up to 12. It can be less than 12 if it's a new product)
      4. Map each product to the season in the mapping table with the lowest error rate.
      5. Calculate a product & seasonality index ID mapping table using the output above.
5. Using the final SI bucket information, create a seasonality index table with two SI versions.
   1. Version 1 (input for above steps):
      1. Category level SI
      2. Category & season level SI
   2. Version 2 (input for sales forecasts):
      1. Category level SI including products whose final SI bucket is category level (excluding products whose final SI group is category-season)
      2. Category-season level SI includes products whose final SI bucket is category-season
6. Map products to their final SI using PID & SI ID table.&#x20;

| Predefined Season Name | Month                    |
| ---------------------- | ------------------------ |
| SPRING                 | Feb - April              |
| SUMMER                 | May - Aug                |
| FALL                   | Sept - Nov               |
| WINTER                 | Nov - Feb                |
| EVERGREEN              | Jan - Dec                |
| SPRING\&SUMMER         | Feb - Aug                |
| SPRING\&FALL           | Feb - April & Sept - Nov |
| WINTER\&SPRING         | Nov - April              |
| FALL\&WINTER           | Sept - Feb               |
| SPRING\&FALL\&WINTER   | Feb - Aug & Nov - Feb    |
| SPRING\&SUMMER\&FALL   | Feb - Nov                |
| Lunar New Year         |                          |
| New Year               | 1/1                      |
| Christmas              | 12/25                    |
| Valentine's Day        | 2/14                     |
