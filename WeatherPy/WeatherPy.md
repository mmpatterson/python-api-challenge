
# WeatherPy
----

#### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
from datetime import date

# Import API key
from api_keys import api_key

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Today's date
today = date.today()

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)

# OpenWeather API url
url = "http://api.openweathermap.org/data/2.5/weather?"




```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)

```




    622



### Perform API Calls
* Perform a weather check on each city using a series of successive API calls.
* Include a print log of each city as it'sbeing processed (with the city number and city name).



```python
#Create arrays for each piece of info I need from the API call
names = []
temps = []
winds = []
humids = []
clouds = []
lat = []

for city in cities: 
    # Add + 1 because index starts at 0
    city_number = (cities.index(city)) + 1
    #city = city.capitalize()
    print("---------------------")
    print(f"Processing city #{city_number}: {city.capitalize()}")
    # Adjust multi-word urls so that api will call to correct city
    adjusted_city = city.replace(" ", "%20")
    # Customized url
    query_url = f"{url}appid={api_key}&units=imperial&q={adjusted_city}"
    weather_response = requests.get(query_url)
    weather_json = weather_response.json()
    # Handle key error exception (If city doesn't exist in OpenWeather db)
    try:
        print(f"url: {query_url}")
        temps.append(weather_json['main']['temp_max'])
        names.append(weather_json['name'])
        winds.append(weather_json['wind']['speed'])
        humids.append(weather_json['main']['humidity'])
        clouds.append(weather_json['clouds']['all'])
        lat.append(weather_json['coord']['lat'])
    except KeyError:
        print(f"{city.capitalize()} doesn't exist at OpenWeather API")
    

    print(f"Finished processing {city.capitalize()}")
    # 1 call per second
    time.sleep(1)

```

    ---------------------
    Processing city #1: Ituni
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ituni
    Ituni doesn't exist at OpenWeather API
    Finished processing Ituni
    ---------------------
    Processing city #2: Mariinsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mariinsk
    Finished processing Mariinsk
    ---------------------
    Processing city #3: Chokurdakh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=chokurdakh
    Finished processing Chokurdakh
    ---------------------
    Processing city #4: Voloshka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=voloshka
    Finished processing Voloshka
    ---------------------
    Processing city #5: Namibe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=namibe
    Finished processing Namibe
    ---------------------
    Processing city #6: Punta arenas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=punta%20arenas
    Finished processing Punta arenas
    ---------------------
    Processing city #7: Watertown
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=watertown
    Finished processing Watertown
    ---------------------
    Processing city #8: Severo-kurilsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=severo-kurilsk
    Finished processing Severo-kurilsk
    ---------------------
    Processing city #9: Butaritari
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=butaritari
    Finished processing Butaritari
    ---------------------
    Processing city #10: Ribeira grande
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ribeira%20grande
    Finished processing Ribeira grande
    ---------------------
    Processing city #11: Lithakia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lithakia
    Finished processing Lithakia
    ---------------------
    Processing city #12: Plettenberg bay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=plettenberg%20bay
    Finished processing Plettenberg bay
    ---------------------
    Processing city #13: Laval
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=laval
    Finished processing Laval
    ---------------------
    Processing city #14: Lerwick
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lerwick
    Finished processing Lerwick
    ---------------------
    Processing city #15: Mataura
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mataura
    Finished processing Mataura
    ---------------------
    Processing city #16: Hisua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hisua
    Finished processing Hisua
    ---------------------
    Processing city #17: East london
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=east%20london
    Finished processing East london
    ---------------------
    Processing city #18: Saint-philippe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint-philippe
    Finished processing Saint-philippe
    ---------------------
    Processing city #19: Auki
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=auki
    Finished processing Auki
    ---------------------
    Processing city #20: Naze
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=naze
    Finished processing Naze
    ---------------------
    Processing city #21: Goderich
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=goderich
    Finished processing Goderich
    ---------------------
    Processing city #22: Nikolskoye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nikolskoye
    Finished processing Nikolskoye
    ---------------------
    Processing city #23: Hobart
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hobart
    Finished processing Hobart
    ---------------------
    Processing city #24: Husavik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=husavik
    Finished processing Husavik
    ---------------------
    Processing city #25: Busselton
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=busselton
    Finished processing Busselton
    ---------------------
    Processing city #26: Ushuaia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ushuaia
    Finished processing Ushuaia
    ---------------------
    Processing city #27: Saskylakh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saskylakh
    Finished processing Saskylakh
    ---------------------
    Processing city #28: Kodiak
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kodiak
    Finished processing Kodiak
    ---------------------
    Processing city #29: Taolanaro
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=taolanaro
    Taolanaro doesn't exist at OpenWeather API
    Finished processing Taolanaro
    ---------------------
    Processing city #30: Castro
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=castro
    Finished processing Castro
    ---------------------
    Processing city #31: Tuktoyaktuk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tuktoyaktuk
    Finished processing Tuktoyaktuk
    ---------------------
    Processing city #32: Sao miguel do araguaia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sao%20miguel%20do%20araguaia
    Finished processing Sao miguel do araguaia
    ---------------------
    Processing city #33: Tiksi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tiksi
    Finished processing Tiksi
    ---------------------
    Processing city #34: Jamestown
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=jamestown
    Finished processing Jamestown
    ---------------------
    Processing city #35: Rikitea
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rikitea
    Finished processing Rikitea
    ---------------------
    Processing city #36: Clyde river
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=clyde%20river
    Finished processing Clyde river
    ---------------------
    Processing city #37: Dubbo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dubbo
    Finished processing Dubbo
    ---------------------
    Processing city #38: Mar del plata
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mar%20del%20plata
    Finished processing Mar del plata
    ---------------------
    Processing city #39: Chiredzi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=chiredzi
    Finished processing Chiredzi
    ---------------------
    Processing city #40: Kloulklubed
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kloulklubed
    Finished processing Kloulklubed
    ---------------------
    Processing city #41: Illoqqortoormiut
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=illoqqortoormiut
    Illoqqortoormiut doesn't exist at OpenWeather API
    Finished processing Illoqqortoormiut
    ---------------------
    Processing city #42: Boulder
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=boulder
    Finished processing Boulder
    ---------------------
    Processing city #43: Samusu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=samusu
    Samusu doesn't exist at OpenWeather API
    Finished processing Samusu
    ---------------------
    Processing city #44: Korla
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=korla
    Korla doesn't exist at OpenWeather API
    Finished processing Korla
    ---------------------
    Processing city #45: Avarua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=avarua
    Finished processing Avarua
    ---------------------
    Processing city #46: Bengkulu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bengkulu
    Bengkulu doesn't exist at OpenWeather API
    Finished processing Bengkulu
    ---------------------
    Processing city #47: Labuan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=labuan
    Finished processing Labuan
    ---------------------
    Processing city #48: Guerrero negro
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=guerrero%20negro
    Finished processing Guerrero negro
    ---------------------
    Processing city #49: Port elizabeth
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20elizabeth
    Finished processing Port elizabeth
    ---------------------
    Processing city #50: Bredasdorp
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bredasdorp
    Finished processing Bredasdorp
    ---------------------
    Processing city #51: Lebu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lebu
    Finished processing Lebu
    ---------------------
    Processing city #52: Lere
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lere
    Finished processing Lere
    ---------------------
    Processing city #53: Maragogi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=maragogi
    Finished processing Maragogi
    ---------------------
    Processing city #54: New norfolk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=new%20norfolk
    Finished processing New norfolk
    ---------------------
    Processing city #55: Vaini
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vaini
    Finished processing Vaini
    ---------------------
    Processing city #56: Cayenne
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cayenne
    Finished processing Cayenne
    ---------------------
    Processing city #57: Marcona
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=marcona
    Marcona doesn't exist at OpenWeather API
    Finished processing Marcona
    ---------------------
    Processing city #58: Tasiilaq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tasiilaq
    Finished processing Tasiilaq
    ---------------------
    Processing city #59: Airai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=airai
    Finished processing Airai
    ---------------------
    Processing city #60: Pisco
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pisco
    Finished processing Pisco
    ---------------------
    Processing city #61: Touros
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=touros
    Finished processing Touros
    ---------------------
    Processing city #62: Hithadhoo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hithadhoo
    Finished processing Hithadhoo
    ---------------------
    Processing city #63: Hambantota
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hambantota
    Finished processing Hambantota
    ---------------------
    Processing city #64: Cabo san lucas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cabo%20san%20lucas
    Finished processing Cabo san lucas
    ---------------------
    Processing city #65: Deputatskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=deputatskiy
    Finished processing Deputatskiy
    ---------------------
    Processing city #66: Guantanamo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=guantanamo
    Finished processing Guantanamo
    ---------------------
    Processing city #67: Hilo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hilo
    Finished processing Hilo
    ---------------------
    Processing city #68: Kapaa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kapaa
    Finished processing Kapaa
    ---------------------
    Processing city #69: Alamosa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=alamosa
    Finished processing Alamosa
    ---------------------
    Processing city #70: Yellowknife
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yellowknife
    Finished processing Yellowknife
    ---------------------
    Processing city #71: Yanchukan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yanchukan
    Yanchukan doesn't exist at OpenWeather API
    Finished processing Yanchukan
    ---------------------
    Processing city #72: Ulladulla
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ulladulla
    Finished processing Ulladulla
    ---------------------
    Processing city #73: Cherskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cherskiy
    Finished processing Cherskiy
    ---------------------
    Processing city #74: Ancud
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ancud
    Finished processing Ancud
    ---------------------
    Processing city #75: Bac lieu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bac%20lieu
    Bac lieu doesn't exist at OpenWeather API
    Finished processing Bac lieu
    ---------------------
    Processing city #76: Rawson
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rawson
    Finished processing Rawson
    ---------------------
    Processing city #77: Mao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mao
    Finished processing Mao
    ---------------------
    Processing city #78: Tuatapere
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tuatapere
    Finished processing Tuatapere
    ---------------------
    Processing city #79: Mahebourg
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mahebourg
    Finished processing Mahebourg
    ---------------------
    Processing city #80: Mokrousovo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mokrousovo
    Finished processing Mokrousovo
    ---------------------
    Processing city #81: Gumdag
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gumdag
    Finished processing Gumdag
    ---------------------
    Processing city #82: Codrington
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=codrington
    Finished processing Codrington
    ---------------------
    Processing city #83: Hvammstangi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hvammstangi
    Hvammstangi doesn't exist at OpenWeather API
    Finished processing Hvammstangi
    ---------------------
    Processing city #84: Regidor
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=regidor
    Finished processing Regidor
    ---------------------
    Processing city #85: Calbuco
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=calbuco
    Finished processing Calbuco
    ---------------------
    Processing city #86: Victoria
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=victoria
    Finished processing Victoria
    ---------------------
    Processing city #87: Qaanaaq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=qaanaaq
    Finished processing Qaanaaq
    ---------------------
    Processing city #88: Buin
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=buin
    Finished processing Buin
    ---------------------
    Processing city #89: Mazagao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mazagao
    Finished processing Mazagao
    ---------------------
    Processing city #90: Arandis
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arandis
    Finished processing Arandis
    ---------------------
    Processing city #91: Kupang
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kupang
    Finished processing Kupang
    ---------------------
    Processing city #92: Dingle
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dingle
    Finished processing Dingle
    ---------------------
    Processing city #93: Grootfontein
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=grootfontein
    Finished processing Grootfontein
    ---------------------
    Processing city #94: Torbay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=torbay
    Finished processing Torbay
    ---------------------
    Processing city #95: San quintin
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20quintin
    Finished processing San quintin
    ---------------------
    Processing city #96: Port hardy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20hardy
    Finished processing Port hardy
    ---------------------
    Processing city #97: Solovetskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=solovetskiy
    Solovetskiy doesn't exist at OpenWeather API
    Finished processing Solovetskiy
    ---------------------
    Processing city #98: Bluff
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bluff
    Finished processing Bluff
    ---------------------
    Processing city #99: Tumannyy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tumannyy
    Tumannyy doesn't exist at OpenWeather API
    Finished processing Tumannyy
    ---------------------
    Processing city #100: Weyburn
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=weyburn
    Finished processing Weyburn
    ---------------------
    Processing city #101: Nabire
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nabire
    Finished processing Nabire
    ---------------------
    Processing city #102: Saldanha
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saldanha
    Finished processing Saldanha
    ---------------------
    Processing city #103: Constitucion
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=constitucion
    Finished processing Constitucion
    ---------------------
    Processing city #104: Los llanos de aridane
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=los%20llanos%20de%20aridane
    Finished processing Los llanos de aridane
    ---------------------
    Processing city #105: Albany
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=albany
    Finished processing Albany
    ---------------------
    Processing city #106: Georgetown
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=georgetown
    Finished processing Georgetown
    ---------------------
    Processing city #107: Lata
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lata
    Finished processing Lata
    ---------------------
    Processing city #108: Khatanga
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=khatanga
    Finished processing Khatanga
    ---------------------
    Processing city #109: Parabel
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=parabel
    Finished processing Parabel
    ---------------------
    Processing city #110: Carnarvon
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=carnarvon
    Finished processing Carnarvon
    ---------------------
    Processing city #111: Norman wells
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=norman%20wells
    Finished processing Norman wells
    ---------------------
    Processing city #112: Cape town
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cape%20town
    Finished processing Cape town
    ---------------------
    Processing city #113: Sioux lookout
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sioux%20lookout
    Finished processing Sioux lookout
    ---------------------
    Processing city #114: Shimoda
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shimoda
    Finished processing Shimoda
    ---------------------
    Processing city #115: Batagay-alyta
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=batagay-alyta
    Finished processing Batagay-alyta
    ---------------------
    Processing city #116: Hofn
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hofn
    Finished processing Hofn
    ---------------------
    Processing city #117: Hermanus
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hermanus
    Finished processing Hermanus
    ---------------------
    Processing city #118: Attawapiskat
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=attawapiskat
    Attawapiskat doesn't exist at OpenWeather API
    Finished processing Attawapiskat
    ---------------------
    Processing city #119: Sitka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sitka
    Finished processing Sitka
    ---------------------
    Processing city #120: Cortez
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cortez
    Finished processing Cortez
    ---------------------
    Processing city #121: Mount isa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mount%20isa
    Finished processing Mount isa
    ---------------------
    Processing city #122: Inhambane
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=inhambane
    Finished processing Inhambane
    ---------------------
    Processing city #123: Luau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=luau
    Finished processing Luau
    ---------------------
    Processing city #124: Mys shmidta
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mys%20shmidta
    Mys shmidta doesn't exist at OpenWeather API
    Finished processing Mys shmidta
    ---------------------
    Processing city #125: Port macquarie
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20macquarie
    Finished processing Port macquarie
    ---------------------
    Processing city #126: Iquitos
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=iquitos
    Finished processing Iquitos
    ---------------------
    Processing city #127: Hella
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hella
    Finished processing Hella
    ---------------------
    Processing city #128: Kruisfontein
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kruisfontein
    Finished processing Kruisfontein
    ---------------------
    Processing city #129: Arraial do cabo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arraial%20do%20cabo
    Finished processing Arraial do cabo
    ---------------------
    Processing city #130: Komsomolskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=komsomolskiy
    Finished processing Komsomolskiy
    ---------------------
    Processing city #131: Saint-georges
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint-georges
    Finished processing Saint-georges
    ---------------------
    Processing city #132: Heinola
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=heinola
    Finished processing Heinola
    ---------------------
    Processing city #133: Saint anthony
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint%20anthony
    Finished processing Saint anthony
    ---------------------
    Processing city #134: Westport
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=westport
    Finished processing Westport
    ---------------------
    Processing city #135: Sur
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sur
    Finished processing Sur
    ---------------------
    Processing city #136: Mantua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mantua
    Finished processing Mantua
    ---------------------
    Processing city #137: Gorontalo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gorontalo
    Finished processing Gorontalo
    ---------------------
    Processing city #138: Kiama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kiama
    Finished processing Kiama
    ---------------------
    Processing city #139: Vaitupu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vaitupu
    Vaitupu doesn't exist at OpenWeather API
    Finished processing Vaitupu
    ---------------------
    Processing city #140: Mount gambier
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mount%20gambier
    Finished processing Mount gambier
    ---------------------
    Processing city #141: Puerto ayora
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puerto%20ayora
    Finished processing Puerto ayora
    ---------------------
    Processing city #142: Port alfred
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20alfred
    Finished processing Port alfred
    ---------------------
    Processing city #143: Kyshtovka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kyshtovka
    Finished processing Kyshtovka
    ---------------------
    Processing city #144: Nantucket
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nantucket
    Finished processing Nantucket
    ---------------------
    Processing city #145: Cam ranh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cam%20ranh
    Finished processing Cam ranh
    ---------------------
    Processing city #146: Turochak
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=turochak
    Finished processing Turochak
    ---------------------
    Processing city #147: Storsteinnes
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=storsteinnes
    Finished processing Storsteinnes
    ---------------------
    Processing city #148: Leeton
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=leeton
    Finished processing Leeton
    ---------------------
    Processing city #149: Krasnovishersk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=krasnovishersk
    Finished processing Krasnovishersk
    ---------------------
    Processing city #150: San patricio
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20patricio
    Finished processing San patricio
    ---------------------
    Processing city #151: Barentsburg
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=barentsburg
    Barentsburg doesn't exist at OpenWeather API
    Finished processing Barentsburg
    ---------------------
    Processing city #152: Esperance
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=esperance
    Finished processing Esperance
    ---------------------
    Processing city #153: Martapura
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=martapura
    Finished processing Martapura
    ---------------------
    Processing city #154: Saint george
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint%20george
    Finished processing Saint george
    ---------------------
    Processing city #155: Laguna
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=laguna
    Finished processing Laguna
    ---------------------
    Processing city #156: Vagur
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vagur
    Finished processing Vagur
    ---------------------
    Processing city #157: Vysokogornyy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vysokogornyy
    Finished processing Vysokogornyy
    ---------------------
    Processing city #158: Bilma
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bilma
    Finished processing Bilma
    ---------------------
    Processing city #159: Wagar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=wagar
    Finished processing Wagar
    ---------------------
    Processing city #160: Nouadhibou
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nouadhibou
    Finished processing Nouadhibou
    ---------------------
    Processing city #161: Okhotsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=okhotsk
    Finished processing Okhotsk
    ---------------------
    Processing city #162: Dikson
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dikson
    Finished processing Dikson
    ---------------------
    Processing city #163: Leshukonskoye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=leshukonskoye
    Finished processing Leshukonskoye
    ---------------------
    Processing city #164: Upernavik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=upernavik
    Finished processing Upernavik
    ---------------------
    Processing city #165: Agogo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=agogo
    Finished processing Agogo
    ---------------------
    Processing city #166: Taseyevo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=taseyevo
    Finished processing Taseyevo
    ---------------------
    Processing city #167: Vanimo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vanimo
    Finished processing Vanimo
    ---------------------
    Processing city #168: Juifang
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=juifang
    Juifang doesn't exist at OpenWeather API
    Finished processing Juifang
    ---------------------
    Processing city #169: Jeremie
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=jeremie
    Finished processing Jeremie
    ---------------------
    Processing city #170: Tucuman
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tucuman
    Finished processing Tucuman
    ---------------------
    Processing city #171: Akyab
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=akyab
    Akyab doesn't exist at OpenWeather API
    Finished processing Akyab
    ---------------------
    Processing city #172: Karratha
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=karratha
    Finished processing Karratha
    ---------------------
    Processing city #173: Egvekinot
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=egvekinot
    Finished processing Egvekinot
    ---------------------
    Processing city #174: Hami
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hami
    Finished processing Hami
    ---------------------
    Processing city #175: Blytheville
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=blytheville
    Finished processing Blytheville
    ---------------------
    Processing city #176: Dolbeau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dolbeau
    Dolbeau doesn't exist at OpenWeather API
    Finished processing Dolbeau
    ---------------------
    Processing city #177: Naryan-mar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=naryan-mar
    Finished processing Naryan-mar
    ---------------------
    Processing city #178: Macheng
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=macheng
    Finished processing Macheng
    ---------------------
    Processing city #179: Weligama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=weligama
    Finished processing Weligama
    ---------------------
    Processing city #180: Noshiro
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=noshiro
    Finished processing Noshiro
    ---------------------
    Processing city #181: Ferkessedougou
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ferkessedougou
    Finished processing Ferkessedougou
    ---------------------
    Processing city #182: Tunxi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tunxi
    Tunxi doesn't exist at OpenWeather API
    Finished processing Tunxi
    ---------------------
    Processing city #183: Lompoc
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lompoc
    Finished processing Lompoc
    ---------------------
    Processing city #184: Kodinsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kodinsk
    Finished processing Kodinsk
    ---------------------
    Processing city #185: Kenai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kenai
    Finished processing Kenai
    ---------------------
    Processing city #186: Pimentel
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pimentel
    Finished processing Pimentel
    ---------------------
    Processing city #187: Talnakh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=talnakh
    Finished processing Talnakh
    ---------------------
    Processing city #188: Butembo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=butembo
    Finished processing Butembo
    ---------------------
    Processing city #189: Verkhnyaya inta
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=verkhnyaya%20inta
    Finished processing Verkhnyaya inta
    ---------------------
    Processing city #190: Kuusamo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kuusamo
    Finished processing Kuusamo
    ---------------------
    Processing city #191: Poim
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=poim
    Finished processing Poim
    ---------------------
    Processing city #192: Sentyabrskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sentyabrskiy
    Sentyabrskiy doesn't exist at OpenWeather API
    Finished processing Sentyabrskiy
    ---------------------
    Processing city #193: San policarpo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20policarpo
    Finished processing San policarpo
    ---------------------
    Processing city #194: Nome
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nome
    Finished processing Nome
    ---------------------
    Processing city #195: San cristobal
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20cristobal
    Finished processing San cristobal
    ---------------------
    Processing city #196: Qaqortoq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=qaqortoq
    Finished processing Qaqortoq
    ---------------------
    Processing city #197: Constantine
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=constantine
    Finished processing Constantine
    ---------------------
    Processing city #198: Didsbury
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=didsbury
    Finished processing Didsbury
    ---------------------
    Processing city #199: Margate
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=margate
    Finished processing Margate
    ---------------------
    Processing city #200: Fort myers beach
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fort%20myers%20beach
    Finished processing Fort myers beach
    ---------------------
    Processing city #201: Hasaki
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hasaki
    Finished processing Hasaki
    ---------------------
    Processing city #202: Coquimbo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=coquimbo
    Finished processing Coquimbo
    ---------------------
    Processing city #203: Louisbourg
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=louisbourg
    Louisbourg doesn't exist at OpenWeather API
    Finished processing Louisbourg
    ---------------------
    Processing city #204: Lethem
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lethem
    Finished processing Lethem
    ---------------------
    Processing city #205: San andres
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20andres
    Finished processing San andres
    ---------------------
    Processing city #206: Richards bay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=richards%20bay
    Finished processing Richards bay
    ---------------------
    Processing city #207: Balkhash
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=balkhash
    Finished processing Balkhash
    ---------------------
    Processing city #208: Flin flon
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=flin%20flon
    Finished processing Flin flon
    ---------------------
    Processing city #209: Charters towers
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=charters%20towers
    Finished processing Charters towers
    ---------------------
    Processing city #210: Kaitangata
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kaitangata
    Finished processing Kaitangata
    ---------------------
    Processing city #211: Moncao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=moncao
    Finished processing Moncao
    ---------------------
    Processing city #212: Yambio
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yambio
    Yambio doesn't exist at OpenWeather API
    Finished processing Yambio
    ---------------------
    Processing city #213: The pas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=the%20pas
    Finished processing The pas
    ---------------------
    Processing city #214: Narsaq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=narsaq
    Finished processing Narsaq
    ---------------------
    Processing city #215: Qiryat shemona
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=qiryat%20shemona
    Finished processing Qiryat shemona
    ---------------------
    Processing city #216: Agadez
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=agadez
    Finished processing Agadez
    ---------------------
    Processing city #217: Buchanan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=buchanan
    Finished processing Buchanan
    ---------------------
    Processing city #218: Khandyga
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=khandyga
    Finished processing Khandyga
    ---------------------
    Processing city #219: Purpe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=purpe
    Finished processing Purpe
    ---------------------
    Processing city #220: Pringsewu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pringsewu
    Finished processing Pringsewu
    ---------------------
    Processing city #221: San jose
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=san%20jose
    Finished processing San jose
    ---------------------
    Processing city #222: Lorengau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lorengau
    Finished processing Lorengau
    ---------------------
    Processing city #223: Puerto leguizamo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puerto%20leguizamo
    Finished processing Puerto leguizamo
    ---------------------
    Processing city #224: Fukue
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fukue
    Finished processing Fukue
    ---------------------
    Processing city #225: Barrow
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=barrow
    Finished processing Barrow
    ---------------------
    Processing city #226: Katherine
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=katherine
    Finished processing Katherine
    ---------------------
    Processing city #227: Stonewall
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=stonewall
    Finished processing Stonewall
    ---------------------
    Processing city #228: Buala
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=buala
    Finished processing Buala
    ---------------------
    Processing city #229: Bambous virieux
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bambous%20virieux
    Finished processing Bambous virieux
    ---------------------
    Processing city #230: Port blair
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20blair
    Finished processing Port blair
    ---------------------
    Processing city #231: Sistranda
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sistranda
    Finished processing Sistranda
    ---------------------
    Processing city #232: Provideniya
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=provideniya
    Finished processing Provideniya
    ---------------------
    Processing city #233: Srednekolymsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=srednekolymsk
    Finished processing Srednekolymsk
    ---------------------
    Processing city #234: Along
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=along
    Finished processing Along
    ---------------------
    Processing city #235: Kushima
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kushima
    Finished processing Kushima
    ---------------------
    Processing city #236: Iqaluit
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=iqaluit
    Finished processing Iqaluit
    ---------------------
    Processing city #237: Novyy urengoy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=novyy%20urengoy
    Finished processing Novyy urengoy
    ---------------------
    Processing city #238: Maniitsoq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=maniitsoq
    Finished processing Maniitsoq
    ---------------------
    Processing city #239: Portland
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=portland
    Finished processing Portland
    ---------------------
    Processing city #240: Atuona
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=atuona
    Finished processing Atuona
    ---------------------
    Processing city #241: Shubarshi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shubarshi
    Finished processing Shubarshi
    ---------------------
    Processing city #242: Belushya guba
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=belushya%20guba
    Belushya guba doesn't exist at OpenWeather API
    Finished processing Belushya guba
    ---------------------
    Processing city #243: Kahului
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kahului
    Finished processing Kahului
    ---------------------
    Processing city #244: Hirschaid
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hirschaid
    Finished processing Hirschaid
    ---------------------
    Processing city #245: Mahajanga
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mahajanga
    Finished processing Mahajanga
    ---------------------
    Processing city #246: Asosa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=asosa
    Finished processing Asosa
    ---------------------
    Processing city #247: Assiros
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=assiros
    Finished processing Assiros
    ---------------------
    Processing city #248: Pesqueira
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pesqueira
    Finished processing Pesqueira
    ---------------------
    Processing city #249: Kavieng
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kavieng
    Finished processing Kavieng
    ---------------------
    Processing city #250: Tsabong
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tsabong
    Finished processing Tsabong
    ---------------------
    Processing city #251: Evensk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=evensk
    Finished processing Evensk
    ---------------------
    Processing city #252: Saint-pierre
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint-pierre
    Finished processing Saint-pierre
    ---------------------
    Processing city #253: Buraydah
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=buraydah
    Finished processing Buraydah
    ---------------------
    Processing city #254: Ashtian
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ashtian
    Finished processing Ashtian
    ---------------------
    Processing city #255: Cuiluan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cuiluan
    Finished processing Cuiluan
    ---------------------
    Processing city #256: Fort nelson
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fort%20nelson
    Finished processing Fort nelson
    ---------------------
    Processing city #257: Faanui
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=faanui
    Finished processing Faanui
    ---------------------
    Processing city #258: Kungurtug
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kungurtug
    Finished processing Kungurtug
    ---------------------
    Processing city #259: Padang
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=padang
    Finished processing Padang
    ---------------------
    Processing city #260: Aklavik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=aklavik
    Finished processing Aklavik
    ---------------------
    Processing city #261: Udachnyy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=udachnyy
    Finished processing Udachnyy
    ---------------------
    Processing city #262: Namatanai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=namatanai
    Finished processing Namatanai
    ---------------------
    Processing city #263: Nyagan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nyagan
    Finished processing Nyagan
    ---------------------
    Processing city #264: Valdivia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=valdivia
    Finished processing Valdivia
    ---------------------
    Processing city #265: Esso
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=esso
    Finished processing Esso
    ---------------------
    Processing city #266: Mlowo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mlowo
    Finished processing Mlowo
    ---------------------
    Processing city #267: Katsuura
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=katsuura
    Finished processing Katsuura
    ---------------------
    Processing city #268: Saint-joseph
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint-joseph
    Finished processing Saint-joseph
    ---------------------
    Processing city #269: Awjilah
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=awjilah
    Finished processing Awjilah
    ---------------------
    Processing city #270: Orocue
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=orocue
    Finished processing Orocue
    ---------------------
    Processing city #271: Taoudenni
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=taoudenni
    Finished processing Taoudenni
    ---------------------
    Processing city #272: Olafsvik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=olafsvik
    Olafsvik doesn't exist at OpenWeather API
    Finished processing Olafsvik
    ---------------------
    Processing city #273: Arcachon
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arcachon
    Finished processing Arcachon
    ---------------------
    Processing city #274: Tateyama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tateyama
    Finished processing Tateyama
    ---------------------
    Processing city #275: Conceicao do araguaia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=conceicao%20do%20araguaia
    Finished processing Conceicao do araguaia
    ---------------------
    Processing city #276: Lavrentiya
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lavrentiya
    Finished processing Lavrentiya
    ---------------------
    Processing city #277: Kerman
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kerman
    Finished processing Kerman
    ---------------------
    Processing city #278: Caravelas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=caravelas
    Finished processing Caravelas
    ---------------------
    Processing city #279: Bandarbeyla
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bandarbeyla
    Finished processing Bandarbeyla
    ---------------------
    Processing city #280: Ambilobe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ambilobe
    Finished processing Ambilobe
    ---------------------
    Processing city #281: Takoradi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=takoradi
    Finished processing Takoradi
    ---------------------
    Processing city #282: Mitreni
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mitreni
    Finished processing Mitreni
    ---------------------
    Processing city #283: Stelle
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=stelle
    Finished processing Stelle
    ---------------------
    Processing city #284: Puerto escondido
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puerto%20escondido
    Finished processing Puerto escondido
    ---------------------
    Processing city #285: Balakhta
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=balakhta
    Finished processing Balakhta
    ---------------------
    Processing city #286: Grand gaube
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=grand%20gaube
    Finished processing Grand gaube
    ---------------------
    Processing city #287: Viamao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=viamao
    Finished processing Viamao
    ---------------------
    Processing city #288: Bolshegrivskoye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bolshegrivskoye
    Bolshegrivskoye doesn't exist at OpenWeather API
    Finished processing Bolshegrivskoye
    ---------------------
    Processing city #289: Chulman
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=chulman
    Finished processing Chulman
    ---------------------
    Processing city #290: Bethel
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bethel
    Finished processing Bethel
    ---------------------
    Processing city #291: Deer lake
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=deer%20lake
    Finished processing Deer lake
    ---------------------
    Processing city #292: Karaul
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=karaul
    Karaul doesn't exist at OpenWeather API
    Finished processing Karaul
    ---------------------
    Processing city #293: Kununurra
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kununurra
    Finished processing Kununurra
    ---------------------
    Processing city #294: Necochea
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=necochea
    Finished processing Necochea
    ---------------------
    Processing city #295: Urumqi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=urumqi
    Urumqi doesn't exist at OpenWeather API
    Finished processing Urumqi
    ---------------------
    Processing city #296: Te anau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=te%20anau
    Finished processing Te anau
    ---------------------
    Processing city #297: Pevek
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pevek
    Finished processing Pevek
    ---------------------
    Processing city #298: Umzimvubu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=umzimvubu
    Umzimvubu doesn't exist at OpenWeather API
    Finished processing Umzimvubu
    ---------------------
    Processing city #299: Sfantu gheorghe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sfantu%20gheorghe
    Finished processing Sfantu gheorghe
    ---------------------
    Processing city #300: Palauig
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=palauig
    Finished processing Palauig
    ---------------------
    Processing city #301: Sovetskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sovetskiy
    Finished processing Sovetskiy
    ---------------------
    Processing city #302: Vardo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vardo
    Finished processing Vardo
    ---------------------
    Processing city #303: Ketchikan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ketchikan
    Finished processing Ketchikan
    ---------------------
    Processing city #304: Faya
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=faya
    Finished processing Faya
    ---------------------
    Processing city #305: Kailua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kailua
    Finished processing Kailua
    ---------------------
    Processing city #306: Abu dhabi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=abu%20dhabi
    Finished processing Abu dhabi
    ---------------------
    Processing city #307: Viedma
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=viedma
    Finished processing Viedma
    ---------------------
    Processing city #308: Mapastepec
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mapastepec
    Finished processing Mapastepec
    ---------------------
    Processing city #309: Pokhara
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pokhara
    Finished processing Pokhara
    ---------------------
    Processing city #310: Ixtapa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ixtapa
    Finished processing Ixtapa
    ---------------------
    Processing city #311: Bolungarvik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bolungarvik
    Bolungarvik doesn't exist at OpenWeather API
    Finished processing Bolungarvik
    ---------------------
    Processing city #312: Poum
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=poum
    Finished processing Poum
    ---------------------
    Processing city #313: Troitsko-pechorsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=troitsko-pechorsk
    Finished processing Troitsko-pechorsk
    ---------------------
    Processing city #314: Ruatoria
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ruatoria
    Ruatoria doesn't exist at OpenWeather API
    Finished processing Ruatoria
    ---------------------
    Processing city #315: Palabuhanratu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=palabuhanratu
    Palabuhanratu doesn't exist at OpenWeather API
    Finished processing Palabuhanratu
    ---------------------
    Processing city #316: Furstenwalde
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=furstenwalde
    Finished processing Furstenwalde
    ---------------------
    Processing city #317: Havre-saint-pierre
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=havre-saint-pierre
    Finished processing Havre-saint-pierre
    ---------------------
    Processing city #318: Thompson
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=thompson
    Finished processing Thompson
    ---------------------
    Processing city #319: Lalmohan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lalmohan
    Finished processing Lalmohan
    ---------------------
    Processing city #320: Toora-khem
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=toora-khem
    Finished processing Toora-khem
    ---------------------
    Processing city #321: Kashi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kashi
    Kashi doesn't exist at OpenWeather API
    Finished processing Kashi
    ---------------------
    Processing city #322: Zuwarah
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=zuwarah
    Finished processing Zuwarah
    ---------------------
    Processing city #323: Hihifo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hihifo
    Hihifo doesn't exist at OpenWeather API
    Finished processing Hihifo
    ---------------------
    Processing city #324: Vestmannaeyjar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vestmannaeyjar
    Finished processing Vestmannaeyjar
    ---------------------
    Processing city #325: Tajumulco
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tajumulco
    Finished processing Tajumulco
    ---------------------
    Processing city #326: Meyungs
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=meyungs
    Meyungs doesn't exist at OpenWeather API
    Finished processing Meyungs
    ---------------------
    Processing city #327: Geraldton
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=geraldton
    Finished processing Geraldton
    ---------------------
    Processing city #328: Kysyl-syr
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kysyl-syr
    Finished processing Kysyl-syr
    ---------------------
    Processing city #329: Tucumcari
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tucumcari
    Finished processing Tucumcari
    ---------------------
    Processing city #330: Vilhena
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vilhena
    Finished processing Vilhena
    ---------------------
    Processing city #331: Yangjiang
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yangjiang
    Finished processing Yangjiang
    ---------------------
    Processing city #332: Barcelos
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=barcelos
    Finished processing Barcelos
    ---------------------
    Processing city #333: Puchezh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puchezh
    Finished processing Puchezh
    ---------------------
    Processing city #334: Tigzirt
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tigzirt
    Finished processing Tigzirt
    ---------------------
    Processing city #335: Saurimo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saurimo
    Finished processing Saurimo
    ---------------------
    Processing city #336: Chuy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=chuy
    Finished processing Chuy
    ---------------------
    Processing city #337: Ternate
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ternate
    Finished processing Ternate
    ---------------------
    Processing city #338: Teguise
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=teguise
    Finished processing Teguise
    ---------------------
    Processing city #339: Santa cruz del norte
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=santa%20cruz%20del%20norte
    Finished processing Santa cruz del norte
    ---------------------
    Processing city #340: Mudgee
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mudgee
    Finished processing Mudgee
    ---------------------
    Processing city #341: Celestun
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=celestun
    Finished processing Celestun
    ---------------------
    Processing city #342: Nizhneyansk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nizhneyansk
    Nizhneyansk doesn't exist at OpenWeather API
    Finished processing Nizhneyansk
    ---------------------
    Processing city #343: Kismayo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kismayo
    Kismayo doesn't exist at OpenWeather API
    Finished processing Kismayo
    ---------------------
    Processing city #344: Szamosszeg
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=szamosszeg
    Finished processing Szamosszeg
    ---------------------
    Processing city #345: Srikakulam
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=srikakulam
    Finished processing Srikakulam
    ---------------------
    Processing city #346: Camacha
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=camacha
    Finished processing Camacha
    ---------------------
    Processing city #347: Vostok
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vostok
    Finished processing Vostok
    ---------------------
    Processing city #348: Grand river south east
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=grand%20river%20south%20east
    Grand river south east doesn't exist at OpenWeather API
    Finished processing Grand river south east
    ---------------------
    Processing city #349: Kandara
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kandara
    Finished processing Kandara
    ---------------------
    Processing city #350: Port lincoln
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=port%20lincoln
    Finished processing Port lincoln
    ---------------------
    Processing city #351: Hokitika
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hokitika
    Finished processing Hokitika
    ---------------------
    Processing city #352: Sola
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sola
    Finished processing Sola
    ---------------------
    Processing city #353: Kuantan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kuantan
    Finished processing Kuantan
    ---------------------
    Processing city #354: Amderma
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=amderma
    Amderma doesn't exist at OpenWeather API
    Finished processing Amderma
    ---------------------
    Processing city #355: Tsihombe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tsihombe
    Tsihombe doesn't exist at OpenWeather API
    Finished processing Tsihombe
    ---------------------
    Processing city #356: Comodoro rivadavia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=comodoro%20rivadavia
    Finished processing Comodoro rivadavia
    ---------------------
    Processing city #357: Tondano
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tondano
    Finished processing Tondano
    ---------------------
    Processing city #358: Yuzhno-kurilsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yuzhno-kurilsk
    Finished processing Yuzhno-kurilsk
    ---------------------
    Processing city #359: Tongzi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tongzi
    Finished processing Tongzi
    ---------------------
    Processing city #360: Sivas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sivas
    Finished processing Sivas
    ---------------------
    Processing city #361: Souillac
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=souillac
    Finished processing Souillac
    ---------------------
    Processing city #362: Itarema
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=itarema
    Finished processing Itarema
    ---------------------
    Processing city #363: Pacific grove
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pacific%20grove
    Finished processing Pacific grove
    ---------------------
    Processing city #364: Inongo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=inongo
    Finished processing Inongo
    ---------------------
    Processing city #365: Gushikawa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gushikawa
    Finished processing Gushikawa
    ---------------------
    Processing city #366: Mackay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mackay
    Finished processing Mackay
    ---------------------
    Processing city #367: Naarden
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=naarden
    Finished processing Naarden
    ---------------------
    Processing city #368: Mahon
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mahon
    Finished processing Mahon
    ---------------------
    Processing city #369: Kribi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kribi
    Finished processing Kribi
    ---------------------
    Processing city #370: Dwarka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dwarka
    Finished processing Dwarka
    ---------------------
    Processing city #371: George town
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=george%20town
    Finished processing George town
    ---------------------
    Processing city #372: Svetlaya
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=svetlaya
    Finished processing Svetlaya
    ---------------------
    Processing city #373: Yar-sale
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yar-sale
    Finished processing Yar-sale
    ---------------------
    Processing city #374: Laranjeiras do sul
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=laranjeiras%20do%20sul
    Finished processing Laranjeiras do sul
    ---------------------
    Processing city #375: Otradnoye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=otradnoye
    Finished processing Otradnoye
    ---------------------
    Processing city #376: Brigantine
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=brigantine
    Finished processing Brigantine
    ---------------------
    Processing city #377: Ozernovskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ozernovskiy
    Finished processing Ozernovskiy
    ---------------------
    Processing city #378: Kon tum
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kon%20tum
    Finished processing Kon tum
    ---------------------
    Processing city #379: Sao joao da barra
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sao%20joao%20da%20barra
    Finished processing Sao joao da barra
    ---------------------
    Processing city #380: Rio gallegos
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rio%20gallegos
    Finished processing Rio gallegos
    ---------------------
    Processing city #381: Severnyy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=severnyy
    Severnyy doesn't exist at OpenWeather API
    Finished processing Severnyy
    ---------------------
    Processing city #382: Edd
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=edd
    Finished processing Edd
    ---------------------
    Processing city #383: Finschhafen
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=finschhafen
    Finished processing Finschhafen
    ---------------------
    Processing city #384: Novopokrovka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=novopokrovka
    Finished processing Novopokrovka
    ---------------------
    Processing city #385: Pyinmana
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pyinmana
    Finished processing Pyinmana
    ---------------------
    Processing city #386: Meiktila
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=meiktila
    Finished processing Meiktila
    ---------------------
    Processing city #387: Stillwater
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=stillwater
    Finished processing Stillwater
    ---------------------
    Processing city #388: Portsmouth
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=portsmouth
    Finished processing Portsmouth
    ---------------------
    Processing city #389: High level
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=high%20level
    Finished processing High level
    ---------------------
    Processing city #390: Lolua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lolua
    Lolua doesn't exist at OpenWeather API
    Finished processing Lolua
    ---------------------
    Processing city #391: Rodez
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rodez
    Finished processing Rodez
    ---------------------
    Processing city #392: Bayonet point
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bayonet%20point
    Finished processing Bayonet point
    ---------------------
    Processing city #393: Bubaque
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bubaque
    Finished processing Bubaque
    ---------------------
    Processing city #394: Ust-kishert
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ust-kishert
    Finished processing Ust-kishert
    ---------------------
    Processing city #395: Marawi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=marawi
    Finished processing Marawi
    ---------------------
    Processing city #396: Nahrin
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nahrin
    Finished processing Nahrin
    ---------------------
    Processing city #397: Juba
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=juba
    Finished processing Juba
    ---------------------
    Processing city #398: Beyneu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=beyneu
    Finished processing Beyneu
    ---------------------
    Processing city #399: Flinders
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=flinders
    Finished processing Flinders
    ---------------------
    Processing city #400: Santa helena
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=santa%20helena
    Finished processing Santa helena
    ---------------------
    Processing city #401: Doba
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=doba
    Finished processing Doba
    ---------------------
    Processing city #402: Lagoa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lagoa
    Finished processing Lagoa
    ---------------------
    Processing city #403: Rajgarh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rajgarh
    Finished processing Rajgarh
    ---------------------
    Processing city #404: Luganville
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=luganville
    Finished processing Luganville
    ---------------------
    Processing city #405: Man
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=man
    Finished processing Man
    ---------------------
    Processing city #406: Guanica
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=guanica
    Finished processing Guanica
    ---------------------
    Processing city #407: Lira
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lira
    Finished processing Lira
    ---------------------
    Processing city #408: Cape coast
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cape%20coast
    Finished processing Cape coast
    ---------------------
    Processing city #409: Gat
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gat
    Finished processing Gat
    ---------------------
    Processing city #410: Alotau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=alotau
    Alotau doesn't exist at OpenWeather API
    Finished processing Alotau
    ---------------------
    Processing city #411: Tungor
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tungor
    Finished processing Tungor
    ---------------------
    Processing city #412: Henties bay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=henties%20bay
    Finished processing Henties bay
    ---------------------
    Processing city #413: Saint-francois
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saint-francois
    Finished processing Saint-francois
    ---------------------
    Processing city #414: Ovsyanka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ovsyanka
    Finished processing Ovsyanka
    ---------------------
    Processing city #415: Mucurapo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mucurapo
    Finished processing Mucurapo
    ---------------------
    Processing city #416: Broome
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=broome
    Finished processing Broome
    ---------------------
    Processing city #417: Nanortalik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nanortalik
    Finished processing Nanortalik
    ---------------------
    Processing city #418: Naron
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=naron
    Finished processing Naron
    ---------------------
    Processing city #419: Umbertide
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=umbertide
    Finished processing Umbertide
    ---------------------
    Processing city #420: Aksarka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=aksarka
    Finished processing Aksarka
    ---------------------
    Processing city #421: Turayf
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=turayf
    Finished processing Turayf
    ---------------------
    Processing city #422: Wilmington island
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=wilmington%20island
    Finished processing Wilmington island
    ---------------------
    Processing city #423: Lamar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lamar
    Finished processing Lamar
    ---------------------
    Processing city #424: Sindor
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sindor
    Finished processing Sindor
    ---------------------
    Processing city #425: Jujuy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=jujuy
    Jujuy doesn't exist at OpenWeather API
    Finished processing Jujuy
    ---------------------
    Processing city #426: Hervey bay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hervey%20bay
    Finished processing Hervey bay
    ---------------------
    Processing city #427: Acapulco
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=acapulco
    Finished processing Acapulco
    ---------------------
    Processing city #428: Arys
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arys
    Finished processing Arys
    ---------------------
    Processing city #429: Ayna
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ayna
    Finished processing Ayna
    ---------------------
    Processing city #430: Waw
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=waw
    Waw doesn't exist at OpenWeather API
    Finished processing Waw
    ---------------------
    Processing city #431: Amuntai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=amuntai
    Finished processing Amuntai
    ---------------------
    Processing city #432: Magadan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=magadan
    Finished processing Magadan
    ---------------------
    Processing city #433: Akureyri
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=akureyri
    Finished processing Akureyri
    ---------------------
    Processing city #434: Diamantino
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=diamantino
    Finished processing Diamantino
    ---------------------
    Processing city #435: Kaiyuan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kaiyuan
    Finished processing Kaiyuan
    ---------------------
    Processing city #436: Nyuzen
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nyuzen
    Finished processing Nyuzen
    ---------------------
    Processing city #437: Dongsheng
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dongsheng
    Finished processing Dongsheng
    ---------------------
    Processing city #438: Burica
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=burica
    Burica doesn't exist at OpenWeather API
    Finished processing Burica
    ---------------------
    Processing city #439: Stawell
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=stawell
    Finished processing Stawell
    ---------------------
    Processing city #440: Tzucacab
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tzucacab
    Finished processing Tzucacab
    ---------------------
    Processing city #441: Mayskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mayskiy
    Finished processing Mayskiy
    ---------------------
    Processing city #442: North bend
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=north%20bend
    Finished processing North bend
    ---------------------
    Processing city #443: Hornepayne
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hornepayne
    Finished processing Hornepayne
    ---------------------
    Processing city #444: Callosa de segura
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=callosa%20de%20segura
    Finished processing Callosa de segura
    ---------------------
    Processing city #445: Anito
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=anito
    Finished processing Anito
    ---------------------
    Processing city #446: Maceio
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=maceio
    Finished processing Maceio
    ---------------------
    Processing city #447: Porto novo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=porto%20novo
    Finished processing Porto novo
    ---------------------
    Processing city #448: Coolum beach
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=coolum%20beach
    Finished processing Coolum beach
    ---------------------
    Processing city #449: Klaksvik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=klaksvik
    Finished processing Klaksvik
    ---------------------
    Processing city #450: Krasnoselkup
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=krasnoselkup
    Krasnoselkup doesn't exist at OpenWeather API
    Finished processing Krasnoselkup
    ---------------------
    Processing city #451: Luderitz
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=luderitz
    Finished processing Luderitz
    ---------------------
    Processing city #452: Atambua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=atambua
    Finished processing Atambua
    ---------------------
    Processing city #453: Oreanda
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=oreanda
    Oreanda doesn't exist at OpenWeather API
    Finished processing Oreanda
    ---------------------
    Processing city #454: Puerto baquerizo moreno
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puerto%20baquerizo%20moreno
    Finished processing Puerto baquerizo moreno
    ---------------------
    Processing city #455: Alekseyevsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=alekseyevsk
    Finished processing Alekseyevsk
    ---------------------
    Processing city #456: Longyearbyen
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=longyearbyen
    Finished processing Longyearbyen
    ---------------------
    Processing city #457: Gulshat
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gulshat
    Gulshat doesn't exist at OpenWeather API
    Finished processing Gulshat
    ---------------------
    Processing city #458: Cam pha
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cam%20pha
    Cam pha doesn't exist at OpenWeather API
    Finished processing Cam pha
    ---------------------
    Processing city #459: Hudson bay
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hudson%20bay
    Finished processing Hudson bay
    ---------------------
    Processing city #460: Ardmore
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ardmore
    Finished processing Ardmore
    ---------------------
    Processing city #461: Teknaf
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=teknaf
    Finished processing Teknaf
    ---------------------
    Processing city #462: Berikulskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=berikulskiy
    Berikulskiy doesn't exist at OpenWeather API
    Finished processing Berikulskiy
    ---------------------
    Processing city #463: Ballina
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ballina
    Finished processing Ballina
    ---------------------
    Processing city #464: Ust-kamchatsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ust-kamchatsk
    Ust-kamchatsk doesn't exist at OpenWeather API
    Finished processing Ust-kamchatsk
    ---------------------
    Processing city #465: Anjiang
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=anjiang
    Finished processing Anjiang
    ---------------------
    Processing city #466: Shirokiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shirokiy
    Finished processing Shirokiy
    ---------------------
    Processing city #467: Arlit
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arlit
    Finished processing Arlit
    ---------------------
    Processing city #468: Huron
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=huron
    Finished processing Huron
    ---------------------
    Processing city #469: Kaihua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kaihua
    Finished processing Kaihua
    ---------------------
    Processing city #470: Presidencia roque saenz pena
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=presidencia%20roque%20saenz%20pena
    Finished processing Presidencia roque saenz pena
    ---------------------
    Processing city #471: Polunochnoye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=polunochnoye
    Finished processing Polunochnoye
    ---------------------
    Processing city #472: Adrar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=adrar
    Finished processing Adrar
    ---------------------
    Processing city #473: Saleaula
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=saleaula
    Saleaula doesn't exist at OpenWeather API
    Finished processing Saleaula
    ---------------------
    Processing city #474: College
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=college
    Finished processing College
    ---------------------
    Processing city #475: Kirensk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kirensk
    Finished processing Kirensk
    ---------------------
    Processing city #476: Platonovka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=platonovka
    Platonovka doesn't exist at OpenWeather API
    Finished processing Platonovka
    ---------------------
    Processing city #477: Wewak
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=wewak
    Finished processing Wewak
    ---------------------
    Processing city #478: Lahaina
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lahaina
    Finished processing Lahaina
    ---------------------
    Processing city #479: Leningradskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=leningradskiy
    Finished processing Leningradskiy
    ---------------------
    Processing city #480: Fortuna
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fortuna
    Finished processing Fortuna
    ---------------------
    Processing city #481: Berthierville
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=berthierville
    Finished processing Berthierville
    ---------------------
    Processing city #482: Ucluelet
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ucluelet
    Finished processing Ucluelet
    ---------------------
    Processing city #483: Gatesville
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gatesville
    Finished processing Gatesville
    ---------------------
    Processing city #484: Betsiamites
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=betsiamites
    Finished processing Betsiamites
    ---------------------
    Processing city #485: Neiafu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=neiafu
    Finished processing Neiafu
    ---------------------
    Processing city #486: Pinhao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pinhao
    Finished processing Pinhao
    ---------------------
    Processing city #487: Payo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=payo
    Finished processing Payo
    ---------------------
    Processing city #488: Kosonsoy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kosonsoy
    Finished processing Kosonsoy
    ---------------------
    Processing city #489: Payakumbuh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=payakumbuh
    Finished processing Payakumbuh
    ---------------------
    Processing city #490: Cidreira
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cidreira
    Finished processing Cidreira
    ---------------------
    Processing city #491: Inuvik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=inuvik
    Finished processing Inuvik
    ---------------------
    Processing city #492: Pljevlja
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pljevlja
    Finished processing Pljevlja
    ---------------------
    Processing city #493: Omboue
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=omboue
    Finished processing Omboue
    ---------------------
    Processing city #494: Nanakuli
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nanakuli
    Finished processing Nanakuli
    ---------------------
    Processing city #495: Pimenta bueno
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pimenta%20bueno
    Finished processing Pimenta bueno
    ---------------------
    Processing city #496: Pangody
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pangody
    Finished processing Pangody
    ---------------------
    Processing city #497: Manta
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=manta
    Finished processing Manta
    ---------------------
    Processing city #498: Grindavik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=grindavik
    Finished processing Grindavik
    ---------------------
    Processing city #499: Khani
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=khani
    Finished processing Khani
    ---------------------
    Processing city #500: Leh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=leh
    Finished processing Leh
    ---------------------
    Processing city #501: Hovd
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hovd
    Finished processing Hovd
    ---------------------
    Processing city #502: Sertaozinho
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sertaozinho
    Finished processing Sertaozinho
    ---------------------
    Processing city #503: Bintulu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bintulu
    Finished processing Bintulu
    ---------------------
    Processing city #504: Yantal
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yantal
    Finished processing Yantal
    ---------------------
    Processing city #505: Kangaatsiaq
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kangaatsiaq
    Finished processing Kangaatsiaq
    ---------------------
    Processing city #506: Sao jose da coroa grande
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sao%20jose%20da%20coroa%20grande
    Finished processing Sao jose da coroa grande
    ---------------------
    Processing city #507: Belaya gora
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=belaya%20gora
    Finished processing Belaya gora
    ---------------------
    Processing city #508: Oktyabrskiy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=oktyabrskiy
    Finished processing Oktyabrskiy
    ---------------------
    Processing city #509: Skalistyy
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=skalistyy
    Skalistyy doesn't exist at OpenWeather API
    Finished processing Skalistyy
    ---------------------
    Processing city #510: Sarnen
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sarnen
    Finished processing Sarnen
    ---------------------
    Processing city #511: Ca mau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ca%20mau
    Finished processing Ca mau
    ---------------------
    Processing city #512: Ambon
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ambon
    Finished processing Ambon
    ---------------------
    Processing city #513: Pontianak
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pontianak
    Finished processing Pontianak
    ---------------------
    Processing city #514: Alofi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=alofi
    Finished processing Alofi
    ---------------------
    Processing city #515: Kropotkin
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kropotkin
    Finished processing Kropotkin
    ---------------------
    Processing city #516: Kashin
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kashin
    Finished processing Kashin
    ---------------------
    Processing city #517: Grafton
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=grafton
    Finished processing Grafton
    ---------------------
    Processing city #518: Nyurba
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nyurba
    Finished processing Nyurba
    ---------------------
    Processing city #519: Vao
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vao
    Finished processing Vao
    ---------------------
    Processing city #520: Ibra
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ibra
    Finished processing Ibra
    ---------------------
    Processing city #521: Zhangye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=zhangye
    Finished processing Zhangye
    ---------------------
    Processing city #522: Mashhad
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mashhad
    Finished processing Mashhad
    ---------------------
    Processing city #523: Sao filipe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sao%20filipe
    Finished processing Sao filipe
    ---------------------
    Processing city #524: Madras
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=madras
    Finished processing Madras
    ---------------------
    Processing city #525: Cockburn harbour
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cockburn%20harbour
    Cockburn harbour doesn't exist at OpenWeather API
    Finished processing Cockburn harbour
    ---------------------
    Processing city #526: Khonuu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=khonuu
    Khonuu doesn't exist at OpenWeather API
    Finished processing Khonuu
    ---------------------
    Processing city #527: Balaipungut
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=balaipungut
    Finished processing Balaipungut
    ---------------------
    Processing city #528: Nizwa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nizwa
    Finished processing Nizwa
    ---------------------
    Processing city #529: Dawson
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dawson
    Finished processing Dawson
    ---------------------
    Processing city #530: Turukhansk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=turukhansk
    Finished processing Turukhansk
    ---------------------
    Processing city #531: Viligili
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=viligili
    Viligili doesn't exist at OpenWeather API
    Finished processing Viligili
    ---------------------
    Processing city #532: Angoche
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=angoche
    Finished processing Angoche
    ---------------------
    Processing city #533: Roald
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=roald
    Finished processing Roald
    ---------------------
    Processing city #534: Fossano
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fossano
    Finished processing Fossano
    ---------------------
    Processing city #535: Paka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=paka
    Finished processing Paka
    ---------------------
    Processing city #536: Atar
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=atar
    Finished processing Atar
    ---------------------
    Processing city #537: Samana
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=samana
    Finished processing Samana
    ---------------------
    Processing city #538: Itoman
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=itoman
    Finished processing Itoman
    ---------------------
    Processing city #539: Macau
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=macau
    Finished processing Macau
    ---------------------
    Processing city #540: Glendive
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=glendive
    Finished processing Glendive
    ---------------------
    Processing city #541: Nova olinda do norte
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=nova%20olinda%20do%20norte
    Finished processing Nova olinda do norte
    ---------------------
    Processing city #542: Magalia
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=magalia
    Finished processing Magalia
    ---------------------
    Processing city #543: Puerto del rosario
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=puerto%20del%20rosario
    Finished processing Puerto del rosario
    ---------------------
    Processing city #544: Wajima
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=wajima
    Finished processing Wajima
    ---------------------
    Processing city #545: Micheweni
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=micheweni
    Finished processing Micheweni
    ---------------------
    Processing city #546: Yinchuan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=yinchuan
    Finished processing Yinchuan
    ---------------------
    Processing city #547: Kalmunai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kalmunai
    Finished processing Kalmunai
    ---------------------
    Processing city #548: Dogondoutchi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dogondoutchi
    Finished processing Dogondoutchi
    ---------------------
    Processing city #549: Mookane
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mookane
    Finished processing Mookane
    ---------------------
    Processing city #550: Den helder
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=den%20helder
    Finished processing Den helder
    ---------------------
    Processing city #551: Shwebo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shwebo
    Finished processing Shwebo
    ---------------------
    Processing city #552: Alenquer
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=alenquer
    Finished processing Alenquer
    ---------------------
    Processing city #553: Hatillo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hatillo
    Finished processing Hatillo
    ---------------------
    Processing city #554: Joshimath
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=joshimath
    Finished processing Joshimath
    ---------------------
    Processing city #555: Mbanza-ngungu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mbanza-ngungu
    Finished processing Mbanza-ngungu
    ---------------------
    Processing city #556: Turbana
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=turbana
    Finished processing Turbana
    ---------------------
    Processing city #557: Uglegorsk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=uglegorsk
    Finished processing Uglegorsk
    ---------------------
    Processing city #558: Sao raimundo das mangabeiras
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sao%20raimundo%20das%20mangabeiras
    Finished processing Sao raimundo das mangabeiras
    ---------------------
    Processing city #559: Dabhol
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=dabhol
    Finished processing Dabhol
    ---------------------
    Processing city #560: Cavalcante
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=cavalcante
    Finished processing Cavalcante
    ---------------------
    Processing city #561: Calama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=calama
    Finished processing Calama
    ---------------------
    Processing city #562: Oussouye
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=oussouye
    Finished processing Oussouye
    ---------------------
    Processing city #563: La ronge
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=la%20ronge
    Finished processing La ronge
    ---------------------
    Processing city #564: Qasigiannguit
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=qasigiannguit
    Finished processing Qasigiannguit
    ---------------------
    Processing city #565: Ahuimanu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ahuimanu
    Finished processing Ahuimanu
    ---------------------
    Processing city #566: Bonavista
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bonavista
    Finished processing Bonavista
    ---------------------
    Processing city #567: Anzio
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=anzio
    Finished processing Anzio
    ---------------------
    Processing city #568: Arawa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=arawa
    Finished processing Arawa
    ---------------------
    Processing city #569: Pangai
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=pangai
    Finished processing Pangai
    ---------------------
    Processing city #570: Kimbe
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kimbe
    Finished processing Kimbe
    ---------------------
    Processing city #571: Sechelt
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sechelt
    Finished processing Sechelt
    ---------------------
    Processing city #572: Obo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=obo
    Finished processing Obo
    ---------------------
    Processing city #573: Fort wellington
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fort%20wellington
    Finished processing Fort wellington
    ---------------------
    Processing city #574: Ust-kuyga
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ust-kuyga
    Finished processing Ust-kuyga
    ---------------------
    Processing city #575: Tiznit
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tiznit
    Finished processing Tiznit
    ---------------------
    Processing city #576: Abengourou
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=abengourou
    Finished processing Abengourou
    ---------------------
    Processing city #577: Praia da vitoria
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=praia%20da%20vitoria
    Finished processing Praia da vitoria
    ---------------------
    Processing city #578: Ostersund
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ostersund
    Finished processing Ostersund
    ---------------------
    Processing city #579: Rio grande
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=rio%20grande
    Finished processing Rio grande
    ---------------------
    Processing city #580: Itacarambi
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=itacarambi
    Finished processing Itacarambi
    ---------------------
    Processing city #581: Labuhan
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=labuhan
    Finished processing Labuhan
    ---------------------
    Processing city #582: Gua
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gua
    Finished processing Gua
    ---------------------
    Processing city #583: Hajnowka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=hajnowka
    Finished processing Hajnowka
    ---------------------
    Processing city #584: Kendari
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kendari
    Finished processing Kendari
    ---------------------
    Processing city #585: Creel
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=creel
    Finished processing Creel
    ---------------------
    Processing city #586: Bonfim
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=bonfim
    Finished processing Bonfim
    ---------------------
    Processing city #587: Tevriz
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tevriz
    Finished processing Tevriz
    ---------------------
    Processing city #588: Shatki
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shatki
    Finished processing Shatki
    ---------------------
    Processing city #589: Caapucu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=caapucu
    Finished processing Caapucu
    ---------------------
    Processing city #590: General pico
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=general%20pico
    Finished processing General pico
    ---------------------
    Processing city #591: Lasa
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lasa
    Finished processing Lasa
    ---------------------
    Processing city #592: Aflu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=aflu
    Aflu doesn't exist at OpenWeather API
    Finished processing Aflu
    ---------------------
    Processing city #593: Uk
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=uk
    Uk doesn't exist at OpenWeather API
    Finished processing Uk
    ---------------------
    Processing city #594: Gryfice
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=gryfice
    Finished processing Gryfice
    ---------------------
    Processing city #595: Eyl
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=eyl
    Finished processing Eyl
    ---------------------
    Processing city #596: Karpogory
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=karpogory
    Finished processing Karpogory
    ---------------------
    Processing city #597: Sebezh
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sebezh
    Finished processing Sebezh
    ---------------------
    Processing city #598: Tautira
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=tautira
    Finished processing Tautira
    ---------------------
    Processing city #599: Haines junction
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=haines%20junction
    Finished processing Haines junction
    ---------------------
    Processing city #600: Sychevka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sychevka
    Finished processing Sychevka
    ---------------------
    Processing city #601: Fairbanks
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=fairbanks
    Finished processing Fairbanks
    ---------------------
    Processing city #602: Kaa-khem
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kaa-khem
    Finished processing Kaa-khem
    ---------------------
    Processing city #603: Ornskoldsvik
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=ornskoldsvik
    Finished processing Ornskoldsvik
    ---------------------
    Processing city #604: Myronivka
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=myronivka
    Finished processing Myronivka
    ---------------------
    Processing city #605: Oyama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=oyama
    Finished processing Oyama
    ---------------------
    Processing city #606: Piacabucu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=piacabucu
    Finished processing Piacabucu
    ---------------------
    Processing city #607: Makhachkala
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=makhachkala
    Finished processing Makhachkala
    ---------------------
    Processing city #608: Brahmapuri
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=brahmapuri
    Brahmapuri doesn't exist at OpenWeather API
    Finished processing Brahmapuri
    ---------------------
    Processing city #609: Vila franca do campo
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=vila%20franca%20do%20campo
    Finished processing Vila franca do campo
    ---------------------
    Processing city #610: Los aquijes
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=los%20aquijes
    Finished processing Los aquijes
    ---------------------
    Processing city #611: Isiro
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=isiro
    Finished processing Isiro
    ---------------------
    Processing city #612: Shieli
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=shieli
    Finished processing Shieli
    ---------------------
    Processing city #613: Sakakah
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=sakakah
    Sakakah doesn't exist at OpenWeather API
    Finished processing Sakakah
    ---------------------
    Processing city #614: Jalu
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=jalu
    Finished processing Jalu
    ---------------------
    Processing city #615: Surt
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=surt
    Finished processing Surt
    ---------------------
    Processing city #616: Avera
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=avera
    Finished processing Avera
    ---------------------
    Processing city #617: Kinel
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kinel
    Finished processing Kinel
    ---------------------
    Processing city #618: Linares
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=linares
    Finished processing Linares
    ---------------------
    Processing city #619: Lagunas
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=lagunas
    Finished processing Lagunas
    ---------------------
    Processing city #620: Mabama
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mabama
    Finished processing Mabama
    ---------------------
    Processing city #621: Mehriz
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=mehriz
    Finished processing Mehriz
    ---------------------
    Processing city #622: Kynashiv
    url: http://api.openweathermap.org/data/2.5/weather?appid=7fd4a4a24b5be08f6fe06eaaafc64a46&units=imperial&q=kynashiv
    Finished processing Kynashiv


### Convert Raw Data to DataFrame
* Export the city data into a .csv.
* Display the DataFrame


```python
# Create dataframe with api data
weather_df = pd.DataFrame([names, temps, winds, humids, clouds, lat])
weather_df = weather_df.transpose()
weather_df.columns = ['City Name', 'Max Temperature (F)', 'Wind speed (mph)', \
                      'Humidity (%)', 'Cloudiness', 'Latitude']

# Create csv output
weather_df.to_csv('../output/weather.csv', )

# Display dataframe
weather_df
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
      <th>City Name</th>
      <th>Max Temperature (F)</th>
      <th>Wind speed (mph)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness</th>
      <th>Latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mariinsk</td>
      <td>41.98</td>
      <td>10.16</td>
      <td>71</td>
      <td>56</td>
      <td>56.21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Chokurdakh</td>
      <td>27.04</td>
      <td>12.86</td>
      <td>88</td>
      <td>97</td>
      <td>70.62</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Voloshka</td>
      <td>37.77</td>
      <td>7.34</td>
      <td>88</td>
      <td>100</td>
      <td>61.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Namibe</td>
      <td>64.93</td>
      <td>6.58</td>
      <td>82</td>
      <td>53</td>
      <td>-15.19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Punta Arenas</td>
      <td>41</td>
      <td>28.86</td>
      <td>77</td>
      <td>0</td>
      <td>-53.16</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Watertown</td>
      <td>46.4</td>
      <td>8.05</td>
      <td>93</td>
      <td>90</td>
      <td>44.9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Severo-Kurilsk</td>
      <td>48.01</td>
      <td>14.41</td>
      <td>96</td>
      <td>100</td>
      <td>50.68</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Butaritari</td>
      <td>84.87</td>
      <td>2.44</td>
      <td>59</td>
      <td>100</td>
      <td>3.07</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ribeira Grande</td>
      <td>69.54</td>
      <td>8.68</td>
      <td>68</td>
      <td>100</td>
      <td>38.52</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Lithakia</td>
      <td>75.2</td>
      <td>8.05</td>
      <td>73</td>
      <td>40</td>
      <td>37.72</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Plettenberg Bay</td>
      <td>58.32</td>
      <td>7.43</td>
      <td>88</td>
      <td>100</td>
      <td>-34.05</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Laval</td>
      <td>46</td>
      <td>6.93</td>
      <td>70</td>
      <td>5</td>
      <td>45.58</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Lerwick</td>
      <td>48.2</td>
      <td>8.05</td>
      <td>87</td>
      <td>67</td>
      <td>60.15</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Mataura</td>
      <td>46.99</td>
      <td>7</td>
      <td>91</td>
      <td>86</td>
      <td>-46.19</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Hisua</td>
      <td>82.4</td>
      <td>5.17</td>
      <td>83</td>
      <td>75</td>
      <td>24.84</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East London</td>
      <td>55.4</td>
      <td>2.24</td>
      <td>87</td>
      <td>0</td>
      <td>-33.02</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Saint-Philippe</td>
      <td>46</td>
      <td>9.13</td>
      <td>86</td>
      <td>1</td>
      <td>45.36</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Auki</td>
      <td>72.9</td>
      <td>6.76</td>
      <td>89</td>
      <td>29</td>
      <td>12.18</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Naze</td>
      <td>72.33</td>
      <td>4.47</td>
      <td>98</td>
      <td>100</td>
      <td>5.43</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Goderich</td>
      <td>54</td>
      <td>11.01</td>
      <td>85</td>
      <td>100</td>
      <td>43.74</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Nikolskoye</td>
      <td>41</td>
      <td>4.47</td>
      <td>93</td>
      <td>90</td>
      <td>59.7</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hobart</td>
      <td>82</td>
      <td>10.29</td>
      <td>31</td>
      <td>0</td>
      <td>-42.88</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Husavik</td>
      <td>42.01</td>
      <td>5.7</td>
      <td>85</td>
      <td>100</td>
      <td>50.56</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Busselton</td>
      <td>68</td>
      <td>3</td>
      <td>74</td>
      <td>100</td>
      <td>-33.64</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Ushuaia</td>
      <td>41</td>
      <td>29.97</td>
      <td>65</td>
      <td>40</td>
      <td>-54.81</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Saskylakh</td>
      <td>19.39</td>
      <td>0.87</td>
      <td>76</td>
      <td>4</td>
      <td>71.97</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Kodiak</td>
      <td>59</td>
      <td>11.41</td>
      <td>93</td>
      <td>90</td>
      <td>39.95</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Castro</td>
      <td>35.6</td>
      <td>6.93</td>
      <td>94</td>
      <td>0</td>
      <td>-42.48</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Tuktoyaktuk</td>
      <td>35.6</td>
      <td>14.99</td>
      <td>93</td>
      <td>90</td>
      <td>69.44</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Sao Miguel do Araguaia</td>
      <td>73.95</td>
      <td>3.49</td>
      <td>77</td>
      <td>0</td>
      <td>-13.28</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>530</th>
      <td>Caapucu</td>
      <td>69.02</td>
      <td>5.73</td>
      <td>94</td>
      <td>100</td>
      <td>-26.22</td>
    </tr>
    <tr>
      <th>531</th>
      <td>General Pico</td>
      <td>48.46</td>
      <td>11.88</td>
      <td>48</td>
      <td>6</td>
      <td>-35.66</td>
    </tr>
    <tr>
      <th>532</th>
      <td>Lasa</td>
      <td>68</td>
      <td>8.05</td>
      <td>82</td>
      <td>75</td>
      <td>34.92</td>
    </tr>
    <tr>
      <th>533</th>
      <td>Gryfice</td>
      <td>46.99</td>
      <td>5.82</td>
      <td>100</td>
      <td>75</td>
      <td>53.91</td>
    </tr>
    <tr>
      <th>534</th>
      <td>Eyl</td>
      <td>73.21</td>
      <td>1.81</td>
      <td>91</td>
      <td>12</td>
      <td>7.98</td>
    </tr>
    <tr>
      <th>535</th>
      <td>Karpogory</td>
      <td>30.71</td>
      <td>6.35</td>
      <td>90</td>
      <td>46</td>
      <td>64</td>
    </tr>
    <tr>
      <th>536</th>
      <td>Sebezh</td>
      <td>45.27</td>
      <td>7.34</td>
      <td>96</td>
      <td>100</td>
      <td>56.29</td>
    </tr>
    <tr>
      <th>537</th>
      <td>Tautira</td>
      <td>80.6</td>
      <td>9.17</td>
      <td>69</td>
      <td>20</td>
      <td>-17.73</td>
    </tr>
    <tr>
      <th>538</th>
      <td>Haines Junction</td>
      <td>34.92</td>
      <td>2.86</td>
      <td>78</td>
      <td>19</td>
      <td>60.75</td>
    </tr>
    <tr>
      <th>539</th>
      <td>Sychevka</td>
      <td>50.33</td>
      <td>7.61</td>
      <td>94</td>
      <td>100</td>
      <td>55.83</td>
    </tr>
    <tr>
      <th>540</th>
      <td>Fairbanks</td>
      <td>37.99</td>
      <td>5.82</td>
      <td>100</td>
      <td>90</td>
      <td>64.84</td>
    </tr>
    <tr>
      <th>541</th>
      <td>Kaa-Khem</td>
      <td>34.67</td>
      <td>4.23</td>
      <td>39</td>
      <td>0</td>
      <td>51.7</td>
    </tr>
    <tr>
      <th>542</th>
      <td>Ornskoldsvik</td>
      <td>39.2</td>
      <td>10.29</td>
      <td>80</td>
      <td>66</td>
      <td>63.29</td>
    </tr>
    <tr>
      <th>543</th>
      <td>Myronivka</td>
      <td>55.37</td>
      <td>12.73</td>
      <td>88</td>
      <td>90</td>
      <td>49.66</td>
    </tr>
    <tr>
      <th>544</th>
      <td>Oyama</td>
      <td>87.01</td>
      <td>1.01</td>
      <td>11</td>
      <td>62</td>
      <td>36.31</td>
    </tr>
    <tr>
      <th>545</th>
      <td>Piacabucu</td>
      <td>76</td>
      <td>11.45</td>
      <td>80</td>
      <td>0</td>
      <td>-10.41</td>
    </tr>
    <tr>
      <th>546</th>
      <td>Makhachkala</td>
      <td>64.4</td>
      <td>11.18</td>
      <td>88</td>
      <td>20</td>
      <td>42.98</td>
    </tr>
    <tr>
      <th>547</th>
      <td>Vila Franca do Campo</td>
      <td>71.01</td>
      <td>12.75</td>
      <td>73</td>
      <td>75</td>
      <td>37.72</td>
    </tr>
    <tr>
      <th>548</th>
      <td>Los Aquijes</td>
      <td>63</td>
      <td>2.24</td>
      <td>82</td>
      <td>0</td>
      <td>-14.1</td>
    </tr>
    <tr>
      <th>549</th>
      <td>Isiro</td>
      <td>66.89</td>
      <td>1.72</td>
      <td>99</td>
      <td>98</td>
      <td>2.77</td>
    </tr>
    <tr>
      <th>550</th>
      <td>Shieli</td>
      <td>51.88</td>
      <td>16.53</td>
      <td>30</td>
      <td>0</td>
      <td>44.18</td>
    </tr>
    <tr>
      <th>551</th>
      <td>Jalu</td>
      <td>74.4</td>
      <td>11.14</td>
      <td>31</td>
      <td>0</td>
      <td>29.03</td>
    </tr>
    <tr>
      <th>552</th>
      <td>Surt</td>
      <td>78.92</td>
      <td>18.75</td>
      <td>71</td>
      <td>0</td>
      <td>31.21</td>
    </tr>
    <tr>
      <th>553</th>
      <td>Avera</td>
      <td>82.99</td>
      <td>4.7</td>
      <td>74</td>
      <td>1</td>
      <td>33.19</td>
    </tr>
    <tr>
      <th>554</th>
      <td>Kinel</td>
      <td>48.2</td>
      <td>6.71</td>
      <td>100</td>
      <td>100</td>
      <td>53.22</td>
    </tr>
    <tr>
      <th>555</th>
      <td>Linares</td>
      <td>40.83</td>
      <td>4.07</td>
      <td>89</td>
      <td>0</td>
      <td>-35.85</td>
    </tr>
    <tr>
      <th>556</th>
      <td>Lagunas</td>
      <td>77.13</td>
      <td>0.45</td>
      <td>71</td>
      <td>56</td>
      <td>-5.23</td>
    </tr>
    <tr>
      <th>557</th>
      <td>Mabama</td>
      <td>72.65</td>
      <td>4.36</td>
      <td>98</td>
      <td>100</td>
      <td>8.94</td>
    </tr>
    <tr>
      <th>558</th>
      <td>Mehriz</td>
      <td>68</td>
      <td>2.24</td>
      <td>22</td>
      <td>0</td>
      <td>31.58</td>
    </tr>
    <tr>
      <th>559</th>
      <td>Kynashiv</td>
      <td>57.2</td>
      <td>6.71</td>
      <td>76</td>
      <td>40</td>
      <td>48.67</td>
    </tr>
  </tbody>
</table>
<p>560 rows × 6 columns</p>
</div>




```python
# Some basic stats about the dataframe for reference
weather_df.describe()
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
      <th>City Name</th>
      <th>Max Temperature (F)</th>
      <th>Wind speed (mph)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness</th>
      <th>Latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>560</td>
      <td>560.0</td>
      <td>560.0</td>
      <td>560</td>
      <td>560</td>
      <td>560.00</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>559</td>
      <td>347.0</td>
      <td>285.0</td>
      <td>87</td>
      <td>74</td>
      <td>538.00</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Victoria</td>
      <td>84.2</td>
      <td>4.7</td>
      <td>93</td>
      <td>0</td>
      <td>52.47</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2</td>
      <td>13.0</td>
      <td>34.0</td>
      <td>38</td>
      <td>137</td>
      <td>2.00</td>
    </tr>
  </tbody>
</table>
</div>



### Plotting the Data
* Use proper labeling of the plots using plot titles (including date of analysis) and axes labels.
* Save the plotted figures as .pngs.

#### Latitude vs. Temperature Plot


```python
plt.scatter(weather_df['Latitude'], weather_df['Max Temperature (F)'], c = 'blue', marker = '.')
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.title(f"Latitude vs. Temperature ({today.strftime('%m/%d/%y')})", weight = 'bold')
plt.vlines(0, min(weather_df['Max Temperature (F)'] - 10), max(weather_df['Max Temperature (F)'] + 10), linestyles='solid', colors = 'red')
plt.grid()
plt.ylim(min(weather_df['Max Temperature (F)'] - 10), max(weather_df['Max Temperature (F)'] + 10))
plt.savefig("../images/temperature.png")
```


![png](output_11_0.png)


#### Latitude vs. Humidity Plot


```python
plt.scatter(weather_df['Latitude'], weather_df['Humidity (%)'], c = 'lightseagreen', marker = '.')
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.title(f"Latitude vs. Humidity ({today.strftime('%m/%d/%y')})", weight = 'bold')
plt.vlines(0, 0,100, linestyles='solid', colors = 'red')
plt.ylim(0,100)
plt.grid()
plt.savefig("../images/humidity.png")
```


![png](output_13_0.png)


#### Latitude vs. Cloudiness Plot


```python
plt.scatter(weather_df['Latitude'], weather_df['Cloudiness'], c = 'slategrey', marker = '.')
plt.xlabel("Latitude")
plt.ylabel("Cloudiness")
plt.title(f"Latitude vs. Cloudiness ({today.strftime('%m/%d/%y')})", weight = 'bold')
plt.vlines(0, 0,100, linestyles='solid', colors = 'r')
plt.ylim(0,100)
plt.grid()
plt.savefig("../images/cloudiness.png")
```


![png](output_15_0.png)


#### Latitude vs. Wind Speed Plot


```python
plt.scatter(weather_df['Latitude'], weather_df['Wind speed (mph)'], c = 'cyan', marker = '.')
plt.xlabel("Latitude")
plt.ylabel("Wind speed (mph)")
plt.title(f"Latitude vs. Wind Speed ({today.strftime('%m/%d/%y')})", weight = 'bold')
plt.vlines(0, 0,100, linestyles='solid', colors = 'red')
plt.ylim(0,100)
plt.grid()
plt.savefig("../images/wind_speed.png")
```


![png](output_17_0.png)

