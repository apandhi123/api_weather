

```python
import matplotlib.pyplot as plt
%matplotlib inline 
from citipy import citipy as cp
import pandas as pd
import requests as req
import json
import time
```


```python
#Grab list of cities based on coordinates from citipy
citylist = []
count = 0
dup = 'no'

for x in range(-90,90,1):
    for y in range(-180,180,1):
        city = cp.nearest_city(x, y)
        citdict = {}
        citdict['city'] = city.city_name
        citdict['country'] = city.country_code
        citdict['lat'] = x
        citdict['long'] = y
        if len(citylist) == 0:
            citylist.append(citdict)
            count+=1
            continue
        else:
            #Get rid of duplicates
            for city in citylist:
                if city['city'] == citdict['city']:
                    dup = 'yes'
        if dup == 'no':
            citylist.append(citdict)
            count+=1
        else:
            dup = 'no'

print(len(citylist))
```

    7957
    


```python
#Create dataframe. Grab 500 random cities
citypd = pd.DataFrame({
    'city': [x['city'] for x in citylist],
    'country': [x['country'] for x in citylist],
})

citypd.head()

samplecity = citypd.sample(500)
```


```python
samplecity
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
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7529</th>
      <td>syamzha</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>1720</th>
      <td>juruti</td>
      <td>br</td>
    </tr>
    <tr>
      <th>3097</th>
      <td>srivardhan</td>
      <td>in</td>
    </tr>
    <tr>
      <th>4450</th>
      <td>tral</td>
      <td>in</td>
    </tr>
    <tr>
      <th>3327</th>
      <td>shimoda</td>
      <td>jp</td>
    </tr>
    <tr>
      <th>3744</th>
      <td>sheopur</td>
      <td>in</td>
    </tr>
    <tr>
      <th>5968</th>
      <td>caribou</td>
      <td>us</td>
    </tr>
    <tr>
      <th>5881</th>
      <td>kanjiza</td>
      <td>rs</td>
    </tr>
    <tr>
      <th>6874</th>
      <td>brunsbuttel</td>
      <td>de</td>
    </tr>
    <tr>
      <th>5531</th>
      <td>zhanakorgan</td>
      <td>kz</td>
    </tr>
    <tr>
      <th>47</th>
      <td>saldanha</td>
      <td>za</td>
    </tr>
    <tr>
      <th>1227</th>
      <td>chama</td>
      <td>zm</td>
    </tr>
    <tr>
      <th>4686</th>
      <td>gilgit</td>
      <td>pk</td>
    </tr>
    <tr>
      <th>3213</th>
      <td>cozumel</td>
      <td>mx</td>
    </tr>
    <tr>
      <th>7465</th>
      <td>sosva</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>7761</th>
      <td>malm</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4296</th>
      <td>mineral wells</td>
      <td>us</td>
    </tr>
    <tr>
      <th>2197</th>
      <td>manadhoo</td>
      <td>mv</td>
    </tr>
    <tr>
      <th>2852</th>
      <td>bandiagara</td>
      <td>ml</td>
    </tr>
    <tr>
      <th>911</th>
      <td>mocambique</td>
      <td>mz</td>
    </tr>
    <tr>
      <th>4013</th>
      <td>nanchang</td>
      <td>cn</td>
    </tr>
    <tr>
      <th>5905</th>
      <td>arshan</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>7537</th>
      <td>raduzhnyy</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>5707</th>
      <td>chippewa falls</td>
      <td>us</td>
    </tr>
    <tr>
      <th>1530</th>
      <td>russas</td>
      <td>br</td>
    </tr>
    <tr>
      <th>6138</th>
      <td>sinegorskiy</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>7237</th>
      <td>poddorye</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>2893</th>
      <td>bang len</td>
      <td>th</td>
    </tr>
    <tr>
      <th>3444</th>
      <td>sakti</td>
      <td>in</td>
    </tr>
    <tr>
      <th>5184</th>
      <td>huanren</td>
      <td>cn</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2119</th>
      <td>bandarbeyla</td>
      <td>so</td>
    </tr>
    <tr>
      <th>5773</th>
      <td>krymsk</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>1111</th>
      <td>catumbela</td>
      <td>ao</td>
    </tr>
    <tr>
      <th>5799</th>
      <td>chaihe</td>
      <td>cn</td>
    </tr>
    <tr>
      <th>3054</th>
      <td>kosum phisai</td>
      <td>th</td>
    </tr>
    <tr>
      <th>2338</th>
      <td>savalou</td>
      <td>bj</td>
    </tr>
    <tr>
      <th>4432</th>
      <td>alashtar</td>
      <td>ir</td>
    </tr>
    <tr>
      <th>2551</th>
      <td>bahile</td>
      <td>ph</td>
    </tr>
    <tr>
      <th>6420</th>
      <td>tyrma</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>7301</th>
      <td>alekseyevsk</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>1886</th>
      <td>wamba</td>
      <td>cd</td>
    </tr>
    <tr>
      <th>6261</th>
      <td>mahdalynivka</td>
      <td>ua</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>san jeronimo</td>
      <td>mx</td>
    </tr>
    <tr>
      <th>5449</th>
      <td>mason city</td>
      <td>us</td>
    </tr>
    <tr>
      <th>3278</th>
      <td>niquero</td>
      <td>cu</td>
    </tr>
    <tr>
      <th>1771</th>
      <td>sao gabriel da cachoeira</td>
      <td>br</td>
    </tr>
    <tr>
      <th>1976</th>
      <td>opobo</td>
      <td>ng</td>
    </tr>
    <tr>
      <th>2564</th>
      <td>puerto el triunfo</td>
      <td>sv</td>
    </tr>
    <tr>
      <th>7255</th>
      <td>urzhum</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>6197</th>
      <td>steinbach</td>
      <td>ca</td>
    </tr>
    <tr>
      <th>857</th>
      <td>marondera</td>
      <td>zw</td>
    </tr>
    <tr>
      <th>1768</th>
      <td>archidona</td>
      <td>ec</td>
    </tr>
    <tr>
      <th>4210</th>
      <td>monroe</td>
      <td>us</td>
    </tr>
    <tr>
      <th>1825</th>
      <td>salinopolis</td>
      <td>br</td>
    </tr>
    <tr>
      <th>6308</th>
      <td>powell river</td>
      <td>ca</td>
    </tr>
    <tr>
      <th>7207</th>
      <td>mallaig</td>
      <td>gb</td>
    </tr>
    <tr>
      <th>3301</th>
      <td>kanker</td>
      <td>in</td>
    </tr>
    <tr>
      <th>3122</th>
      <td>that phanom</td>
      <td>th</td>
    </tr>
    <tr>
      <th>3358</th>
      <td>cockburn town</td>
      <td>tc</td>
    </tr>
    <tr>
      <th>3801</th>
      <td>matay</td>
      <td>eg</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 2 columns</p>
</div>




```python
apikey = 'dbf03c8aa578b5077b6964aa9ffdba0e'
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "Imperial"
count = 0
samplecity['latitude'] = ""
samplecity['longitude'] = ""
samplecity['temperature'] = ""
samplecity['humidity'] = ""
samplecity['cloudiness'] = ""
samplecity['wind_speed'] = ""

for index,row in samplecity.iterrows():
    count+= 1
    query_url = url + "appid=" + apikey + "&units=" + units + "&q=" + row['city']
    try:
        weather_response = req.get(query_url)
        cityweather = weather_response.json()
#         print(cityweather)
        samplecity.set_value(index, "latitude", int(cityweather['coord']['lat']))
        samplecity.set_value(index, "longitude", int(cityweather['coord']['lat']))
        samplecity.set_value(index, "temperature", int(cityweather['main']['temp']))
        samplecity.set_value(index, "humidity", int(cityweather['main']['humidity']))
        samplecity.set_value(index, "cloudiness", int(cityweather['clouds']['all']))
        samplecity.set_value(index, "wind_speed", int(cityweather['wind']['speed']))
    except:
        print(f"No data for this city: {row['city']}")
    print(f"This is city#: {count}")
    print(f"This is: {row['city']}" )
    print(f"This is the requested URL: {query_url}")
```

    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:19: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:20: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:21: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:22: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:23: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    C:\Users\apandhi\Anaconda3\lib\site-packages\ipykernel_launcher.py:24: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    

    This is city#: 1
    This is: syamzha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=syamzha
    This is city#: 2
    This is: juruti
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=juruti
    This is city#: 3
    This is: srivardhan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=srivardhan
    This is city#: 4
    This is: tral
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tral
    This is city#: 5
    This is: shimoda
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shimoda
    This is city#: 6
    This is: sheopur
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sheopur
    This is city#: 7
    This is: caribou
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=caribou
    This is city#: 8
    This is: kanjiza
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kanjiza
    This is city#: 9
    This is: brunsbuttel
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=brunsbuttel
    This is city#: 10
    This is: zhanakorgan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=zhanakorgan
    This is city#: 11
    This is: saldanha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=saldanha
    This is city#: 12
    This is: chama
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=chama
    This is city#: 13
    This is: gilgit
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=gilgit
    No data for this city: cozumel
    This is city#: 14
    This is: cozumel
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=cozumel
    This is city#: 15
    This is: sosva
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sosva
    This is city#: 16
    This is: malm
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=malm
    This is city#: 17
    This is: mineral wells
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=mineral wells
    This is city#: 18
    This is: manadhoo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=manadhoo
    This is city#: 19
    This is: bandiagara
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bandiagara
    No data for this city: mocambique
    This is city#: 20
    This is: mocambique
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=mocambique
    This is city#: 21
    This is: nanchang
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=nanchang
    This is city#: 22
    This is: arshan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=arshan
    This is city#: 23
    This is: raduzhnyy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=raduzhnyy
    This is city#: 24
    This is: chippewa falls
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=chippewa falls
    This is city#: 25
    This is: russas
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=russas
    This is city#: 26
    This is: sinegorskiy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sinegorskiy
    This is city#: 27
    This is: poddorye
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=poddorye
    This is city#: 28
    This is: bang len
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bang len
    This is city#: 29
    This is: sakti
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sakti
    This is city#: 30
    This is: huanren
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=huanren
    This is city#: 31
    This is: bonnyville
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bonnyville
    This is city#: 32
    This is: aketi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=aketi
    This is city#: 33
    This is: sajoszoged
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sajoszoged
    This is city#: 34
    This is: moree
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=moree
    This is city#: 35
    This is: podgornoye
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=podgornoye
    This is city#: 36
    This is: altus
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=altus
    No data for this city: ondorhaan
    This is city#: 37
    This is: ondorhaan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ondorhaan
    This is city#: 38
    This is: koscierzyna
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=koscierzyna
    This is city#: 39
    This is: cefalu
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=cefalu
    No data for this city: hunza
    This is city#: 40
    This is: hunza
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=hunza
    This is city#: 41
    This is: chiang khong
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=chiang khong
    This is city#: 42
    This is: kinsale
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kinsale
    This is city#: 43
    This is: yuli
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=yuli
    This is city#: 44
    This is: yarim
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=yarim
    This is city#: 45
    This is: canmore
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=canmore
    This is city#: 46
    This is: golub-dobrzyn
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=golub-dobrzyn
    This is city#: 47
    This is: east brainerd
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=east brainerd
    No data for this city: milingimbi
    This is city#: 48
    This is: milingimbi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=milingimbi
    This is city#: 49
    This is: jinka
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=jinka
    This is city#: 50
    This is: shintomi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shintomi
    This is city#: 51
    This is: langley park
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=langley park
    This is city#: 52
    This is: novyy nekouz
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=novyy nekouz
    This is city#: 53
    This is: sontra
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sontra
    No data for this city: rungata
    This is city#: 54
    This is: rungata
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=rungata
    This is city#: 55
    This is: singarayakonda
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=singarayakonda
    This is city#: 56
    This is: mglin
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=mglin
    This is city#: 57
    This is: obsharovka
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=obsharovka
    This is city#: 58
    This is: thai binh
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=thai binh
    This is city#: 59
    This is: guymon
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=guymon
    This is city#: 60
    This is: batouri
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=batouri
    This is city#: 61
    This is: khatra
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=khatra
    This is city#: 62
    This is: krasnyy chikoy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=krasnyy chikoy
    This is city#: 63
    This is: pastos bons
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=pastos bons
    No data for this city: haibowan
    This is city#: 64
    This is: haibowan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=haibowan
    This is city#: 65
    This is: krutikha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=krutikha
    No data for this city: tanshui
    This is city#: 66
    This is: tanshui
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tanshui
    This is city#: 67
    This is: sarlat-la-caneda
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sarlat-la-caneda
    This is city#: 68
    This is: kalyazin
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kalyazin
    This is city#: 69
    This is: ensenada
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ensenada
    No data for this city: karakendzha
    This is city#: 70
    This is: karakendzha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=karakendzha
    This is city#: 71
    This is: salamanca
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=salamanca
    No data for this city: vitoria da conquista
    This is city#: 72
    This is: vitoria da conquista
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=vitoria da conquista
    This is city#: 73
    This is: wawa
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=wawa
    This is city#: 74
    This is: arawa
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=arawa
    No data for this city: kegayli
    This is city#: 75
    This is: kegayli
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kegayli
    This is city#: 76
    This is: misratah
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=misratah
    This is city#: 77
    This is: jaguarao
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=jaguarao
    This is city#: 78
    This is: gairo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=gairo
    This is city#: 79
    This is: huaicheng
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=huaicheng
    This is city#: 80
    This is: brindisi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=brindisi
    This is city#: 81
    This is: khor
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=khor
    This is city#: 82
    This is: corner brook
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=corner brook
    This is city#: 83
    This is: shebunino
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shebunino
    This is city#: 84
    This is: narok
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=narok
    This is city#: 85
    This is: anadyr
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=anadyr
    This is city#: 86
    This is: bhanpuri
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bhanpuri
    No data for this city: saleilua
    This is city#: 87
    This is: saleilua
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=saleilua
    This is city#: 88
    This is: tomari
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tomari
    This is city#: 89
    This is: salekhard
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=salekhard
    This is city#: 90
    This is: tumpat
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tumpat
    This is city#: 91
    This is: peru
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=peru
    This is city#: 92
    This is: diamantino
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=diamantino
    No data for this city: khagrachari
    This is city#: 93
    This is: khagrachari
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=khagrachari
    This is city#: 94
    This is: high level
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=high level
    This is city#: 95
    This is: preston
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=preston
    This is city#: 96
    This is: belaya kholunitsa
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=belaya kholunitsa
    This is city#: 97
    This is: colquechaca
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=colquechaca
    This is city#: 98
    This is: kardla
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kardla
    This is city#: 99
    This is: visby
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=visby
    This is city#: 100
    This is: plastun
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=plastun
    This is city#: 101
    This is: touba
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=touba
    This is city#: 102
    This is: sao borja
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sao borja
    This is city#: 103
    This is: dumbraveni
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=dumbraveni
    This is city#: 104
    This is: betlitsa
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=betlitsa
    This is city#: 105
    This is: shilka
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shilka
    This is city#: 106
    This is: kilrush
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kilrush
    This is city#: 107
    This is: belaya glina
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=belaya glina
    This is city#: 108
    This is: doha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=doha
    This is city#: 109
    This is: broome
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=broome
    This is city#: 110
    This is: salgueiro
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=salgueiro
    This is city#: 111
    This is: shihezi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shihezi
    This is city#: 112
    This is: kolondieba
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kolondieba
    This is city#: 113
    This is: palmerston
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=palmerston
    No data for this city: burkhala
    This is city#: 114
    This is: burkhala
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=burkhala
    This is city#: 115
    This is: bose
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bose
    This is city#: 116
    This is: harer
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=harer
    This is city#: 117
    This is: channagiri
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=channagiri
    This is city#: 118
    This is: varkaus
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=varkaus
    This is city#: 119
    This is: stupino
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=stupino
    This is city#: 120
    This is: koungheul
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=koungheul
    This is city#: 121
    This is: emba
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=emba
    This is city#: 122
    This is: lalmohan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=lalmohan
    This is city#: 123
    This is: vikindu
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=vikindu
    This is city#: 124
    This is: bagotville
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bagotville
    This is city#: 125
    This is: durusu
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=durusu
    This is city#: 126
    This is: tanhacu
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tanhacu
    This is city#: 127
    This is: karaton
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=karaton
    This is city#: 128
    This is: sciacca
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sciacca
    This is city#: 129
    This is: lata
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=lata
    This is city#: 130
    This is: beysehir
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=beysehir
    This is city#: 131
    This is: dvinskoy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=dvinskoy
    This is city#: 132
    This is: kavali
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kavali
    This is city#: 133
    This is: bela vista
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bela vista
    This is city#: 134
    This is: surgut
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=surgut
    This is city#: 135
    This is: batken
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=batken
    This is city#: 136
    This is: batalha
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=batalha
    This is city#: 137
    This is: shorapani
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=shorapani
    This is city#: 138
    This is: satka
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=satka
    No data for this city: zachagansk
    This is city#: 139
    This is: zachagansk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=zachagansk
    This is city#: 140
    This is: pitanga
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=pitanga
    This is city#: 141
    This is: volchansk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=volchansk
    No data for this city: mrirt
    This is city#: 142
    This is: mrirt
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=mrirt
    This is city#: 143
    This is: srednebelaya
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=srednebelaya
    This is city#: 144
    This is: khirkiya
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=khirkiya
    This is city#: 145
    This is: togur
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=togur
    This is city#: 146
    This is: dayong
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=dayong
    No data for this city: azad shahr
    This is city#: 147
    This is: azad shahr
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=azad shahr
    This is city#: 148
    This is: saint-malo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=saint-malo
    This is city#: 149
    This is: yashan
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=yashan
    This is city#: 150
    This is: kaabong
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kaabong
    This is city#: 151
    This is: truth or consequences
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=truth or consequences
    This is city#: 152
    This is: westwood
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=westwood
    This is city#: 153
    This is: sooke
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sooke
    This is city#: 154
    This is: dasoguz
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=dasoguz
    This is city#: 155
    This is: fuerte olimpo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=fuerte olimpo
    This is city#: 156
    This is: xinxiang
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=xinxiang
    This is city#: 157
    This is: laguna de perlas
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=laguna de perlas
    This is city#: 158
    This is: narsaq
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=narsaq
    No data for this city: kopyevo
    This is city#: 159
    This is: kopyevo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kopyevo
    This is city#: 160
    This is: petropavl
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=petropavl
    This is city#: 161
    This is: bathsheba
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bathsheba
    This is city#: 162
    This is: hamilton
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=hamilton
    This is city#: 163
    This is: taicheng
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=taicheng
    This is city#: 164
    This is: cody
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=cody
    This is city#: 165
    This is: padampur
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=padampur
    This is city#: 166
    This is: southaven
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=southaven
    This is city#: 167
    This is: tamsweg
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tamsweg
    This is city#: 168
    This is: bratsk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bratsk
    This is city#: 169
    This is: la ronge
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=la ronge
    This is city#: 170
    This is: vyazemskiy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=vyazemskiy
    This is city#: 171
    This is: batabano
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=batabano
    This is city#: 172
    This is: kuandian
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kuandian
    This is city#: 173
    This is: klyuchevskiy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=klyuchevskiy
    This is city#: 174
    This is: krasnokholmskiy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=krasnokholmskiy
    This is city#: 175
    This is: port alberni
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=port alberni
    This is city#: 176
    This is: tawau
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tawau
    This is city#: 177
    This is: chernyshevskiy
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=chernyshevskiy
    No data for this city: ust-bolsheretsk
    This is city#: 178
    This is: ust-bolsheretsk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ust-bolsheretsk
    This is city#: 179
    This is: keita
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=keita
    This is city#: 180
    This is: teguldet
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=teguldet
    No data for this city: porto santo
    This is city#: 181
    This is: porto santo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=porto santo
    This is city#: 182
    This is: brownwood
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=brownwood
    This is city#: 183
    This is: pine bluff
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=pine bluff
    No data for this city: vestbygda
    This is city#: 184
    This is: vestbygda
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=vestbygda
    This is city#: 185
    This is: two rivers
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=two rivers
    This is city#: 186
    This is: amarpur
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=amarpur
    This is city#: 187
    This is: ostrogozhsk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ostrogozhsk
    This is city#: 188
    This is: adelaide
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=adelaide
    No data for this city: kytlym
    This is city#: 189
    This is: kytlym
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=kytlym
    This is city#: 190
    This is: bogovarovo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=bogovarovo
    This is city#: 191
    This is: zvenigovo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=zvenigovo
    No data for this city: ayer itam
    This is city#: 192
    This is: ayer itam
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ayer itam
    This is city#: 193
    This is: tacna
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tacna
    This is city#: 194
    This is: stepnogorsk
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=stepnogorsk
    This is city#: 195
    This is: hukuntsi
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=hukuntsi
    This is city#: 196
    This is: etla
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=etla
    This is city#: 197
    This is: middelburg
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=middelburg
    This is city#: 198
    This is: carballo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=carballo
    No data for this city: ipora
    This is city#: 199
    This is: ipora
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ipora
    This is city#: 200
    This is: ust-omchug
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=ust-omchug
    This is city#: 201
    This is: constitucion
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=constitucion
    No data for this city: vastervik
    This is city#: 202
    This is: vastervik
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=vastervik
    This is city#: 203
    This is: sendafa
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=sendafa
    This is city#: 204
    This is: chongwe
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=chongwe
    No data for this city: villazon
    This is city#: 205
    This is: villazon
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=villazon
    This is city#: 206
    This is: phangnga
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=phangnga
    This is city#: 207
    This is: imbituba
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=imbituba
    This is city#: 208
    This is: el guamo
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=el guamo
    This is city#: 209
    This is: tapiramuta
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=tapiramuta
    This is city#: 210
    This is: veliki preslav
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=veliki preslav
    This is city#: 211
    This is: howard springs
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=howard springs
    This is city#: 212
    This is: new smyrna beach
    This is the requested URL: http://api.openweathermap.org/data/2.5/weather?appid=dbf03c8aa578b5077b6964aa9ffdba0e&units=Imperial&q=new smyrna beach
    


```python
samplecity = samplecity[samplecity.latitude != ""]
samplecity.head()
```


```python
# import datetime
# date = datetime.date.today()
date = time.strftime("%m/%d/%Y")
# print(date)
plt.scatter(samplecity['latitude'],samplecity['temperature'])
plt.title(f"Temperature (F) vs. Latitude {date}")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.style.use('ggplot')
plt.savefig("Temperature.png")
plt.grid(True)
plt.show()
```


![png](output_6_0.png)



```python
# plt.scatter(latitude,humidity)
plt.scatter(samplecity['latitude'], samplecity['humidity'])
plt.title(f"Humidity (%) vs. Latitude {date}")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.style.use('ggplot')
plt.grid(True)
plt.savefig("Humidity.png")
plt.show()
```


![png](output_7_0.png)



```python
# plt.scatter(latitude,cloudy)
plt.scatter(samplecity['latitude'], samplecity['cloudiness'])
plt.title(f"Cloudiness (%) vs. Latitude {date}")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.style.use('ggplot')
plt.grid(True)
plt.savefig("Cloudiness.png")
plt.show()
```


![png](output_8_0.png)



```python
# plt.scatter(latitude,windspeed)
plt.scatter(samplecity['latitude'], samplecity['wind_speed'])
plt.title(f"Wind Speed(mph) vs. Latitude {date}")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed(mph)")
plt.style.use('ggplot')
plt.grid(True)
plt.savefig("Wind_Speed.png")
plt.show()
```


![png](output_9_0.png)



```python
# samplecity = samplecity[samplecity.latitude != ""]
#above works to remove blank answers
#Make into CSV
samplecity.to_csv("cityweather_data.csv", encoding="utf-8", index=False)
df = pd.read_csv("cityweather_data.csv")
df.head()
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
      <th>country</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>cloudiness</th>
      <th>wind_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>portmore</td>
      <td>jm</td>
      <td>17</td>
      <td>17</td>
      <td>86</td>
      <td>70</td>
      <td>20</td>
      <td>19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>funtua</td>
      <td>ng</td>
      <td>11</td>
      <td>11</td>
      <td>73</td>
      <td>72</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>bhasawar</td>
      <td>in</td>
      <td>27</td>
      <td>27</td>
      <td>79</td>
      <td>45</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>rumonge</td>
      <td>bi</td>
      <td>-3</td>
      <td>-3</td>
      <td>71</td>
      <td>100</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>canchungo</td>
      <td>gw</td>
      <td>12</td>
      <td>12</td>
      <td>80</td>
      <td>69</td>
      <td>0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>


