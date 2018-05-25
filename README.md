

```python
# Dependencies
import json
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline 
import seaborn as sns
import openweathermapy as ow
from citipy import citipy
import requests
import time
import random
from config import api_key
```


```python
cities = []
country_code =[]

bignum = 3000 # get way > than 500 random cities as many cities aren't covered by openweathermap & lead to 404 error
url = "http://api.openweathermap.org/data/2.5/weather?" 

while len(cities) < bignum:                                      # num set to number of cities you need data for
    latitude = random.uniform(-1, 1) * 90                     # get random latitude
    longitude = random.uniform(-1, 1) * 180                   # get random longitude
    city = citipy.nearest_city(latitude, longitude)           # get nearest city from latitude & longitude
    if city not in cities:                                    # skip duplicate city
        cities.append(city.city_name)                         # save city in the cities list
        country_code.append((city.country_code).upper())

print(cities)
```


```python
# create lists for all the variables
temperature = []
humidity = []
cloudiness = []
windspeed = []
weather_data = []
date = []
lat = []
lng = []
set = 0
ncity = 0
innerloop = 51   # 51 in each run before delay of 61 sec
num = 505

while ncity < num:      # counter for outer loop which is executed 10 times
    j = 0                  # reset inner loop counter for next 51 iterations. (so data for 51x10 = 510 cities) 
    while j < innerloop:   # data extracted in 51 calls each time, separated by 61 sec delays.
        # For temp in Fahrenheit & wind speed in mph, added "&units=imperial" in the query_url below.
        print("ncity = " + str(ncity))
        query_url = url + "q=" +  cities[ncity] + "," +  country_code[ncity] + "&appid=" + api_key + "&units=imperial"
        response = requests.get(query_url).json()             # get new city's weather using query_url
        if response["cod"] != 200:   # if any code other than 'ok', skip and go for next city
            cities.pop(ncity)
            country_code.pop(ncity)
            continue
        weather_data.append(response) 
        print("City Number = " + str(ncity) + ", Processing record " + str(j) + " of set "+ str(set) + " | " + cities[ncity])
        print(query_url)
        lat.append(response["coord"]["lat"])                  # save latitude in the latitudes list
        lng.append(response["coord"]["lon"])                  # save longitude in the longitudes list
        weather_data.append(response)                         # save new city's weather in weather_data list
        temperature.append(response["main"]["temp_max"])      # save new city's max temperature in temperature list
        humidity.append(response["main"]["humidity"])         # save new city's humidity in humidity list
        cloudiness.append(response["clouds"]["all"])          # save new city's clouds in cloudiness list
        windspeed.append(response["wind"]["speed"])           # save new city's windspeed in windspeed list
        date.append(response["dt"])
        ncity = ncity + 1
        j=j+1   # if response["cod"] == 200, then you want to increment the counter too for the next iteration. 
    set = set+1 # go for the next set of 50 calls/cities
    time.sleep(61)   # delay just over a minute as only upto 60 calls/min are allowed.
```


```python
City = cities[:505]
Country=country_code[:505]
Date = date[:505]
Lat=lat[:505]
Lng=lng[:505]
Humidity=humidity[:505]
Max_Temp=temperature[:505]
Cloudiness = cloudiness[:505]
Wind_Speed = windspeed[:505]

cities_df = pd.DataFrame(dict(City=City, Country=Country, Date=Date, Lat=Lat, Lng=Lng, Humidity=Humidity, Max_Temp=Max_Temp, Cloudiness=Cloudiness, Wind_Speed=Wind_Speed))
cities_df = cities_df[["City", "Cloudiness", "Country", "Date", "Humidity", "Lat", "Lng", "Max_Temp", "Wind_Speed"]]
cities_df.to_csv("cities_weather_data.csv", encoding="utf-8", index=False)
cities_df
```


```python
max_temp = cities_df['Max_Temp'].max()
min_temp = cities_df['Max_Temp'].min()
max_lat = cities_df['Lat'].max()
min_lat = cities_df['Lat'].min()

# Construct the colormap

current_palette = sns.color_palette("muted", n_colors=5)
cmap = ListedColormap(sns.color_palette(current_palette).as_hex())

# Relationship between temperature and latitute
# Tell matplotlib to create a scatter plot based upon the above data


plt.scatter(cities_df["Lat"], 
            cities_df["Max_Temp"],
            edgecolor="black", linewidths=1, marker="o", 
            c = colors,
            cmap = cmap,
            alpha=0.8)

Date = time.strftime("%D %H:%M", time.localtime(int(date[0]))) # extract date to include in the plot title
Date = Date[:8]   # get only date i.e. first 8 chars of Date string

# Create a title, x label, and y label for our chart
plt.title("Latitude vs Max Temperature" + ' ' + Date)
plt.ylabel("Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

plt.xlim([min_lat -10, max_lat +10])
plt.ylim([min_temp-10, max_temp +10]) 

# Save the figure
plt.savefig("Max Temperature_Latitude.png")

plt.show()
```


```python
max_humidity = cities_df['Humidity'].max()
min_humidity = cities_df['Humidity'].min()
max_lat  = cities_df['Lat'].max()
min_lat  = cities_df['Lat'].min()

plt.scatter(cities_df["Lat"], 
            cities_df["Humidity"],
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="Zip Codes")

# Incorporate the other graph properties
plt.title("City Latitude vs. Humidity (" + Date + ")")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([min_lat-10, max_lat+10])         # margins so plot axes extend beyond max/min values
plt.ylim([min_humidity-10, max_humidity+10])  # margins so plot axes extend beyond max/min values 

# Save the figure
plt.savefig("Humidity_Latitude.png")

# Show plot
plt.show()
```


```python
max_cloudiness = cities_df['Cloudiness'].max()
min_cloudiness = cities_df['Cloudiness'].min()
max_lat  = cities_df['Lat'].max()
min_lat  = cities_df['Lat'].min()

plt.scatter(cities_df["Lat"], 
            cities_df["Cloudiness"],
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8)

# Incorporate the other graph properties
plt.title("City Latitude vs. Cloudiness (" + Date + ")")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([min_lat-10, max_lat+10])         # margins so plot axes extend beyond max/min values
plt.ylim([min_cloudiness-10, max_cloudiness+10])  # margins so plot axes extend beyond max/min values 

plt.savefig("Cloudiness_Latitude.png")

# Show plot
plt.show()
```


```python
max_windspeed = cities_df['Wind_Speed'].max()
min_windspeed = cities_df['Wind_Speed'].min()
max_lat  = cities_df['Lat'].max()
min_lat  = cities_df['Lat'].min()

plt.scatter(cities_df["Lat"], 
            cities_df["Wind_Speed"],
            edgecolor="black", linewidths=1, marker="o", 
            alpha=0.8, label="Zip Codes")

# Incorporate the other graph properties
plt.title("City Latitude vs. Wind Speed (" + Date + ")")
plt.ylabel("Wind_Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([min_lat-10, max_lat+10])         # margins so plot axes extend beyond max/min values
plt.ylim([min_windspeed-10, max_windspeed+10])         # margins so plot axes extend beyond max/min values 

# Save the figure
plt.savefig("WindSpeed_Latitude.png")

# Show plot
plt.show()
```
