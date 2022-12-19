# Pyber_Analysis
## Overview

After landing a job at Pyber, a Python-based ride-sharing company, Omar and I were tasked with generating some import summary figures for ride-sharing data from 2019. We wanted to impress the CEO, V. Isualize, by ensuring our analytical work was comprehensive and correct. 

## Purpose

We were tasked with generating summary figures for the ride-sharing data in pandas dataframes format using the matplotlib library for impressive data visualizations. First, we needed to generate dataframes for each city type within our data (Rural, Suburban, and Urban). Then, we needed to retrieve summary statistics from these data, such as the total number of rides, number of drivers, and sum of the fares from each city type. We then performed calculations to generate relevant statistics such as the average fare per ride for each city type and the average fare per driver for each city type. Further data structuring in dataframes and indexing allowed us to generate the code using the matplotlib library to visualize the data.

## Analysis
First, our pandas library (as pd) code `city_data_df = pd.read_csv("Resources/city_data.csv")` and `ride_data_df = pd.read_csv("Resources/city_data.csv") ` was used to simultaneously open the necessary csv files and load them into their own pandas dataframes. The dataframes were subsequently merged into a single dataframe by the code:

 `pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])`.

 We generated a statistic for the count of all rides per city type by grouping the `pyber_data_df` dataframe by `"type"`, performing a `count()` aggregation function, and designating to return only the `ride_id` column. This gave us the variable `rides_per_city_type`. This code was summarized as:

 `rides_per_city_type = pyber_data_df.groupby(["type"]).count()["ride_id"]`.

 We obtained total drivers per city type by using the same method as above except for applying the `.sum()` aggregation function and designating the `"driver_count"` column to be returned. This yielded the `drivers_per_city_type` variable. The code was summarized as:

 `drivers_per_city_type = city_data_df.groupby(["type"]).sum()["driver_count"]`

Another statistic, `fares_per_city_type` was generated in a similar way as above, but we instead designated the `"fare"` column to be returned. This gave us the total fare per city type:

`fares_per_city_type = pyber_data_df.groupby(["type"]).sum()["fare"]`

Calculations were required to generate the the `average_fare_per_ride` and the `average_fare_per_driver` statistics for our summary table. The average fare per ride was calculated by dividing the `fares_per_city_type` series by the `rides_per_city_type` series, then multiplying this value by 100. In the same way, we `fares_per_city_type` series and divided it by the `drivers_per_city_type` figure, multiplying by 100 to get the `average_fare_per_driver` statistics. The below code was used to accomplish this:

`average_fare_per_ride = fares_per_city_type/rides_per_city_type`

`average_fare_per_driver = fares_per_city_type/drivers_per_city_type`.

We then took each of the series we generated and converted them to a dataframe using a general format. The dataframe conversion for the `average_fare_per_driver` series was performed by the code: `fare_per_driver_df = average_fare_per_driver.to_frame()`. 
Our first key figure, Deliverable #1, was a summary table to include all of the statistics we generated. These two figures were paramount in helping us carry out our analysis to observe the discepancies in ride-sharing data among varying types of cities. We would soon begin to see how the economic environment and geographic qualities of the differing city types could influence these figures.
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

Our first deliverable was then displayed in our jupyter notebook file.

### Deliverable #2 

The second deliverable in our report for V. Isualize was a multiple line plot. We wanted this line plot to exhibit the total ride-sharing fares per week for each type of city. 

We grouped the the merged pyber_data_df dataframe by "type" and "date". We performed the sum() aggregation function on this grouped dataframe and displayed only the "fare" column. We set this equal to the fare_by_date grouped dataframe. This was achieved with the code: 

`fare_by_date = pyber_data_df.groupby(["type","date"]).sum()["fare"]`

We then reset the index for this new grouped dataframe by running the code

`fare_by_date = fare_by_date.reset_index()`. 

This primed the data for compatibility with the pivot() function.

We then set the new variable for our pivot table `fare_by_pivot_table` equal to the `fare_by_date` dataframe with the `.pivot()` function applied using "date" for the index parameter, "type" for the columns parameter, and "fare" for the values parameter. We now had access to our pivot table. This was accomplished in the line of code:
`fare_by_date_pivot = fare_by_date.pivot(index = "date", columns = "type", values = "fare")`

We used the `.loc[]` function to have our pivot table only include rows of data with the dates between `2019-01-01` and `2019-04-28`. The line of code for this was:

`fare_by_date_pivot_df = fare_by_date_pivot.loc['2019-01-01':'2019-04-28']`.

Almost finished with the analysis, we converted the "date" index of our pivot table to a datetime datatype by using the code:

`fare_by_date_pivot_df.index = pd.to_datetime(fare_by_date_pivot_df.index)`. 

The last step of our analysis before plotting the data was to resample the pivot table `fare_by_week_pivot_df` was to resample the dates in the table by weeks. To accomplish this, we applied the `.resample()` function to the pivot table, and included the "W" parameter for the date resampling method. We finally applied the `.sum()` function to get the sum of the fares for each week.
`fare_by_week_pivot_df = fare_by_date_pivot_df.resample("W").sum()`

Finally, we had the data necessary for constructing our multiple line plot. We used the object oriented method in matplotlib to generate our overlayed line plot.
The following block of included code was used to generate Deliverable #2:

`from matplotlib import style`

`style.use('fivethirtyeight')`

`fix, ax = plt.subplots(figsize = (16,5.5))`


`timestamps = fare_by_week_pivot_df.index`

Note that for the below code, I extracted the list of weeks from the indexed `fare_by_week_pivot_df` dataframe. These elements for weeks were converted to strings We then iterated through the list of weeks and located character position `[5:7]` to return the month from the string. The numbers for the month were appended to to the `months` list. Later on in the code, I used the code for `len(months)` to get the number of elements in the list to eventually be used in the calculation for the increments of the xticks on the x-axis of the final figure. We believed this was performed pythonically so that any given csv input could find out how many months we wanted to include in a plotted chart. This was a more sophisticated way as opposed to manually entering the number of each month into the `xvalues` for the tick locations.

`weeks = timestamps.astype(str).tolist()`
`months = []`

`for i in weeks:`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`if i[5:7] not in months:`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`months.append(i[5:7])`



`xlabels = ("Jan\n2019", "Feb", "Mar", "Apr")`

The code below was used to plot the cotents of the final pivot table.

`ax.plot(fare_by_week_pivot_df)`



`ax.set_title("Total Fare by City Type")`

`ax.set_ylabel("Fare($USD)")`

The actual location of the x-ticks was acquired using the below code `ax.get_xticks()` and the resulting array was stored in a list of values. The values were then converted to integers so they could be used in the later `np.arange()` function.

`lists = ax.get_xticks().tolist()`

`intlist = []`

`for i in lists:`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`i = int(i)`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`intlist.append(i)`


`start = min(intlist)`

`end = max(intlist)`

`increments = int((end-start)/len(months))` 

The `np.arange()` function was necessary to reformat the default x-tick positions to a more preferable format.

`xaxis = np.arange(start,end,increments)`

List comprehension was used to extract the range of x-tick position values into a list.

`xvalues = [value for value in xaxis]`

`xaxis[0] = xaxis[0]+5`

`ax.set_xlim(737176,737177)`

`ax.set_xticks(xaxis)`

`ax.set(xticklabels = xlabels)`

`ax.legend(labels = ["Rural","Suburban", "Urban"], loc = "center")`

`plt.tight_layout()`

`plt.savefig("analysis/PyBer_fare_summary.png")`




## Results


From January to April 2019, we observed that Urban cities exhibited the highest number of total rides at 1625, total drivers at 2405, and highest total fares at $39.854.38. This makes sense because Urban cities tend to have a higher population density than Rural and Suburban cities. It also was logical that Rural cities, with their generally low population densities, had the lowest number of total rides at 125, lowest number of total drivers at 78, and lowest total fares at $4327.93. What also made sense was the highest average fare per ride at $34.62 for the Rural cities. This was logical because drivers are generally less likely to operate in Rural areas where there are less crowds of people waiting for transportation. Thus, the supply of rides in these areas causes the average fare per ride price to increase. Again, the higher average fare per driver at $55.49 for Rural areas makes sense because there are less available drivers and thus the denominator for making this calculation is lower. When dividing a sum by a lower denominator, you generally get a higher value. The lowest average fare per ride in Urban cities at $24.53 per ride makes sense because there is a supply of rides in this area. Thus, the higher supply will drive the price down. In line with the earlier algebraic logic, the lowest average fare per driver was in Urban cities because the higher availability of drivers contributes to a greater denominator in this calculation. When dividing by a greater denominator, such as more available drivers, you will get a lower value.


![Deliverable #1 Summary table](https://github.com/willmino/Pyber_Analysis/blob/main/Deliverable1.png)


Looking at our multiple line chart plot for each city type, we can see that urban cities generally have the highest total fare per week at a value ranging above $1500 total fares per week and below $2500 total fares per week throughout the months of January through April. Predictably, Rural cities have the lowest fares per week at about less than $500 total fares per week. Suburban cities range from above $500 total fares per week to below $1500 total fares per week. 

![Deliverable #2: Total Fare by City Type](https://github.com/willmino/Pyber_Analysis/blob/main/analysis/Pyber_fare_summary.png)

## Summary

