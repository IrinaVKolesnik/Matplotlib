

```python
# Pyber_IK
# Imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import os
```


```python
cdata = "city_data.csv"
city = pd.read_csv(cdata, encoding = "ISO-8859-1")
#by_city = city.groupby(["city"])    //testing
#print(by_city.count())
#city.head()
```


```python
city_grouped = city.groupby("city")
city_all = pd.DataFrame({
    'type':city_grouped['type'].min(), 
    'driver_count':city_grouped['driver_count'].sum()}
)[['type', 'driver_count']].reset_index()
#city_all.head()

```


```python
rdata = "ride_data.csv"
ride = pd.read_csv(rdata, encoding = "ISO-8859-1")
#by_city = city.groupby(["city"])    //testing
#print(by_city.count())
#ride.head()

```


```python
city_ride = pd.merge(city_all, ride, on='city', how='inner')
#city_ride.head()

```


```python
cr_grouped = city_ride.groupby('city')

cr = pd.DataFrame({
    'type':cr_grouped['type'].min(), 
    'driver_count':cr_grouped['driver_count'].min(), 
    'ride_count':cr_grouped['ride_id'].count(), 
    'fare_average':cr_grouped['fare'].mean()}
)[['type', 'driver_count', 'ride_count', 'fare_average']]

cr.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>driver_count</th>
      <th>ride_count</th>
      <th>fare_average</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>Urban</td>
      <td>21</td>
      <td>31</td>
      <td>23.928710</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>Urban</td>
      <td>67</td>
      <td>26</td>
      <td>20.609615</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>Suburban</td>
      <td>16</td>
      <td>9</td>
      <td>37.315556</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>Urban</td>
      <td>21</td>
      <td>22</td>
      <td>23.625000</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>Urban</td>
      <td>49</td>
      <td>19</td>
      <td>21.981579</td>
    </tr>
  </tbody>
</table>
</div>




```python
ct_grouped = cr.groupby('type')
for name, group in ct_grouped:
    plt.scatter(group['ride_count'],
            group['fare_average'],
            s=group['driver_count']*10,
            edgecolors='black',
            label=name,
            alpha=0.75)   
                
plt.title('Pyber Ride Sharing Data (2016)')
plt.xlabel('Total Number of Rides per City')
plt.ylabel('Average Fare ($) Per City')
plt.legend(title='City Types')
plt.grid()
plt.show()
    
```


![png](output_6_0.png)



```python
# % of Total Fares by City Type
ct_grouped = city_ride.groupby('type')
ct_fare = pd.DataFrame({
    'fare_total':ct_grouped['fare'].sum()}
)[['fare_total']]
#ct_fare
```


```python
ct_fare.plot.pie("fare_total", 
            explode = (0, 0, 0.1), 
            autopct='%1.1f%%', 
            shadow=True, 
            startangle=130, 
            colors = ["gold","lightskyblue" ,"lightcoral"])
plt.title('% of Total Fares by City Type')
plt.show()
```


![png](output_8_0.png)



```python
ct_grouped = city_ride.groupby('type')
ct_rides = pd.DataFrame({
    'rides_total':ct_grouped['ride_id'].count()}
)[['rides_total']]
#ct_rides
```


```python
# '% of Total Rades by City Type
ct_rides.plot.pie("rides_total", 
            explode = (0, 0, 0.1), 
            autopct='%1.1f%%', 
            shadow=True, 
            startangle=130, 
            colors = ["gold","lightskyblue" ,"lightcoral"])
plt.title('% of Total Rades by City Type')
plt.show()
```


![png](output_10_0.png)



```python
cd_grouped = cr.groupby('type')
cd_drivers = pd.DataFrame({
    'drivers_total':cd_grouped['driver_count'].sum()}
)[['drivers_total']]
#cd_drivers
```


```python
# % of Total Drivers by City Type
cd_drivers.plot.pie("drivers_total", 
            explode = (0, 0, 0.1), 
            autopct='%1.1f%%', 
            shadow=True, 
            startangle=130, 
            colors = ["gold","lightskyblue" ,"lightcoral"])
plt.title('% of Total Drivers by City Type')
plt.show()
```


![png](output_12_0.png)

