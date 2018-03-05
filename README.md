**Analysis** 
**#1** As expected, the closer you get to the equator, the higher the max daily temperature is in Fahrenheit.

**#2** There does not appear to be much correlation between latitude and cloudiness. 

**#3** It appears that in latitude vs. humidity, the closer you get to the equator the humidity is higher but there is not a strong correlation here either.

**#4** There appears to be more variability in wind speed the further away you get from the equator, particularly in the northern hemisphere here.  This may be due to the northern hemisphere being in its winter state.

```python
# Dependencies
import pandas as pd
import numpy as np
import requests
import json
import matplotlib.pyplot as plt
import seaborn as sns
import random
from citipy import citipy
import time as time
import unidecode
```


```python
# Grab random cities from CSV
worldcities_pd = pd.read_csv("simple-cities.csv", encoding = 'utf8')

cities = worldcities_pd.sample(n=600)

cities.head()
#random.sample
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>city_ascii</th>
      <th>lat</th>
      <th>lng</th>
      <th>pop</th>
      <th>country</th>
      <th>iso2</th>
      <th>iso3</th>
      <th>province</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5823</th>
      <td>Bade</td>
      <td>Bade</td>
      <td>24.957500</td>
      <td>121.298889</td>
      <td>172065.0</td>
      <td>Taiwan</td>
      <td>TW</td>
      <td>TWN</td>
      <td>Taoyuan</td>
    </tr>
    <tr>
      <th>6733</th>
      <td>Bloomington</td>
      <td>Bloomington</td>
      <td>40.484595</td>
      <td>-88.993597</td>
      <td>99842.5</td>
      <td>United+States+of+America</td>
      <td>US</td>
      <td>USA</td>
      <td>Illinois</td>
    </tr>
    <tr>
      <th>1672</th>
      <td>Sanming</td>
      <td>Sanming</td>
      <td>26.229987</td>
      <td>117.580048</td>
      <td>104941.5</td>
      <td>China</td>
      <td>CN</td>
      <td>CHN</td>
      <td>Fujian</td>
    </tr>
    <tr>
      <th>1665</th>
      <td>Valdivia</td>
      <td>Valdivia</td>
      <td>-39.795001</td>
      <td>-73.245023</td>
      <td>146509.0</td>
      <td>Chile</td>
      <td>CL</td>
      <td>CHL</td>
      <td>Los+Ríos</td>
    </tr>
    <tr>
      <th>477</th>
      <td>Caboolture</td>
      <td>Caboolture</td>
      <td>-27.082961</td>
      <td>152.949982</td>
      <td>26495.5</td>
      <td>Australia</td>
      <td>AU</td>
      <td>AUS</td>
      <td>Queensland</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Save Parameters for Use
api_key = "5a1900b8b6b43ecdc7d4067c67fe076e"

units = "imperial"

    
```


```python


```


```python
if __name__ == '__main__':
# Create blank columns for necessary fields

    cities_name = []
    cities_cloud = []
    cities_country = []
    cities_date = []
    cities_humid = []
    cities_lat = []
    cities_lon = []
    cities_temp = []
    cities_wind = []
# Counter
    row_count = 0

# Loop through and grab the lat/lng using Google maps
    for index, row in cities.iterrows():
    
    # Create endpoint URL
        url = "http://api.openweathermap.org/data/2.5/weather?&q=" + str(row["city_ascii"]) + "&units=" + units + "&appid=" + api_key
    
    # Print log to ensure loop is working correctly
        print("Now retrieving city # " + str(row_count))
        print(url)
        row_count += 1
    
    # Run requests to grab the JSON at the requested URL
        zip_location = requests.get(url).json()

        time.sleep(1)
    # Append the lat/lng to the appropriate columns
    # Use try / except to skip any cities with errors
        try:
            cities_name.append(zip_location["name"])
            cities_cloud.append(zip_location["clouds"]["all"])
            cities_country.append(zip_location["sys"]["country"])
            cities_date.append(zip_location["dt"])
            cities_humid.append(zip_location["main"]["humidity"])
            cities_lat.append(zip_location["coord"]["lat"])
            cities_lon.append(zip_location["coord"]["lon"])
            cities_temp.append(zip_location["main"]["temp_max"])
            cities_wind.append(zip_location["wind"]["speed"])
    
        except:     
           print("Error with city data. Skipping")
        continue

```

    Now retrieving city # 0
    http://api.openweathermap.org/data/2.5/weather?&q=Bade&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 1
    http://api.openweathermap.org/data/2.5/weather?&q=Bloomington&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 2
    http://api.openweathermap.org/data/2.5/weather?&q=Sanming&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 3
    http://api.openweathermap.org/data/2.5/weather?&q=Valdivia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 4
    http://api.openweathermap.org/data/2.5/weather?&q=Caboolture&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 5
    http://api.openweathermap.org/data/2.5/weather?&q=Qui+Nhon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 6
    http://api.openweathermap.org/data/2.5/weather?&q=Oshikango&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 7
    http://api.openweathermap.org/data/2.5/weather?&q=Roswell&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 8
    http://api.openweathermap.org/data/2.5/weather?&q=Mozdok&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 9
    http://api.openweathermap.org/data/2.5/weather?&q=Kindia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 10
    http://api.openweathermap.org/data/2.5/weather?&q=Norman+Wells&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 11
    http://api.openweathermap.org/data/2.5/weather?&q=Lapa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 12
    http://api.openweathermap.org/data/2.5/weather?&q=Karimnagar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 13
    http://api.openweathermap.org/data/2.5/weather?&q=Kyzyl&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 14
    http://api.openweathermap.org/data/2.5/weather?&q=Koktokay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 15
    http://api.openweathermap.org/data/2.5/weather?&q=Stara+Zagora&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 16
    http://api.openweathermap.org/data/2.5/weather?&q=Samara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 17
    http://api.openweathermap.org/data/2.5/weather?&q=Melville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 18
    http://api.openweathermap.org/data/2.5/weather?&q=Shache&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 19
    http://api.openweathermap.org/data/2.5/weather?&q=Eugene&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 20
    http://api.openweathermap.org/data/2.5/weather?&q=Limeira&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 21
    http://api.openweathermap.org/data/2.5/weather?&q=Mariehamn&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 22
    http://api.openweathermap.org/data/2.5/weather?&q=Strasbourg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 23
    http://api.openweathermap.org/data/2.5/weather?&q=Shemonaikha&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 24
    http://api.openweathermap.org/data/2.5/weather?&q=Nizwa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 25
    http://api.openweathermap.org/data/2.5/weather?&q=Las+Palmas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 26
    http://api.openweathermap.org/data/2.5/weather?&q=Zhuhai&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 27
    http://api.openweathermap.org/data/2.5/weather?&q=Sikar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 28
    http://api.openweathermap.org/data/2.5/weather?&q=Rasht&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 29
    http://api.openweathermap.org/data/2.5/weather?&q=Farah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 30
    http://api.openweathermap.org/data/2.5/weather?&q=Bologna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 31
    http://api.openweathermap.org/data/2.5/weather?&q=Caldwell&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 32
    http://api.openweathermap.org/data/2.5/weather?&q=Rockhampton&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 33
    http://api.openweathermap.org/data/2.5/weather?&q=Shashemene&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 34
    http://api.openweathermap.org/data/2.5/weather?&q=Entebbe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 35
    http://api.openweathermap.org/data/2.5/weather?&q=As+Salt&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 36
    http://api.openweathermap.org/data/2.5/weather?&q=Andamooka&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 37
    http://api.openweathermap.org/data/2.5/weather?&q=Eisenstadt&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 38
    http://api.openweathermap.org/data/2.5/weather?&q=San+Ramon+de+la+Nueva+Oran&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 39
    http://api.openweathermap.org/data/2.5/weather?&q=Az+Aubayr&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 40
    http://api.openweathermap.org/data/2.5/weather?&q=Azogues&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 41
    http://api.openweathermap.org/data/2.5/weather?&q=Baturite&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 42
    http://api.openweathermap.org/data/2.5/weather?&q=Beaufort+West&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 43
    http://api.openweathermap.org/data/2.5/weather?&q=Ciudad+Camargo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 44
    http://api.openweathermap.org/data/2.5/weather?&q=Abaetetuba&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 45
    http://api.openweathermap.org/data/2.5/weather?&q=Tekax&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 46
    http://api.openweathermap.org/data/2.5/weather?&q=Viljandi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 47
    http://api.openweathermap.org/data/2.5/weather?&q=Maputo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 48
    http://api.openweathermap.org/data/2.5/weather?&q=Opole&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 49
    http://api.openweathermap.org/data/2.5/weather?&q=Segou&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 50
    http://api.openweathermap.org/data/2.5/weather?&q=Metairie&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 51
    http://api.openweathermap.org/data/2.5/weather?&q=Capitao+Poco&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 52
    http://api.openweathermap.org/data/2.5/weather?&q=Zaghouan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 53
    http://api.openweathermap.org/data/2.5/weather?&q=Lhasa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 54
    http://api.openweathermap.org/data/2.5/weather?&q=Spokane&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 55
    http://api.openweathermap.org/data/2.5/weather?&q=Ambatondrazaka&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 56
    http://api.openweathermap.org/data/2.5/weather?&q=Lahti&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 57
    http://api.openweathermap.org/data/2.5/weather?&q=Thessalon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 58
    http://api.openweathermap.org/data/2.5/weather?&q=Zillah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 59
    http://api.openweathermap.org/data/2.5/weather?&q=Macara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 60
    http://api.openweathermap.org/data/2.5/weather?&q=Butuan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 61
    http://api.openweathermap.org/data/2.5/weather?&q=Patos&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 62
    http://api.openweathermap.org/data/2.5/weather?&q=Portland&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 63
    http://api.openweathermap.org/data/2.5/weather?&q=Morelia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 64
    http://api.openweathermap.org/data/2.5/weather?&q=Shonzhy&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 65
    http://api.openweathermap.org/data/2.5/weather?&q=Opuwo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 66
    http://api.openweathermap.org/data/2.5/weather?&q=El+Progreso&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 67
    http://api.openweathermap.org/data/2.5/weather?&q=Shantou&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 68
    http://api.openweathermap.org/data/2.5/weather?&q=Bemidji&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 69
    http://api.openweathermap.org/data/2.5/weather?&q=Belgrade&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 70
    http://api.openweathermap.org/data/2.5/weather?&q=Jaffna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 71
    http://api.openweathermap.org/data/2.5/weather?&q=Januaria&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 72
    http://api.openweathermap.org/data/2.5/weather?&q=Swan+Hill&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 73
    http://api.openweathermap.org/data/2.5/weather?&q=Inuvik&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 74
    http://api.openweathermap.org/data/2.5/weather?&q=Hotan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 75
    http://api.openweathermap.org/data/2.5/weather?&q=Puerto+San+Julian&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 76
    http://api.openweathermap.org/data/2.5/weather?&q=Mansfield&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 77
    http://api.openweathermap.org/data/2.5/weather?&q=Kenora&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 78
    http://api.openweathermap.org/data/2.5/weather?&q=Niigata&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 79
    http://api.openweathermap.org/data/2.5/weather?&q=Soyo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 80
    http://api.openweathermap.org/data/2.5/weather?&q=Abomey&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 81
    http://api.openweathermap.org/data/2.5/weather?&q=Guaymas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 82
    http://api.openweathermap.org/data/2.5/weather?&q=Trang&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 83
    http://api.openweathermap.org/data/2.5/weather?&q=Gueppi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 84
    http://api.openweathermap.org/data/2.5/weather?&q=Peterborough&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 85
    http://api.openweathermap.org/data/2.5/weather?&q=Nebbi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 86
    http://api.openweathermap.org/data/2.5/weather?&q=Zmeinogorsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 87
    http://api.openweathermap.org/data/2.5/weather?&q=Lingyuan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 88
    http://api.openweathermap.org/data/2.5/weather?&q=Mahalapye&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 89
    http://api.openweathermap.org/data/2.5/weather?&q=Norman&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 90
    http://api.openweathermap.org/data/2.5/weather?&q=Santiago+Ixcuintla&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 91
    http://api.openweathermap.org/data/2.5/weather?&q=Saida&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 92
    http://api.openweathermap.org/data/2.5/weather?&q=Azare&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 93
    http://api.openweathermap.org/data/2.5/weather?&q=Shoyna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 94
    http://api.openweathermap.org/data/2.5/weather?&q=Burley&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 95
    http://api.openweathermap.org/data/2.5/weather?&q=Boorama&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 96
    http://api.openweathermap.org/data/2.5/weather?&q=Danjiangkou&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 97
    http://api.openweathermap.org/data/2.5/weather?&q=Ulkan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 98
    http://api.openweathermap.org/data/2.5/weather?&q=Narvik&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 99
    http://api.openweathermap.org/data/2.5/weather?&q=La+Rochelle&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 100
    http://api.openweathermap.org/data/2.5/weather?&q=Nawabganj&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 101
    http://api.openweathermap.org/data/2.5/weather?&q=Ramla&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 102
    http://api.openweathermap.org/data/2.5/weather?&q=Chadron&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 103
    http://api.openweathermap.org/data/2.5/weather?&q=Smithers&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 104
    http://api.openweathermap.org/data/2.5/weather?&q=Lafia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 105
    http://api.openweathermap.org/data/2.5/weather?&q=Siglan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 106
    http://api.openweathermap.org/data/2.5/weather?&q=Mudangiang&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 107
    http://api.openweathermap.org/data/2.5/weather?&q=Tena&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 108
    http://api.openweathermap.org/data/2.5/weather?&q=Jeremie&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 109
    http://api.openweathermap.org/data/2.5/weather?&q=Qaqortoq&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 110
    http://api.openweathermap.org/data/2.5/weather?&q=Maceio&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 111
    http://api.openweathermap.org/data/2.5/weather?&q=Columbia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 112
    http://api.openweathermap.org/data/2.5/weather?&q=Nanded&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 113
    http://api.openweathermap.org/data/2.5/weather?&q=Nakhon+Si+Thammarat&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 114
    http://api.openweathermap.org/data/2.5/weather?&q=Lanxi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 115
    http://api.openweathermap.org/data/2.5/weather?&q=Sonsonate&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 116
    http://api.openweathermap.org/data/2.5/weather?&q=Capanema&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 117
    http://api.openweathermap.org/data/2.5/weather?&q=Sullana&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 118
    http://api.openweathermap.org/data/2.5/weather?&q=Coimbatore&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 119
    http://api.openweathermap.org/data/2.5/weather?&q=Muineachan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 120
    http://api.openweathermap.org/data/2.5/weather?&q=Brovary&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 121
    http://api.openweathermap.org/data/2.5/weather?&q=Muar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 122
    http://api.openweathermap.org/data/2.5/weather?&q=Yala&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 123
    http://api.openweathermap.org/data/2.5/weather?&q=Semarang&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 124
    http://api.openweathermap.org/data/2.5/weather?&q=Kingston&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 125
    http://api.openweathermap.org/data/2.5/weather?&q=Comallo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 126
    http://api.openweathermap.org/data/2.5/weather?&q=Angarsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 127
    http://api.openweathermap.org/data/2.5/weather?&q=Dalandzadgad&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 128
    http://api.openweathermap.org/data/2.5/weather?&q=Phrae&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 129
    http://api.openweathermap.org/data/2.5/weather?&q=Mysore&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 130
    http://api.openweathermap.org/data/2.5/weather?&q=Toumodi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 131
    http://api.openweathermap.org/data/2.5/weather?&q=Rotterdam&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 132
    http://api.openweathermap.org/data/2.5/weather?&q=Pensacola&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 133
    http://api.openweathermap.org/data/2.5/weather?&q=Pedro+Juan+Caballero&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 134
    http://api.openweathermap.org/data/2.5/weather?&q=Birmingham&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 135
    http://api.openweathermap.org/data/2.5/weather?&q=Nigde&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 136
    http://api.openweathermap.org/data/2.5/weather?&q=Daan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 137
    http://api.openweathermap.org/data/2.5/weather?&q=Ceduna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 138
    http://api.openweathermap.org/data/2.5/weather?&q=Mubi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 139
    http://api.openweathermap.org/data/2.5/weather?&q=Marquette&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 140
    http://api.openweathermap.org/data/2.5/weather?&q=Camana&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 141
    http://api.openweathermap.org/data/2.5/weather?&q=Los+Angeles&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 142
    http://api.openweathermap.org/data/2.5/weather?&q=Kalmar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 143
    http://api.openweathermap.org/data/2.5/weather?&q=Ust+Olensk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 144
    http://api.openweathermap.org/data/2.5/weather?&q=Villamontes&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 145
    http://api.openweathermap.org/data/2.5/weather?&q=Merimbula&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 146
    http://api.openweathermap.org/data/2.5/weather?&q=Taraz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 147
    http://api.openweathermap.org/data/2.5/weather?&q=Zhytomyr&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 148
    http://api.openweathermap.org/data/2.5/weather?&q=Muncie&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 149
    http://api.openweathermap.org/data/2.5/weather?&q=Guilin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 150
    http://api.openweathermap.org/data/2.5/weather?&q=Kattaqorgon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 151
    http://api.openweathermap.org/data/2.5/weather?&q=Oum+Hadjer&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 152
    http://api.openweathermap.org/data/2.5/weather?&q=Novo+Hamburgo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 153
    http://api.openweathermap.org/data/2.5/weather?&q=Chukai&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 154
    http://api.openweathermap.org/data/2.5/weather?&q=Garissa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 155
    http://api.openweathermap.org/data/2.5/weather?&q=Kingston&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 156
    http://api.openweathermap.org/data/2.5/weather?&q=Konza&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 157
    http://api.openweathermap.org/data/2.5/weather?&q=Severouralsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 158
    http://api.openweathermap.org/data/2.5/weather?&q=Chita&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 159
    http://api.openweathermap.org/data/2.5/weather?&q=Gavle&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 160
    http://api.openweathermap.org/data/2.5/weather?&q=Miaoli&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 161
    http://api.openweathermap.org/data/2.5/weather?&q=Yoro&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 162
    http://api.openweathermap.org/data/2.5/weather?&q=Watertown&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 163
    http://api.openweathermap.org/data/2.5/weather?&q=Irvine&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 164
    http://api.openweathermap.org/data/2.5/weather?&q=Hamah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 165
    http://api.openweathermap.org/data/2.5/weather?&q=Gangtok&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 166
    http://api.openweathermap.org/data/2.5/weather?&q=Orodara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 167
    http://api.openweathermap.org/data/2.5/weather?&q=Kokomo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 168
    http://api.openweathermap.org/data/2.5/weather?&q=Khanty+Mansiysk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 169
    http://api.openweathermap.org/data/2.5/weather?&q=Minsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 170
    http://api.openweathermap.org/data/2.5/weather?&q=Yasothon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 171
    http://api.openweathermap.org/data/2.5/weather?&q=Cranbourne&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 172
    http://api.openweathermap.org/data/2.5/weather?&q=Walvis+Bay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 173
    http://api.openweathermap.org/data/2.5/weather?&q=Kalbarri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 174
    http://api.openweathermap.org/data/2.5/weather?&q=Palm+Springs&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 175
    http://api.openweathermap.org/data/2.5/weather?&q=Shuya&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 176
    http://api.openweathermap.org/data/2.5/weather?&q=Ondorhaan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 177
    http://api.openweathermap.org/data/2.5/weather?&q=Vanadzor&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 178
    http://api.openweathermap.org/data/2.5/weather?&q=Jinxi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 179
    http://api.openweathermap.org/data/2.5/weather?&q=Samut+Prakan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 180
    http://api.openweathermap.org/data/2.5/weather?&q=Amravati&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 181
    http://api.openweathermap.org/data/2.5/weather?&q=Saskatoon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 182
    http://api.openweathermap.org/data/2.5/weather?&q=Pescara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 183
    http://api.openweathermap.org/data/2.5/weather?&q=Valparaiso&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 184
    http://api.openweathermap.org/data/2.5/weather?&q=Dund-Us&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 185
    http://api.openweathermap.org/data/2.5/weather?&q=Thai+Binh&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 186
    http://api.openweathermap.org/data/2.5/weather?&q=Georgetown&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 187
    http://api.openweathermap.org/data/2.5/weather?&q=Jizzax&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 188
    http://api.openweathermap.org/data/2.5/weather?&q=Novyy+Port&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 189
    http://api.openweathermap.org/data/2.5/weather?&q=San+Pedro&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 190
    http://api.openweathermap.org/data/2.5/weather?&q=New+Braunfels&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 191
    http://api.openweathermap.org/data/2.5/weather?&q=Yangambi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 192
    http://api.openweathermap.org/data/2.5/weather?&q=Bogor&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 193
    http://api.openweathermap.org/data/2.5/weather?&q=Calais&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 194
    http://api.openweathermap.org/data/2.5/weather?&q=St.+John's&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 195
    http://api.openweathermap.org/data/2.5/weather?&q=Oran&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 196
    http://api.openweathermap.org/data/2.5/weather?&q=Funtua&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 197
    http://api.openweathermap.org/data/2.5/weather?&q=Hathras&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 198
    http://api.openweathermap.org/data/2.5/weather?&q=Pilar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 199
    http://api.openweathermap.org/data/2.5/weather?&q=Palmer&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 200
    http://api.openweathermap.org/data/2.5/weather?&q=Vilyuysk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 201
    http://api.openweathermap.org/data/2.5/weather?&q=Karlskrona&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 202
    http://api.openweathermap.org/data/2.5/weather?&q=Gastre&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 203
    http://api.openweathermap.org/data/2.5/weather?&q=Cebu&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 204
    http://api.openweathermap.org/data/2.5/weather?&q=Linfen&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 205
    http://api.openweathermap.org/data/2.5/weather?&q=Sukhothai&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 206
    http://api.openweathermap.org/data/2.5/weather?&q=Lujan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 207
    http://api.openweathermap.org/data/2.5/weather?&q=Childress&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 208
    http://api.openweathermap.org/data/2.5/weather?&q=Dasoguz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 209
    http://api.openweathermap.org/data/2.5/weather?&q=Lebowakgomo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 210
    http://api.openweathermap.org/data/2.5/weather?&q=Bella+Bella&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 211
    http://api.openweathermap.org/data/2.5/weather?&q=Altdorf&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 212
    http://api.openweathermap.org/data/2.5/weather?&q=Singleton&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 213
    http://api.openweathermap.org/data/2.5/weather?&q=Foumban&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 214
    http://api.openweathermap.org/data/2.5/weather?&q=Barabinsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 215
    http://api.openweathermap.org/data/2.5/weather?&q=Latacunga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 216
    http://api.openweathermap.org/data/2.5/weather?&q=Banfora&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 217
    http://api.openweathermap.org/data/2.5/weather?&q=Lamas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 218
    http://api.openweathermap.org/data/2.5/weather?&q=Logrono&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 219
    http://api.openweathermap.org/data/2.5/weather?&q=Perryville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 220
    http://api.openweathermap.org/data/2.5/weather?&q=Huanta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 221
    http://api.openweathermap.org/data/2.5/weather?&q=Tarin+Kowt&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 222
    http://api.openweathermap.org/data/2.5/weather?&q=Daqing&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 223
    http://api.openweathermap.org/data/2.5/weather?&q=Lahore&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 224
    http://api.openweathermap.org/data/2.5/weather?&q=Ndalatando&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 225
    http://api.openweathermap.org/data/2.5/weather?&q=Karamay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 226
    http://api.openweathermap.org/data/2.5/weather?&q=Jasper&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 227
    http://api.openweathermap.org/data/2.5/weather?&q=Tolyatti&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 228
    http://api.openweathermap.org/data/2.5/weather?&q=Novoshakhtinsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 229
    http://api.openweathermap.org/data/2.5/weather?&q=Koror&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 230
    http://api.openweathermap.org/data/2.5/weather?&q=Agartala&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 231
    http://api.openweathermap.org/data/2.5/weather?&q=Ambriz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 232
    http://api.openweathermap.org/data/2.5/weather?&q=Parintins&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 233
    http://api.openweathermap.org/data/2.5/weather?&q=Banghazi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 234
    http://api.openweathermap.org/data/2.5/weather?&q=Bhimphedi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 235
    http://api.openweathermap.org/data/2.5/weather?&q=Agordat&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 236
    http://api.openweathermap.org/data/2.5/weather?&q=Blacksburg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 237
    http://api.openweathermap.org/data/2.5/weather?&q=Adi+Ugri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 238
    http://api.openweathermap.org/data/2.5/weather?&q=Daru&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 239
    http://api.openweathermap.org/data/2.5/weather?&q=Bielefeld&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 240
    http://api.openweathermap.org/data/2.5/weather?&q=Scottsdale&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 241
    http://api.openweathermap.org/data/2.5/weather?&q=Machiques&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 242
    http://api.openweathermap.org/data/2.5/weather?&q=Mananjary&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 243
    http://api.openweathermap.org/data/2.5/weather?&q=Millerovo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 244
    http://api.openweathermap.org/data/2.5/weather?&q=Bunbury&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 245
    http://api.openweathermap.org/data/2.5/weather?&q=Sambalpur&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 246
    http://api.openweathermap.org/data/2.5/weather?&q=Racine&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 247
    http://api.openweathermap.org/data/2.5/weather?&q=Maradi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 248
    http://api.openweathermap.org/data/2.5/weather?&q=Vac&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 249
    http://api.openweathermap.org/data/2.5/weather?&q=Motupe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 250
    http://api.openweathermap.org/data/2.5/weather?&q=Arlit&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 251
    http://api.openweathermap.org/data/2.5/weather?&q=Pardubice&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 252
    http://api.openweathermap.org/data/2.5/weather?&q=Kimhyonggwon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 253
    http://api.openweathermap.org/data/2.5/weather?&q=Gornyak&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 254
    http://api.openweathermap.org/data/2.5/weather?&q=Parry+Sound&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 255
    http://api.openweathermap.org/data/2.5/weather?&q=Gwadar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 256
    http://api.openweathermap.org/data/2.5/weather?&q=Petrozavodsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 257
    http://api.openweathermap.org/data/2.5/weather?&q=Huntsville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 258
    http://api.openweathermap.org/data/2.5/weather?&q=Medellin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 259
    http://api.openweathermap.org/data/2.5/weather?&q=Fria&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 260
    http://api.openweathermap.org/data/2.5/weather?&q=Ely&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 261
    http://api.openweathermap.org/data/2.5/weather?&q=Montes+Claros&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 262
    http://api.openweathermap.org/data/2.5/weather?&q=Mwanza&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 263
    http://api.openweathermap.org/data/2.5/weather?&q=Hamilton&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 264
    http://api.openweathermap.org/data/2.5/weather?&q=Visalia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 265
    http://api.openweathermap.org/data/2.5/weather?&q=Kendari&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 266
    http://api.openweathermap.org/data/2.5/weather?&q=Bartlesville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 267
    http://api.openweathermap.org/data/2.5/weather?&q=Tyler&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 268
    http://api.openweathermap.org/data/2.5/weather?&q=Berekum&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 269
    http://api.openweathermap.org/data/2.5/weather?&q=Caballococha&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 270
    http://api.openweathermap.org/data/2.5/weather?&q=Wetaskiwin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 271
    http://api.openweathermap.org/data/2.5/weather?&q=Kibale&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 272
    http://api.openweathermap.org/data/2.5/weather?&q=Hobart&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 273
    http://api.openweathermap.org/data/2.5/weather?&q=Kokkola&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 274
    http://api.openweathermap.org/data/2.5/weather?&q=Buenos+Aires&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 275
    http://api.openweathermap.org/data/2.5/weather?&q=Boli&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 276
    http://api.openweathermap.org/data/2.5/weather?&q=Kalachinsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 277
    http://api.openweathermap.org/data/2.5/weather?&q=Khabarovsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 278
    http://api.openweathermap.org/data/2.5/weather?&q=Arctic+Bay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 279
    http://api.openweathermap.org/data/2.5/weather?&q=Salluit&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 280
    http://api.openweathermap.org/data/2.5/weather?&q=Leiria&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 281
    http://api.openweathermap.org/data/2.5/weather?&q=28+de+Noviembre&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 282
    http://api.openweathermap.org/data/2.5/weather?&q=Izamal&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 283
    http://api.openweathermap.org/data/2.5/weather?&q=Abeokuta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 284
    http://api.openweathermap.org/data/2.5/weather?&q=Jieshou&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 285
    http://api.openweathermap.org/data/2.5/weather?&q=Nangong&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 286
    http://api.openweathermap.org/data/2.5/weather?&q=Dover&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 287
    http://api.openweathermap.org/data/2.5/weather?&q=Villanueva&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 288
    http://api.openweathermap.org/data/2.5/weather?&q=Kiselevsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 289
    http://api.openweathermap.org/data/2.5/weather?&q=Marrakesh&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 290
    http://api.openweathermap.org/data/2.5/weather?&q=Eldorado&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 291
    http://api.openweathermap.org/data/2.5/weather?&q=Puerto+Acosta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 292
    http://api.openweathermap.org/data/2.5/weather?&q=Mumbai&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 293
    http://api.openweathermap.org/data/2.5/weather?&q=Araxa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 294
    http://api.openweathermap.org/data/2.5/weather?&q=Mercedes&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 295
    http://api.openweathermap.org/data/2.5/weather?&q=Lautoka&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 296
    http://api.openweathermap.org/data/2.5/weather?&q=Al+Hudaydah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 297
    http://api.openweathermap.org/data/2.5/weather?&q=Goteborg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 298
    http://api.openweathermap.org/data/2.5/weather?&q=Maintirano&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 299
    http://api.openweathermap.org/data/2.5/weather?&q=Jacarezinho&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 300
    http://api.openweathermap.org/data/2.5/weather?&q=Cleveland&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 301
    http://api.openweathermap.org/data/2.5/weather?&q=Namanga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 302
    http://api.openweathermap.org/data/2.5/weather?&q=Pingxiang&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 303
    http://api.openweathermap.org/data/2.5/weather?&q=Las+Tablas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 304
    http://api.openweathermap.org/data/2.5/weather?&q=Haapsalu&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 305
    http://api.openweathermap.org/data/2.5/weather?&q=Granada&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 306
    http://api.openweathermap.org/data/2.5/weather?&q=Lezhe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 307
    http://api.openweathermap.org/data/2.5/weather?&q=Akita&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 308
    http://api.openweathermap.org/data/2.5/weather?&q=Klamath+Falls&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 309
    http://api.openweathermap.org/data/2.5/weather?&q=Telsen&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 310
    http://api.openweathermap.org/data/2.5/weather?&q=Crescent+City&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 311
    http://api.openweathermap.org/data/2.5/weather?&q=Angol&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 312
    http://api.openweathermap.org/data/2.5/weather?&q=Pangnirtung&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 313
    http://api.openweathermap.org/data/2.5/weather?&q=Hai+Duong&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 314
    http://api.openweathermap.org/data/2.5/weather?&q=St.+Petersburg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 315
    http://api.openweathermap.org/data/2.5/weather?&q=Iloilo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 316
    http://api.openweathermap.org/data/2.5/weather?&q=Tiruchirappalli&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 317
    http://api.openweathermap.org/data/2.5/weather?&q=Mityana&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 318
    http://api.openweathermap.org/data/2.5/weather?&q=Sao+Luis&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 319
    http://api.openweathermap.org/data/2.5/weather?&q=Mahajanga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 320
    http://api.openweathermap.org/data/2.5/weather?&q=Prachin+Buri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 321
    http://api.openweathermap.org/data/2.5/weather?&q=Beira&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 322
    http://api.openweathermap.org/data/2.5/weather?&q=Willemstad&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 323
    http://api.openweathermap.org/data/2.5/weather?&q=Kristiansand&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 324
    http://api.openweathermap.org/data/2.5/weather?&q=Arua&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 325
    http://api.openweathermap.org/data/2.5/weather?&q=Kirksville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 326
    http://api.openweathermap.org/data/2.5/weather?&q=Tebingtinggi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 327
    http://api.openweathermap.org/data/2.5/weather?&q=Chuxiong&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 328
    http://api.openweathermap.org/data/2.5/weather?&q=Pondicherry&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 329
    http://api.openweathermap.org/data/2.5/weather?&q=Yessey&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 330
    http://api.openweathermap.org/data/2.5/weather?&q=Swansea&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 331
    http://api.openweathermap.org/data/2.5/weather?&q=Cobija&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 332
    http://api.openweathermap.org/data/2.5/weather?&q=Pingtung&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 333
    http://api.openweathermap.org/data/2.5/weather?&q=Turpan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 334
    http://api.openweathermap.org/data/2.5/weather?&q=Kondopoga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 335
    http://api.openweathermap.org/data/2.5/weather?&q=Colon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 336
    http://api.openweathermap.org/data/2.5/weather?&q=Dodoma&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 337
    http://api.openweathermap.org/data/2.5/weather?&q=Kota+Baharu&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 338
    http://api.openweathermap.org/data/2.5/weather?&q=Miri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 339
    http://api.openweathermap.org/data/2.5/weather?&q=Watsa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 340
    http://api.openweathermap.org/data/2.5/weather?&q=Ulladulla&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 341
    http://api.openweathermap.org/data/2.5/weather?&q=Pisco&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 342
    http://api.openweathermap.org/data/2.5/weather?&q=Aguelhok&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 343
    http://api.openweathermap.org/data/2.5/weather?&q=Chitado&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 344
    http://api.openweathermap.org/data/2.5/weather?&q=Sari&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 345
    http://api.openweathermap.org/data/2.5/weather?&q=Antsirabe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 346
    http://api.openweathermap.org/data/2.5/weather?&q=Pakalongan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 347
    http://api.openweathermap.org/data/2.5/weather?&q=Cairns&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 348
    http://api.openweathermap.org/data/2.5/weather?&q=Saginaw&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 349
    http://api.openweathermap.org/data/2.5/weather?&q=Upata&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 350
    http://api.openweathermap.org/data/2.5/weather?&q=Berri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 351
    http://api.openweathermap.org/data/2.5/weather?&q=Biratnagar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 352
    http://api.openweathermap.org/data/2.5/weather?&q=Bayamo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 353
    http://api.openweathermap.org/data/2.5/weather?&q=Oxford&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 354
    http://api.openweathermap.org/data/2.5/weather?&q=Volksrust&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 355
    http://api.openweathermap.org/data/2.5/weather?&q=Izhevsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 356
    http://api.openweathermap.org/data/2.5/weather?&q=Goycay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 357
    http://api.openweathermap.org/data/2.5/weather?&q=Sotouboua&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 358
    http://api.openweathermap.org/data/2.5/weather?&q=Cloncurry&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 359
    http://api.openweathermap.org/data/2.5/weather?&q=Chiclayo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 360
    http://api.openweathermap.org/data/2.5/weather?&q=Augusta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 361
    http://api.openweathermap.org/data/2.5/weather?&q=Bu'aale&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 362
    http://api.openweathermap.org/data/2.5/weather?&q=Mansa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 363
    http://api.openweathermap.org/data/2.5/weather?&q=Pueblo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 364
    http://api.openweathermap.org/data/2.5/weather?&q=Bondo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 365
    http://api.openweathermap.org/data/2.5/weather?&q=Maraba&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 366
    http://api.openweathermap.org/data/2.5/weather?&q=Paraguari&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 367
    http://api.openweathermap.org/data/2.5/weather?&q=Gainesville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 368
    http://api.openweathermap.org/data/2.5/weather?&q=Beipiao&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 369
    http://api.openweathermap.org/data/2.5/weather?&q=Kobuk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 370
    http://api.openweathermap.org/data/2.5/weather?&q=Chongqing&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 371
    http://api.openweathermap.org/data/2.5/weather?&q=Makkah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 372
    http://api.openweathermap.org/data/2.5/weather?&q=Gila+Bend&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 373
    http://api.openweathermap.org/data/2.5/weather?&q=Granada&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 374
    http://api.openweathermap.org/data/2.5/weather?&q=Port-De-Paix&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 375
    http://api.openweathermap.org/data/2.5/weather?&q=Panda&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 376
    http://api.openweathermap.org/data/2.5/weather?&q=Ulyanovsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 377
    http://api.openweathermap.org/data/2.5/weather?&q=Volos&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 378
    http://api.openweathermap.org/data/2.5/weather?&q=Pingdingshan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 379
    http://api.openweathermap.org/data/2.5/weather?&q=Trancas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 380
    http://api.openweathermap.org/data/2.5/weather?&q=Proddatur&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 381
    http://api.openweathermap.org/data/2.5/weather?&q=Willmar&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 382
    http://api.openweathermap.org/data/2.5/weather?&q=San+Felipe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 383
    http://api.openweathermap.org/data/2.5/weather?&q=Mahmud-E+Eraqi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 384
    http://api.openweathermap.org/data/2.5/weather?&q=Pangkalpinang&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 385
    http://api.openweathermap.org/data/2.5/weather?&q=Malmo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 386
    http://api.openweathermap.org/data/2.5/weather?&q=Streaky+Bay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 387
    http://api.openweathermap.org/data/2.5/weather?&q=Broome&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 388
    http://api.openweathermap.org/data/2.5/weather?&q=Itamaraju&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 389
    http://api.openweathermap.org/data/2.5/weather?&q=Inta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 390
    http://api.openweathermap.org/data/2.5/weather?&q=Sabaya&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 391
    http://api.openweathermap.org/data/2.5/weather?&q=Nazret&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 392
    http://api.openweathermap.org/data/2.5/weather?&q=Sovetsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 393
    http://api.openweathermap.org/data/2.5/weather?&q=Lubumbashi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 394
    http://api.openweathermap.org/data/2.5/weather?&q=Nueva+Ocotepeque&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 395
    http://api.openweathermap.org/data/2.5/weather?&q=Caborca&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 396
    http://api.openweathermap.org/data/2.5/weather?&q=Andkhvoy&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 397
    http://api.openweathermap.org/data/2.5/weather?&q=Pokhara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 398
    http://api.openweathermap.org/data/2.5/weather?&q=Joacaba&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 399
    http://api.openweathermap.org/data/2.5/weather?&q=Coeur+d'Alene&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 400
    http://api.openweathermap.org/data/2.5/weather?&q=St.+Petersburg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 401
    http://api.openweathermap.org/data/2.5/weather?&q=Maringa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 402
    http://api.openweathermap.org/data/2.5/weather?&q=Allentown&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 403
    http://api.openweathermap.org/data/2.5/weather?&q=Paulo+Afonso&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 404
    http://api.openweathermap.org/data/2.5/weather?&q=Patna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 405
    http://api.openweathermap.org/data/2.5/weather?&q=Male&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 406
    http://api.openweathermap.org/data/2.5/weather?&q=Antigua+Guatemala&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 407
    http://api.openweathermap.org/data/2.5/weather?&q=George&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 408
    http://api.openweathermap.org/data/2.5/weather?&q=Murmansk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 409
    http://api.openweathermap.org/data/2.5/weather?&q=Awasa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 410
    http://api.openweathermap.org/data/2.5/weather?&q=Yulara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 411
    http://api.openweathermap.org/data/2.5/weather?&q=Mekoryuk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 412
    http://api.openweathermap.org/data/2.5/weather?&q=Cabo+Frio&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 413
    http://api.openweathermap.org/data/2.5/weather?&q=Balcarce&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 414
    http://api.openweathermap.org/data/2.5/weather?&q=Koyuk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 415
    http://api.openweathermap.org/data/2.5/weather?&q=Paro&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 416
    http://api.openweathermap.org/data/2.5/weather?&q=Antwerpen&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 417
    http://api.openweathermap.org/data/2.5/weather?&q=Encarnacion&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 418
    http://api.openweathermap.org/data/2.5/weather?&q=Tlaxcala&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 419
    http://api.openweathermap.org/data/2.5/weather?&q=Agen&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 420
    http://api.openweathermap.org/data/2.5/weather?&q=Chalatenango&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 421
    http://api.openweathermap.org/data/2.5/weather?&q=Saint-Louis&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 422
    http://api.openweathermap.org/data/2.5/weather?&q=Basel&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 423
    http://api.openweathermap.org/data/2.5/weather?&q=Mesa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 424
    http://api.openweathermap.org/data/2.5/weather?&q=Charleville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 425
    http://api.openweathermap.org/data/2.5/weather?&q=Sheboygan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 426
    http://api.openweathermap.org/data/2.5/weather?&q=San+Cristobal&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 427
    http://api.openweathermap.org/data/2.5/weather?&q=Villa+Union&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 428
    http://api.openweathermap.org/data/2.5/weather?&q=McAllen&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 429
    http://api.openweathermap.org/data/2.5/weather?&q=Eldikan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 430
    http://api.openweathermap.org/data/2.5/weather?&q=Slantsy&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 431
    http://api.openweathermap.org/data/2.5/weather?&q=Veracruz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 432
    http://api.openweathermap.org/data/2.5/weather?&q=Baerum&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 433
    http://api.openweathermap.org/data/2.5/weather?&q=Mehtar+Lam&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 434
    http://api.openweathermap.org/data/2.5/weather?&q=Moanda&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 435
    http://api.openweathermap.org/data/2.5/weather?&q=Ponte+Nova&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 436
    http://api.openweathermap.org/data/2.5/weather?&q=Fort-de-France&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 437
    http://api.openweathermap.org/data/2.5/weather?&q=Timisoara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 438
    http://api.openweathermap.org/data/2.5/weather?&q=Zinder&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 439
    http://api.openweathermap.org/data/2.5/weather?&q=Norseman&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 440
    http://api.openweathermap.org/data/2.5/weather?&q=Parepare&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 441
    http://api.openweathermap.org/data/2.5/weather?&q=Malacca&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 442
    http://api.openweathermap.org/data/2.5/weather?&q=Castillos&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 443
    http://api.openweathermap.org/data/2.5/weather?&q=Porto+Uniao&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 444
    http://api.openweathermap.org/data/2.5/weather?&q=Angoche&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 445
    http://api.openweathermap.org/data/2.5/weather?&q=Tenosique&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 446
    http://api.openweathermap.org/data/2.5/weather?&q=Cienaga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 447
    http://api.openweathermap.org/data/2.5/weather?&q=Itapecuru+Mirim&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 448
    http://api.openweathermap.org/data/2.5/weather?&q=Glasgow&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 449
    http://api.openweathermap.org/data/2.5/weather?&q=Ed+Dueim&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 450
    http://api.openweathermap.org/data/2.5/weather?&q=Cancun&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 451
    http://api.openweathermap.org/data/2.5/weather?&q=Kakinada&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 452
    http://api.openweathermap.org/data/2.5/weather?&q=Mogadishu&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 453
    http://api.openweathermap.org/data/2.5/weather?&q=Kampong+Thum&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 454
    http://api.openweathermap.org/data/2.5/weather?&q=Kouroussa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 455
    http://api.openweathermap.org/data/2.5/weather?&q=Warri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 456
    http://api.openweathermap.org/data/2.5/weather?&q=Chon+Buri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 457
    http://api.openweathermap.org/data/2.5/weather?&q=Hania&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 458
    http://api.openweathermap.org/data/2.5/weather?&q=Fox+Bay&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 459
    http://api.openweathermap.org/data/2.5/weather?&q=Voi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 460
    http://api.openweathermap.org/data/2.5/weather?&q=Nuevo+Laredo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 461
    http://api.openweathermap.org/data/2.5/weather?&q=Bareilly&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 462
    http://api.openweathermap.org/data/2.5/weather?&q=Manukau&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 463
    http://api.openweathermap.org/data/2.5/weather?&q=Kremenchuk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 464
    http://api.openweathermap.org/data/2.5/weather?&q=San+Luis+Potosi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 465
    http://api.openweathermap.org/data/2.5/weather?&q=Isparta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 466
    http://api.openweathermap.org/data/2.5/weather?&q=Vyska&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 467
    http://api.openweathermap.org/data/2.5/weather?&q=Cleburne&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 468
    http://api.openweathermap.org/data/2.5/weather?&q=Kilis&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 469
    http://api.openweathermap.org/data/2.5/weather?&q=Lagos+de+Moreno&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 470
    http://api.openweathermap.org/data/2.5/weather?&q=Como&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 471
    http://api.openweathermap.org/data/2.5/weather?&q=Tmassah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 472
    http://api.openweathermap.org/data/2.5/weather?&q=Shaoguan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 473
    http://api.openweathermap.org/data/2.5/weather?&q=Guymon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 474
    http://api.openweathermap.org/data/2.5/weather?&q=Leon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 475
    http://api.openweathermap.org/data/2.5/weather?&q=Artashat&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 476
    http://api.openweathermap.org/data/2.5/weather?&q=Talara&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 477
    http://api.openweathermap.org/data/2.5/weather?&q=New+Iberia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 478
    http://api.openweathermap.org/data/2.5/weather?&q=Tazovskiy&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 479
    http://api.openweathermap.org/data/2.5/weather?&q=Vila+Velha&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 480
    http://api.openweathermap.org/data/2.5/weather?&q=Feira+de+Santana&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 481
    http://api.openweathermap.org/data/2.5/weather?&q=Oturkpo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 482
    http://api.openweathermap.org/data/2.5/weather?&q=Le+Mans&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 483
    http://api.openweathermap.org/data/2.5/weather?&q=Gulu&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 484
    http://api.openweathermap.org/data/2.5/weather?&q=Hindupur&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 485
    http://api.openweathermap.org/data/2.5/weather?&q=Puerto+Suarez&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 486
    http://api.openweathermap.org/data/2.5/weather?&q=Mpwapwa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 487
    http://api.openweathermap.org/data/2.5/weather?&q=Ubomba&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 488
    http://api.openweathermap.org/data/2.5/weather?&q=Jonesboro&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 489
    http://api.openweathermap.org/data/2.5/weather?&q=South+Bend&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 490
    http://api.openweathermap.org/data/2.5/weather?&q=Maladzyechna&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 491
    http://api.openweathermap.org/data/2.5/weather?&q=Matagami&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 492
    http://api.openweathermap.org/data/2.5/weather?&q=Kambove&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 493
    http://api.openweathermap.org/data/2.5/weather?&q=Poznan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 494
    http://api.openweathermap.org/data/2.5/weather?&q=Ciudad+Obregon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 495
    http://api.openweathermap.org/data/2.5/weather?&q=Mariestad&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 496
    http://api.openweathermap.org/data/2.5/weather?&q=Velsk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 497
    http://api.openweathermap.org/data/2.5/weather?&q=Nuevo+Casas+Grandes&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 498
    http://api.openweathermap.org/data/2.5/weather?&q=Santiago+de+Compostela&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 499
    http://api.openweathermap.org/data/2.5/weather?&q=Hargeysa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 500
    http://api.openweathermap.org/data/2.5/weather?&q=Flagstaff&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 501
    http://api.openweathermap.org/data/2.5/weather?&q=Burnie&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 502
    http://api.openweathermap.org/data/2.5/weather?&q=Charleroi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 503
    http://api.openweathermap.org/data/2.5/weather?&q=Salinas&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 504
    http://api.openweathermap.org/data/2.5/weather?&q=Niquelandia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 505
    http://api.openweathermap.org/data/2.5/weather?&q=Gallup&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 506
    http://api.openweathermap.org/data/2.5/weather?&q=Biarritz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 507
    http://api.openweathermap.org/data/2.5/weather?&q=Novocherkassk&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 508
    http://api.openweathermap.org/data/2.5/weather?&q=Cucuta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 509
    http://api.openweathermap.org/data/2.5/weather?&q=Nova+Cruz&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 510
    http://api.openweathermap.org/data/2.5/weather?&q=Junin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 511
    http://api.openweathermap.org/data/2.5/weather?&q=Merredin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 512
    http://api.openweathermap.org/data/2.5/weather?&q=Mulanje&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 513
    http://api.openweathermap.org/data/2.5/weather?&q=Mbanza-Congo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 514
    http://api.openweathermap.org/data/2.5/weather?&q=Outjo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 515
    http://api.openweathermap.org/data/2.5/weather?&q=Apatity&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 516
    http://api.openweathermap.org/data/2.5/weather?&q=Bogotol&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 517
    http://api.openweathermap.org/data/2.5/weather?&q=Cholpon+Ata&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 518
    http://api.openweathermap.org/data/2.5/weather?&q=Barletta&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 519
    http://api.openweathermap.org/data/2.5/weather?&q=Brussels&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 520
    http://api.openweathermap.org/data/2.5/weather?&q=Viet+Tri&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 521
    http://api.openweathermap.org/data/2.5/weather?&q=Nelson&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 522
    http://api.openweathermap.org/data/2.5/weather?&q=Hassi+Messaoud&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 523
    http://api.openweathermap.org/data/2.5/weather?&q=Yuma&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 524
    http://api.openweathermap.org/data/2.5/weather?&q=Chiquimula&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 525
    http://api.openweathermap.org/data/2.5/weather?&q=Bati&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 526
    http://api.openweathermap.org/data/2.5/weather?&q=Recife&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 527
    http://api.openweathermap.org/data/2.5/weather?&q=Port+Lincoln&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 528
    http://api.openweathermap.org/data/2.5/weather?&q=Rinconada&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 529
    http://api.openweathermap.org/data/2.5/weather?&q=Sanniquellie&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 530
    http://api.openweathermap.org/data/2.5/weather?&q=At+Tafilah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 531
    http://api.openweathermap.org/data/2.5/weather?&q=Rustavi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 532
    http://api.openweathermap.org/data/2.5/weather?&q=Benxi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 533
    http://api.openweathermap.org/data/2.5/weather?&q=Gisenyi&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 534
    http://api.openweathermap.org/data/2.5/weather?&q=Ngaoundere&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 535
    http://api.openweathermap.org/data/2.5/weather?&q=Naberezhnyye+Chelny&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 536
    http://api.openweathermap.org/data/2.5/weather?&q=Sotik&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 537
    http://api.openweathermap.org/data/2.5/weather?&q=Chos+Malal&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 538
    http://api.openweathermap.org/data/2.5/weather?&q=Jerusalem&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 539
    http://api.openweathermap.org/data/2.5/weather?&q=Durban&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 540
    http://api.openweathermap.org/data/2.5/weather?&q=Gbadolite&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 541
    http://api.openweathermap.org/data/2.5/weather?&q=Masan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 542
    http://api.openweathermap.org/data/2.5/weather?&q=Villa+Hayes&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 543
    http://api.openweathermap.org/data/2.5/weather?&q=Aranyaprathet&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 544
    http://api.openweathermap.org/data/2.5/weather?&q=La+Serena&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 545
    http://api.openweathermap.org/data/2.5/weather?&q=Canton&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 546
    http://api.openweathermap.org/data/2.5/weather?&q=Salamanca&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 547
    http://api.openweathermap.org/data/2.5/weather?&q=Haarlem&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 548
    http://api.openweathermap.org/data/2.5/weather?&q=As+Sulayyil&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 549
    http://api.openweathermap.org/data/2.5/weather?&q=Taizhou&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 550
    http://api.openweathermap.org/data/2.5/weather?&q=Cranbrook&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 551
    http://api.openweathermap.org/data/2.5/weather?&q=Merauke&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 552
    http://api.openweathermap.org/data/2.5/weather?&q=Juticalpa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 553
    http://api.openweathermap.org/data/2.5/weather?&q=Jimani&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 554
    http://api.openweathermap.org/data/2.5/weather?&q=Ilorin&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 555
    http://api.openweathermap.org/data/2.5/weather?&q=Milan&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 556
    http://api.openweathermap.org/data/2.5/weather?&q=Adelaide&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 557
    http://api.openweathermap.org/data/2.5/weather?&q=Rochester&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 558
    http://api.openweathermap.org/data/2.5/weather?&q=Hirosaki&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 559
    http://api.openweathermap.org/data/2.5/weather?&q=La+Ligua&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 560
    http://api.openweathermap.org/data/2.5/weather?&q=Nagano&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 561
    http://api.openweathermap.org/data/2.5/weather?&q=Evora&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 562
    http://api.openweathermap.org/data/2.5/weather?&q=Urubamba&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 563
    http://api.openweathermap.org/data/2.5/weather?&q=Huajuapan+de+Leon&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 564
    http://api.openweathermap.org/data/2.5/weather?&q=Hopkinsville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 565
    http://api.openweathermap.org/data/2.5/weather?&q=Seville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 566
    http://api.openweathermap.org/data/2.5/weather?&q=Petropavlovsk+Kamchatskiy&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 567
    http://api.openweathermap.org/data/2.5/weather?&q=Ras+al+Khaymah&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 568
    http://api.openweathermap.org/data/2.5/weather?&q=Kaesong&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 569
    http://api.openweathermap.org/data/2.5/weather?&q=Odessa&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 570
    http://api.openweathermap.org/data/2.5/weather?&q=Galesburg&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 571
    http://api.openweathermap.org/data/2.5/weather?&q=Phitsanulok&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 572
    http://api.openweathermap.org/data/2.5/weather?&q=Jau&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 573
    http://api.openweathermap.org/data/2.5/weather?&q=Kiama&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 574
    http://api.openweathermap.org/data/2.5/weather?&q=Marilia&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 575
    http://api.openweathermap.org/data/2.5/weather?&q=Sagastyr&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 576
    http://api.openweathermap.org/data/2.5/weather?&q=Kosti&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 577
    http://api.openweathermap.org/data/2.5/weather?&q=Ndjamena&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 578
    http://api.openweathermap.org/data/2.5/weather?&q=Erechim&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 579
    http://api.openweathermap.org/data/2.5/weather?&q=Kavieng&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 580
    http://api.openweathermap.org/data/2.5/weather?&q=Cruzeiro+do+Sul&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 581
    http://api.openweathermap.org/data/2.5/weather?&q=Sasovo&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 582
    http://api.openweathermap.org/data/2.5/weather?&q=Mary&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 583
    http://api.openweathermap.org/data/2.5/weather?&q=Koriyama&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 584
    http://api.openweathermap.org/data/2.5/weather?&q=Ishim&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 585
    http://api.openweathermap.org/data/2.5/weather?&q=Pittsfield&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 586
    http://api.openweathermap.org/data/2.5/weather?&q=Baraki+Barak&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 587
    http://api.openweathermap.org/data/2.5/weather?&q=Dalian&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 588
    http://api.openweathermap.org/data/2.5/weather?&q=Catania&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 589
    http://api.openweathermap.org/data/2.5/weather?&q=Danville&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 590
    http://api.openweathermap.org/data/2.5/weather?&q=Salum&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 591
    http://api.openweathermap.org/data/2.5/weather?&q=Yining&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 592
    http://api.openweathermap.org/data/2.5/weather?&q=Zumpango&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 593
    http://api.openweathermap.org/data/2.5/weather?&q=Frutal&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 594
    http://api.openweathermap.org/data/2.5/weather?&q=Pirassununga&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 595
    http://api.openweathermap.org/data/2.5/weather?&q=Puerto+Escondido&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 596
    http://api.openweathermap.org/data/2.5/weather?&q=Mazowe&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 597
    http://api.openweathermap.org/data/2.5/weather?&q=Simao&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Now retrieving city # 598
    http://api.openweathermap.org/data/2.5/weather?&q=Arqalyq&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    Error with city data. Skipping
    Now retrieving city # 599
    http://api.openweathermap.org/data/2.5/weather?&q=Syracuse&units=imperial&appid=5a1900b8b6b43ecdc7d4067c67fe076e
    


```python
#Check to make sure data populated each correctly
cities_cloud
```




    [90,
     20,
     0,
     40,
     88,
     1,
     36,
     0,
     75,
     40,
     0,
     68,
     0,
     90,
     1,
     0,
     40,
     20,
     90,
     90,
     68,
     0,
     92,
     88,
     76,
     75,
     0,
     75,
     90,
     75,
     0,
     20,
     36,
     0,
     56,
     75,
     0,
     0,
     56,
     75,
     56,
     32,
     44,
     12,
     75,
     68,
     20,
     0,
     1,
     92,
     90,
     90,
     90,
     68,
     68,
     64,
     20,
     75,
     24,
     75,
     100,
     40,
     0,
     32,
     92,
     0,
     90,
     48,
     0,
     20,
     8,
     75,
     0,
     75,
     80,
     76,
     0,
     92,
     1,
     90,
     75,
     12,
     1,
     56,
     0,
     75,
     0,
     0,
     75,
     20,
     48,
     44,
     88,
     88,
     0,
     1,
     12,
     0,
     20,
     76,
     8,
     40,
     0,
     75,
     88,
     92,
     75,
     76,
     0,
     0,
     92,
     8,
     20,
     90,
     40,
     0,
     20,
     36,
     56,
     24,
     0,
     90,
     0,
     1,
     90,
     20,
     88,
     90,
     92,
     1,
     90,
     90,
     0,
     0,
     0,
     75,
     75,
     36,
     0,
     88,
     40,
     88,
     1,
     75,
     12,
     0,
     0,
     20,
     75,
     64,
     68,
     48,
     88,
     1,
     88,
     0,
     0,
     20,
     0,
     20,
     20,
     90,
     92,
     20,
     56,
     90,
     1,
     0,
     75,
     90,
     75,
     48,
     92,
     0,
     75,
     36,
     75,
     12,
     24,
     0,
     1,
     0,
     20,
     1,
     90,
     20,
     48,
     76,
     75,
     0,
     75,
     75,
     1,
     75,
     0,
     90,
     48,
     1,
     90,
     20,
     80,
     0,
     1,
     88,
     75,
     1,
     64,
     92,
     76,
     20,
     0,
     90,
     0,
     0,
     0,
     8,
     0,
     76,
     64,
     0,
     92,
     1,
     75,
     0,
     75,
     90,
     20,
     90,
     1,
     56,
     1,
     1,
     0,
     75,
     64,
     20,
     0,
     68,
     40,
     48,
     40,
     75,
     75,
     0,
     64,
     0,
     1,
     75,
     68,
     75,
     0,
     0,
     75,
     0,
     40,
     0,
     80,
     64,
     90,
     48,
     0,
     64,
     92,
     75,
     75,
     75,
     1,
     1,
     0,
     20,
     75,
     92,
     20,
     0,
     20,
     68,
     40,
     20,
     48,
     0,
     90,
     20,
     0,
     8,
     75,
     75,
     48,
     92,
     40,
     8,
     75,
     0,
     75,
     20,
     68,
     88,
     20,
     90,
     88,
     0,
     0,
     90,
     0,
     0,
     80,
     0,
     0,
     0,
     1,
     92,
     1,
     20,
     12,
     0,
     1,
     0,
     88,
     1,
     75,
     90,
     0,
     44,
     64,
     0,
     1,
     0,
     48,
     1,
     0,
     40,
     80,
     68,
     0,
     92,
     88,
     0,
     0,
     0,
     0,
     40,
     32,
     0,
     20,
     75,
     92,
     75,
     0,
     90,
     75,
     0,
     0,
     92,
     0,
     40,
     75,
     0,
     0,
     75,
     75,
     0,
     75,
     32,
     90,
     1,
     56,
     75,
     75,
     48,
     92,
     0,
     0,
     0,
     0,
     0,
     8,
     0,
     0,
     0,
     75,
     0,
     8,
     0,
     0,
     20,
     20,
     48,
     1,
     92,
     88,
     20,
     40,
     1,
     20,
     90,
     75,
     0,
     1,
     90,
     0,
     0,
     1,
     68,
     75,
     76,
     75,
     0,
     0,
     92,
     1,
     40,
     36,
     90,
     92,
     0,
     20,
     80,
     88,
     44,
     40,
     0,
     1,
     0,
     75,
     1,
     92,
     1,
     92,
     20,
     44,
     40,
     0,
     0,
     88,
     92,
     80,
     88,
     0,
     75,
     75,
     8,
     0,
     20,
     24,
     0,
     20,
     24,
     75,
     0,
     0,
     0,
     0,
     8,
     24,
     75,
     12,
     8,
     0,
     92,
     36,
     0,
     0,
     12,
     90,
     90,
     0,
     75,
     0,
     20,
     90,
     8,
     40,
     40,
     12,
     1,
     0,
     90,
     75,
     90,
     0,
     80,
     0,
     1,
     75,
     20,
     0,
     0,
     90,
     88,
     40,
     75,
     92,
     0,
     0,
     20,
     92,
     0,
     92,
     5,
     75,
     92,
     1,
     12,
     40,
     0,
     1,
     0,
     0,
     75,
     75,
     0,
     90]




```python
# Extract interesting data from responses
# city_data = [data.get("name") for data in weather_data]
# cloud_data = [data.get("clouds").get("all") for data in weather_data]
# country_data = [data.get("sys").get("country") for data in weather_data]
# date_data = [data.get("dt") for data in weather_data]
# humid_data = [data.get("main").get("humidity") for data in weather_data]
# lat_data = [data.get("coord").get("lat") for data in weather_data]
# lon_data = [data.get("coord").get("lon") for data in weather_data]
# maxtemp_data = [data.get("main").get("temp_max") for data in weather_data]
# wind_data = [data.get("wind").get("speed") for data in weather_data]


weather_data = {"City": cities_name, "Cloudiness": cities_cloud, "Country": cities_country, "Date": cities_date, 
                "Humidity": cities_humid, "Lat": cities_lat, "Lon": cities_lon, "Max Temp": cities_temp, "Wind Speed": cities_wind}
weather_data = pd.DataFrame(weather_data)
weather = weather_data.sample(n=500)
weather.head()
weather_data.to_csv("WeatherAPI")
```


```python
sns.set()
plt.figure(figsize=(10,7))
# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Max Temp"], c="mediumblue", marker="o", edgecolors="black")

# Incorporate the other graph properties
plt.title("City Latitude vs Max Temperature (12/11/17)", fontsize=15)
plt.ylabel("Max Temperature (F)", fontsize=13)
plt.xlabel("Latitude", fontsize=13)
plt.grid(True)
plt.xticks(size = 13)
plt.yticks(size = 13)
plt.xlim(-80, 100)
plt.ylim(-100, 150)

# Save the figure
plt.savefig("City Latitude vs Max Temperature (12-11-17).png")

# Show plot
plt.show()
```


![png](output_7_0.png)



```python
sns.set()
plt.figure(figsize=(10,7))
# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Humidity"], c="mediumblue", marker="o", edgecolors="black")

# Incorporate the other graph properties
plt.title("City Latitude vs Humidity (12/11/17)", fontsize=15)
plt.ylabel("Humidity (%)", fontsize=13)
plt.xlabel("Latitude", fontsize=13)
plt.grid(True)
plt.xticks(size = 13)
plt.yticks(size = 13)
plt.xlim(-80, 100)
plt.ylim(-20, 120)

# Save the figure
plt.savefig("City Latitude vs Humidity (12-11-17).png")

# Show plot
plt.show()
```


![png](output_8_0.png)



```python
sns.set()
plt.figure(figsize=(10,7))

# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Cloudiness"], c="mediumblue", marker="o", edgecolors="black")

# Incorporate the other graph properties
plt.title("City Latitude vs Cloudiness (12/11/17)",  fontsize=15)
plt.ylabel("Cloudiness (%)",  fontsize=13)
plt.xlabel("Latitude",  fontsize=13)
plt.grid(True)
plt.xticks(size = 13)
plt.yticks(size = 13)
plt.xlim(-80, 100)
plt.ylim(-20, 120)

# Save the figure
plt.savefig("City Latitude vs Cloudiness (12-11-17).png")

# Show plot
plt.show()
```


![png](output_9_0.png)



```python
sns.set()
plt.figure(figsize=(10,7))

# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Wind Speed"], c="mediumblue", marker="o", edgecolors="black")

# Incorporate the other graph properties
plt.title("City Latitude vs Wind Speed (12/11/17)", fontsize=15)
plt.ylabel("Wind Speed (mph)",  fontsize=13)
plt.xlabel("Latitude",  fontsize=13)
plt.grid(True)
plt.xticks(size = 13)
plt.yticks(size = 13)
plt.xlim(-80, 100)
plt.ylim(-5, 40)

# Save the figure
plt.savefig("City Latitude vs Wind Speed (12-11-17).png")

# Show plot
plt.show()
```


![png](output_10_0.png)

