# pyber_analysis
Conduct ride share analysis using matplotlib, NumPy and SciPy

### Table of Contents
- [1 Purpose](#1-purpose)
- [2 Results](#2-results)
  - [2.1 Summary DataFrame](#21-summary-dataframe)
  - [2.2 Multiple Line Plot for Weekly Fares by City Type](#22-multiple-line-plot-for-weekly-fares-by-city-type)
- [3 Summary](#3-summary)

## 1 Purpose
The purpose of this project was to perform exploratory analysis and create visualizations about PyBer data - a Python based ride-sharing app company. The PyBer analysis explores the relationship between the city and number of drivers and riders, as well as the percentage of total fares, riders, and drivers by type of city. The results produced in this project should help Pyber improve access to ride sharing services and determine affordability for low-income riders.

## 2 Results

This project analyzed three primary city types: **Urban, Suburban, and Rural**. Within each city type, data for individual cities included the number of drivers per city, the date driven, and the ride fare on a given day. 

### 2.1 Summary DataFrame

The first step of this project was to construct a summary DataFrame based on the ride_data and city_data csv files. Once these two datasets were merged into a single DataFrame, the following information was extracted using Pandas:

1. Get the total number of rides for each city type
````
total_rides = pyber_data_df.groupby(["type"]).count()["ride_id"]
````
2. Get the total drivers for each city type
````
total_drivers = city_data_df.groupby(["type"]).sum()["driver_count"]
````
3. Get the total amount of fares for each city type
````
total_fares = pyber_data_df.groupby(["type"]).sum()["fare"]
````
4. Get the average fare per ride for each city type. 
````
average_fare_per_ride= total_fares/total_rides
````

5. Get the average fare per driver for each city type. 
````
average_fare_per_driver = total_fares/total_drivers
````

6. Create a PyBer summary DataFrame. 
````
pyber_summary_df = pd.DataFrame({"Total Rides": total_rides,
                                 "Total Drivers": total_drivers,
                                 "Total Fares": total_fares,
                                 "Average Fare per Ride": average_fare_per_ride,
                                 "Average Fare per Driver": average_fare_per_driver})
````

<p align="center">
  <img src="https://user-images.githubusercontent.com/95978097/152815846-e4a5753d-4ece-4ca4-9a37-9bdf28a356fb.png" />
</p>

Based on the Summary DataFrame, as expected, Urban cities had the largest number of rides compared to Suburban and Rural city types. Urban cities had greater than 61.54% more rides over Suburban cities, and greater than 92.31% compared to Rural cities. For total rides, following suit, Urban cities had greater than 79.63% more rides than Suburban cities, and 96.76% more than Rural cities. 

Given the large number of total rides and drivers, Urban cities also had the greatest amount in total fares - 51.43% more than Suburban cities, and 89.14% more than Rural cities. 

However, when calculating the average fare per ride and average fare per driver, Rural cities (followed by Suburban *then* Urban) had the highest for both categories. For Rural cities, the average ride was $34.62, $3.65 more per ride as compared to Suburban cities ($30.97), and $10.09 more than Urban cities ($24.53). Alternatively, Rural Pyber drivers averaged $15.99 more than Suburban drivers ($39.50), and $38.92 more than Urban cities ($16.57). 

Low number of rides and small pool of drivers for rural, less populated, less transit-centric, less metropolitan-like cities generally makes sense the farther one gets from a city with opposite characteristics and general land use designations. It could be assumed that because of the lack of supply of drivers, there is increase in distance between each ride for rural cities. 

In slightly more populated, more residential areas like suburban cities, the number of drivers and trips increase. However, assumptions could be made that distances may be decreased due to proximity to desired destinations or attributed to the increase in overall drivers; but lessens the average fare per ride *and* per driver. Finally, entering into highly populated, transit oriented metropolises, there appears to be a large number of drivers that follows the urban city's high demand for rides. Generally, trips within urban cities are assumed to be short and therefore explains the reduced average fare per ride and driver. 


### 2.2 Multiple Line Plot for Weekly Fares by City Type

Using pandas and matplotlib, the Pyber analysis called for a multiple-line graph that depics the total weekly fares for each city type. To construct this graph, the following steps and code snippets were run: 

1. Using groupby() to create a new DataFrame showing the sum of the fares for each date where the indices are the city type and date.
````
city_fares_by_date_df = pd.DataFrame(pyber_data_df.groupby(["type", "date"]).sum()["fare"])
````
2. Reset the index on the DataFrame you created in #1.
````
city_fares_reset_df = city_fares_by_date_df.reset_index()
````
3. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' to get the total fares for each type of city by the date. 
````
cleaned_city_fares_by_date_df = city_fares_reset_df.pivot(index="date", columns="type", values="fare")
````
4. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-29'.
````
fares_by_dates_df = cleaned_city_fares_by_date_df.loc['2019-01-01':'2019-04-29']
````
5. Set the "date" index to datetime datatype.
````
type(fares_by_dates_df.index)

fares_by_dates_df.index = pd.to_datetime(fares_by_dates_df.index)
````
6. Check that the datatype for the index is datetime using df.info()
````
fares_by_dates_df.info()
````
7. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
````
final_fares_by_dates_df = fares_by_dates_df.resample('W').sum()
````

8. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function
````
from matplotlib import style
style.use('fivethirtyeight')

final_fares_by_dates_df.plot(figsize=(20,7),)
plt.xlabel('')
plt.ylabel('Fare ($USD)')
plt.title('Total Fare by City Type')
plt.savefig('analysis/Pyber_fare_summary.png');

plt.show()
````

to produce this: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/95978097/152830206-9c0d714a-807b-4a49-8734-177115f944ff.png" />
</p>

Based on the image above, rural cities has the lowest total fares by city type week to week followed by Suburban and Urban cities. This information aligns with the results of the *Total Fares* column of the Summary DataFrame. However, the results of the line graph could be misleading to someone who has not seen the Summary DataFrame for the PyBer data. While the information is conveyed appropriately for total fares, it cannot be directly interpretated that rural cities has the highest average price per ride. 

## 3 Summary

Based on the results of the PyBer analysis, the following recommendations would be made to the CEO:

- [x] **Consider what economic class level is typically associated with each city type**
  
Statistically, more affluent individuals tend to reside in urban like areas that are in close proximity to highly populated areas, transit hubs, and work   destinations. To consider lower-income individuals and families that may not have access to alternative modes of transportation (or means of transportation themselves), increasing the fare price for shorter trips for wealthier folks as well as decreasing the fare price for those that live in more rural areas should be considered. 

- [x] **Consider conducting an additional analysis for time of day**

Although the average fare per ride is necessary information to know, further analysis into when individuals choose to use PyBer could determine fairer prices for both the rider and the driver. In comparison to other ride-sharing companies, prices vary depending on the time of day the trip is taken - increasing in high demand times and decreasing in quiet hours. Either implementing this method or conducting further exploratory data analysis is recommended. 

- [x] **Consider conducting an analysis of fulfilled vs. unfulfilled trips**

If PyBer is interested in improving accessibility to riders, another helpful analysis would be to analyze data for how many trips were successfully executed versus those that were not completed due to lack of driver supply. Based on the city and ride data provided, the question could be asked if the data excludes unfulfilled ride calls and includes all acquired fees (e.g. cancellation fees, clean-up fees, etc.).
