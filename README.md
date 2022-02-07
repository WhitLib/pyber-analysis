# pyber_analysis
Conduct ride share analysis using matplotlib, NumPy and SciPy

### Table of Contents
- [1 Purpose](#1-purpose)
  - [1.1 Required Tools](#11-required-tools)
- [2 Results](#2-results)
- [3 Summary](#3-results)

## 1 Purpose
The purpose of this project was to perform exploratory analysis and create visualizations about PyBer data - a Python based ride-sharing app company. The PyBer analysis explores the relationship between the city and number of drivers and riders, as well as the percentage of total fares, riders, and drivers by type of city. The results produced in this project should help Pyber improve access to ride sharing services and determine affordability for low-income riders.

## 2 Results

This project analyzed three primary city types: **Urban, Suburban, and Rural**. Within each city type, data for individual cities included the number of drivers per city, the date driven, and the ride fare on a given day. 

### 2.2 Summary DataFrame

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

<img width="535" alt="Screen Shot 2022-02-07 at 7 15 11 AM" src="https://user-images.githubusercontent.com/95978097/152815846-e4a5753d-4ece-4ca4-9a37-9bdf28a356fb.png">

Based on the Summary DataFrame, as expected, Urban cities had the largest number of rides compared to Suburban and Rural city types. Urban cities had greater than 61.54% more rides over Suburban cities, and greater than 92.31% compared to Rural cities. For total rides, following suit, Urban cities had greater than 79.63% more rides than Suburban cities, and 96.76% more than Rural cities. 

Given the large number of total rides and drivers, Urban cities also had the greatest amount in total fares - 51.43% more than Suburban cities, and 89.14% more than Rural cities. 
