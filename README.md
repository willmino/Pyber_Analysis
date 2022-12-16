# Pyber_Analysis
## Overview

After landing a job at Pyber, a Python-based ride-sharing company, Omar and I were tasked with generating some import summary figures for ride-sharing data from 2019. We want to impress the CEO V. Isualize by ensuring our analytical work is comprehensive and correct. 

## Purpose

We are tasked with generating summary figures for the ride-sharing data in pandas dataframes format using the matplotlib library for impressive data visualizations. First, we need to generate dataframes for each city type within our data (Rural, Suburban, and Urban). Then, we need to retrieve summary statistics from these data, such as the total number of rides, number of drivers, and sum of the fares from each city type. We will then perform calculations to generate relevant statistics such as the average fare per ride for each city type and the average fare per driver for each city type. Further data structuring in dataframes and indexing allowed us to generate the code using the matplotlib library to visualize the data.

## Analysis
First, our pandas library (as pd) code `city_data_df = pd.read_csv("Resources/city_data.csv")` and `ride_data_df = pd.read_csv("Resources/city_data.csv") ` was simultaneously used to open the necessary csv files and load them into their own pandas dataframes. The dataframes were subsequently merged into a single dataframe by the code:

 `pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])`.

 We generated statistics for the count of all rides per city type by grouping the `pyber_data_df` dataframe by `"type"`, performing a `count()` aggregation function, and designating to return only the `ride_id` column. This gave us the variable `rides_per_city_type`. This code was summarized as:

 `rides_per_city_type = pyber_data_df.groupby(["type"]).count()["ride_id"]`.

 We obtained total drivers per city type by using the same method as above except for applying the `.sum()` aggregation function and designating the `"driver_count"` column to be returned. This yielded the `drivers_per_city_type` variable. The code was summarized as:

 `drivers_per_city_type = city_data_df.groupby(["type"]).sum()["driver_count"]`

Another statistic, `fares_per_city_type` was generated in a similar way as above, but we instead designated the `"fare"` column to be returned. This gave us the total fare per city type:

`fares_per_city_type = pyber_data_df.groupby(["type"]).sum()["fare"]`

Calculations were required to generate the the `average_fare_per_ride` and the `average_fare_per_driver` statistics for our summary table. The average fare per ride was calculated by dividing the `fares_per_city_type` series by the `rides_per_city_type` series, then multiplying this value by 100. In the same way, we `fares_per_city_type` series and divided it by the `drivers_per_city_type` figure, multiplying by 100 to get the `average_fare_per_driver` statistics. The below code was used to accomplish this:

`average_fare_per_ride = fares_per_city_type/rides_per_city_type`

`average_fare_per_driver = fares_per_city_type/drivers_per_city_type`.

We then took each of the series we generated and converted them to a dataframe using a general format. The dataframe conversion for the `average_fare_per_driver` series was performed by the code: `fare_per_driver_df = average_fare_per_driver.to_frame()`. 
Our first key figure, Deliverable #1, was a summary table to include all of the statistics we generated.
### Deliverable #1
To make this first deliverable, we first set the summary dataframe equal to the first column: `pyber_summary_df = riders_df`. We then created a list of all the subsequent columns to add to the summary dataframe. 
`dfs = [drivers_df, fares_df, fare_per_ride_df, fare_per_driver_df]`
We used a for loop to iterate through every column to be added to the summary dataframe. These "columns" were already converted to individual dataframes, so we could use the merge function to combine these dataframes together into our summary dataframe.

`for i in dfs:`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pyber_summary_df = pd.merge(pyber_summary_df,i, how = "left", on = "type")`

The columns were renamed to match the desired output for our deliverable summary table

`pyber_summary_df = pyber_summary_df.rename(columns={`

`"ride_id":"Total Rides", "driver_count":"Total Drivers","fare":"Total Fares",`

`"0_x":"Average Fare per Ride","0_y":"Average Fare perDriver"})`

Finally, the designated dataframe columns with fare values were formatted with `$` signs.

`columnlist = ["Total Fares","Average Fare per Ride","Average Fare per Driver"]`

`for i in columnlist:`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pyber_summary_df[i] = pyber_summary_df[i].map('${:,.2f}'.format)`

`pyber_summary_df`

Our first deliverable was then displayed in our jupyter notebook file as below.

![Deliverable #1 Summary table](https://github.com/willmino/Pyber_Analysis/blob/main/Deliverable_1.png)





## Results




## Summary

