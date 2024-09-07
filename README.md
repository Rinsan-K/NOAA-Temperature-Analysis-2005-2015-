NOAA Temperature Analysis (2005-2015)

Project Overview

This project analyzes temperature data from NOAA for the region near Ann Arbor, Michigan. The project covers the following tasks:

1. Visualization of record high and low temperatures from 2005 to 2014.
2. Comparison with 2015 data to detect if any records were broken.
3. Plotting the geographical locations of weather stations on a map.
4. Summarizing the temperature data for 2015.

Dataset

The data for this assignment comes from the National Centers for Environmental Information (NCEI) [Daily Global Historical Climatology Network (GHCN-Daily)] and is stored in two files:

*temperature.csv: Contains daily temperature data with fields like id (station ID), date, element (TMAX/TMIN), and value.
*BinSize.csv: Contains information about the weather stations, including latitude and longitude.
Libraries Used
This project uses several external Python libraries for data manipulation, visualization, and geospatial analysis:

*pandas: For data manipulation and cleaning.
*numpy: For numerical operations.
*matplotlib: For generating line plots and scatter plots.
*seaborn: For additional plotting support.
*geopandas: For handling geospatial data.
*folium: For interactive map visualization.

Code Logic
Step 1: Data Loading and Preparation
The temperature data is loaded using pandas, and unnecessary columns are removed. The data is filtered for the years 2005-2014, and leap days (February 29) are excluded. The data is grouped by day_of_year to calculate record high (TMAX) and record low (TMIN) temperatures for each day of the year.


temperature_data = pd.read_csv('temperature.csv')
binsize_data = pd.read_csv('BinSize.csv')
# Filtering and cleaning
temperature_data_filtered = temperature_data[(temperature_data['date'].dt.year >= 2005) & (temperature_data['date'].dt.year <= 2014)]
temperature_data_filtered = temperature_data_filtered[~((temperature_data_filtered['date'].dt.month == 2) & (temperature_data_filtered['date'].dt.day == 29))]

Step 2: Plot Record Highs and Lows (2005-2014)
Using matplotlib, a line plot is created for the record high and low temperatures. The area between these two records is shaded for better visual distinction.


plt.plot(pivot_data['day_of_year_num'], pivot_data['TMAX'], label='Record High (2005-2014)', color='red')
plt.plot(pivot_data['day_of_year_num'], pivot_data['TMIN'], label='Record Low (2005-2014)', color='blue')
plt.fill_between(pivot_data['day_of_year_num'], pivot_data['TMAX'], pivot_data['TMIN'], color='gray', alpha=0.2)

Step 3: Overlay 2015 Data
The 2015 temperature data is loaded and compared to the 2005-2014 records. Any days where 2015 breaks a record high or low are highlighted using a scatter plot.


# Find days where records were broken
plt.scatter(record_high_broken['day_of_year_num'], record_high_broken['value_2015'], color='orange', label='2015 Record High Broken')
plt.scatter(record_low_broken['day_of_year_num'], record_low_broken['value_2015'], color='purple', label='2015 Record Low Broken')

Step 4: Map Visualization of Stations
Using geopandas and folium, the geographical locations of the weather stations near Ann Arbor are plotted on an interactive map.

map_ann_arbor = folium.Map(location=[42.2808, -83.7430], zoom_start=10)
for idx, row in stations_gdf.iterrows():
    folium.Marker([row['latitude'], row['longitude']], popup=row['station_id']).add_to(map_ann_arbor)

Step 5: Temperature Summary (2015)
A time series plot of the 2015 temperature data for Ann Arbor is generated to summarize the temperature trends.

plt.plot(ann_arbor_2015['date'], ann_arbor_2015['value'], label='Temperature 2015', color='green')


Additional Notes

Leap Days: February 29 was excluded from the analysis to maintain consistency in the day-of-year comparison.
Geospatial Visualization: Weather stations were plotted on an interactive map using folium, which allows zooming and station detail exploration.
Temperature Values: All temperatures are in tenths of degrees Celsius, as is standard in the NOAA dataset.

Conclusion

This project provides a comprehensive temperature analysis near Ann Arbor, Michigan, visualizing record highs and lows, detecting broken records, and plotting weather stations on a map. The Jupyter notebook includes detailed explanations and visual outputs for easy interpretation.