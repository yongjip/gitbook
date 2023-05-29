# Measuring Demand in Ecommerce

It's important to regularly monitor demand in an ecommerce business to ensure that there is sufficient inventory to meet customer needs and avoid out-of-stock situations. To measure demand, busineeses can use hourly inventory snapshot data and sales data. The sales data typically show the number of units sold during each hour, while the inventory data show the inventory level at the end of each hour.

To calculate the OOS rate for each hour, one can check the orderable inventory level at the end of the hour. If the orderable inventory is 0, it means that the SKU was OOS during that hour. However, there may be situations where the inventory is 0 but there are still sales during that hour. In this case, the OOS situation must be treated differently, for example by assuming that 50% of the sales during that hour were lost due to OOS.

To assign a sales weight to each hour, one can divide the total number of unit sold during that hour for the last X days by the total number of unit sold during the same period. This will give a percentage that represents the proportion of total sales that occurred during that hour. The daily weighted OOS rate is then calculated by summing the hourly OOS rate times the sales weight for each hour. This can be used to estimate demand and calculate lost sales.

Once the weighted OOS rate have been calculated, one can use this data to estimate demand by dividing the total number of unit sold during the day by the instock rate during the day (instock rate = 100% - weighted OOS rate). The number of lost sales can then be calculated by subtrating the units sold from the estimated demand.

In summary, demand in an ecommerce businees can be measured by using hourly inventory and sales data to calculate the OOS rates and sales weights for each hour, and then using this information to estimate demand and calculate lost sales. By regularly monitoring these metrics, businesses can ensure that they have sufficient inventory to meet customer needs and avoid OOS.

