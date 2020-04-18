## Data Source

Novel Coronavirus (COVID-19) Cases, provided by JHU CSSE: https://github.com/CSSEGISandData/COVID-19


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import random
```

## Import Data from COVID-19 time series

https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series 


```python
confirmed_df  = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv')
deaths_df     = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv')
recoveries_df = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv')
```

COVID-19 Daily Reports

https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_daily_reports


```python
latest_data = pd.read_csv('https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_daily_reports/04-16-2020.csv')
```


```python
latest_data.describe()
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
      <th>FIPS</th>
      <th>Lat</th>
      <th>Long_</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2774.000000</td>
      <td>2982.000000</td>
      <td>2982.000000</td>
      <td>3041.000000</td>
      <td>3041.000000</td>
      <td>3041.000000</td>
      <td>3041.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>31274.175919</td>
      <td>36.653580</td>
      <td>-80.998891</td>
      <td>707.874712</td>
      <td>47.287405</td>
      <td>178.266031</td>
      <td>482.321276</td>
    </tr>
    <tr>
      <th>std</th>
      <td>17268.589814</td>
      <td>9.926478</td>
      <td>40.254442</td>
      <td>7132.717652</td>
      <td>726.392449</td>
      <td>2862.257922</td>
      <td>4692.124431</td>
    </tr>
    <tr>
      <th>min</th>
      <td>66.000000</td>
      <td>-51.796300</td>
      <td>-164.035380</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-54703.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>18103.500000</td>
      <td>33.780007</td>
      <td>-95.769967</td>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>29142.000000</td>
      <td>37.843679</td>
      <td>-87.469020</td>
      <td>16.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>15.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>46086.500000</td>
      <td>41.517404</td>
      <td>-81.090960</td>
      <td>79.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>69.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>99999.000000</td>
      <td>71.706900</td>
      <td>178.065000</td>
      <td>184948.000000</td>
      <td>22170.000000</td>
      <td>77000.000000</td>
      <td>111669.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
latest_data.head()
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
      <th>FIPS</th>
      <th>Admin2</th>
      <th>Province_State</th>
      <th>Country_Region</th>
      <th>Last_Update</th>
      <th>Lat</th>
      <th>Long_</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Combined_Key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45001.0</td>
      <td>Abbeville</td>
      <td>South Carolina</td>
      <td>US</td>
      <td>2020-04-16 23:30:51</td>
      <td>34.223334</td>
      <td>-82.461707</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>Abbeville, South Carolina, US</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22001.0</td>
      <td>Acadia</td>
      <td>Louisiana</td>
      <td>US</td>
      <td>2020-04-16 23:30:51</td>
      <td>30.295065</td>
      <td>-92.414197</td>
      <td>108</td>
      <td>6</td>
      <td>0</td>
      <td>102</td>
      <td>Acadia, Louisiana, US</td>
    </tr>
    <tr>
      <th>2</th>
      <td>51001.0</td>
      <td>Accomack</td>
      <td>Virginia</td>
      <td>US</td>
      <td>2020-04-16 23:30:51</td>
      <td>37.767072</td>
      <td>-75.632346</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>19</td>
      <td>Accomack, Virginia, US</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16001.0</td>
      <td>Ada</td>
      <td>Idaho</td>
      <td>US</td>
      <td>2020-04-16 23:30:51</td>
      <td>43.452658</td>
      <td>-116.241552</td>
      <td>567</td>
      <td>9</td>
      <td>0</td>
      <td>558</td>
      <td>Ada, Idaho, US</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19001.0</td>
      <td>Adair</td>
      <td>Iowa</td>
      <td>US</td>
      <td>2020-04-16 23:30:51</td>
      <td>41.330756</td>
      <td>-94.471059</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>Adair, Iowa, US</td>
    </tr>
  </tbody>
</table>
</div>



## Get all the dates for the pandemic data


```python
cols = confirmed_df.keys()
cols
```




    Index(['Province/State', 'Country/Region', 'Lat', 'Long', '1/22/20', '1/23/20',
           '1/24/20', '1/25/20', '1/26/20', '1/27/20', '1/28/20', '1/29/20',
           '1/30/20', '1/31/20', '2/1/20', '2/2/20', '2/3/20', '2/4/20', '2/5/20',
           '2/6/20', '2/7/20', '2/8/20', '2/9/20', '2/10/20', '2/11/20', '2/12/20',
           '2/13/20', '2/14/20', '2/15/20', '2/16/20', '2/17/20', '2/18/20',
           '2/19/20', '2/20/20', '2/21/20', '2/22/20', '2/23/20', '2/24/20',
           '2/25/20', '2/26/20', '2/27/20', '2/28/20', '2/29/20', '3/1/20',
           '3/2/20', '3/3/20', '3/4/20', '3/5/20', '3/6/20', '3/7/20', '3/8/20',
           '3/9/20', '3/10/20', '3/11/20', '3/12/20', '3/13/20', '3/14/20',
           '3/15/20', '3/16/20', '3/17/20', '3/18/20', '3/19/20', '3/20/20',
           '3/21/20', '3/22/20', '3/23/20', '3/24/20', '3/25/20', '3/26/20',
           '3/27/20', '3/28/20', '3/29/20', '3/30/20', '3/31/20', '4/1/20',
           '4/2/20', '4/3/20', '4/4/20', '4/5/20', '4/6/20', '4/7/20', '4/8/20',
           '4/9/20', '4/10/20', '4/11/20', '4/12/20', '4/13/20', '4/14/20',
           '4/15/20', '4/16/20', '4/17/20'],
          dtype='object')




```python
# Printing the dates ONLY
cols[4:]
```




    Index(['1/22/20', '1/23/20', '1/24/20', '1/25/20', '1/26/20', '1/27/20',
           '1/28/20', '1/29/20', '1/30/20', '1/31/20', '2/1/20', '2/2/20',
           '2/3/20', '2/4/20', '2/5/20', '2/6/20', '2/7/20', '2/8/20', '2/9/20',
           '2/10/20', '2/11/20', '2/12/20', '2/13/20', '2/14/20', '2/15/20',
           '2/16/20', '2/17/20', '2/18/20', '2/19/20', '2/20/20', '2/21/20',
           '2/22/20', '2/23/20', '2/24/20', '2/25/20', '2/26/20', '2/27/20',
           '2/28/20', '2/29/20', '3/1/20', '3/2/20', '3/3/20', '3/4/20', '3/5/20',
           '3/6/20', '3/7/20', '3/8/20', '3/9/20', '3/10/20', '3/11/20', '3/12/20',
           '3/13/20', '3/14/20', '3/15/20', '3/16/20', '3/17/20', '3/18/20',
           '3/19/20', '3/20/20', '3/21/20', '3/22/20', '3/23/20', '3/24/20',
           '3/25/20', '3/26/20', '3/27/20', '3/28/20', '3/29/20', '3/30/20',
           '3/31/20', '4/1/20', '4/2/20', '4/3/20', '4/4/20', '4/5/20', '4/6/20',
           '4/7/20', '4/8/20', '4/9/20', '4/10/20', '4/11/20', '4/12/20',
           '4/13/20', '4/14/20', '4/15/20', '4/16/20', '4/17/20'],
          dtype='object')



## Get the COVID-19 confirmed-deaths-recoveries data


```python
confirmed  = confirmed_df.loc[:, cols[4]:]
deaths     = deaths_df.loc[:, cols[4]:]
recoveries = recoveries_df.loc[:, cols[4]:]
```


```python
confirmed.head()
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
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>1/31/20</th>
      <th>...</th>
      <th>4/8/20</th>
      <th>4/9/20</th>
      <th>4/10/20</th>
      <th>4/11/20</th>
      <th>4/12/20</th>
      <th>4/13/20</th>
      <th>4/14/20</th>
      <th>4/15/20</th>
      <th>4/16/20</th>
      <th>4/17/20</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>444</td>
      <td>484</td>
      <td>521</td>
      <td>555</td>
      <td>607</td>
      <td>665</td>
      <td>714</td>
      <td>784</td>
      <td>840</td>
      <td>906</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>400</td>
      <td>409</td>
      <td>416</td>
      <td>433</td>
      <td>446</td>
      <td>467</td>
      <td>475</td>
      <td>494</td>
      <td>518</td>
      <td>539</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1572</td>
      <td>1666</td>
      <td>1761</td>
      <td>1825</td>
      <td>1914</td>
      <td>1983</td>
      <td>2070</td>
      <td>2160</td>
      <td>2268</td>
      <td>2418</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>564</td>
      <td>583</td>
      <td>601</td>
      <td>601</td>
      <td>638</td>
      <td>646</td>
      <td>659</td>
      <td>673</td>
      <td>673</td>
      <td>696</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 87 columns</p>
</div>




```python
# RECALL
confirmed_df.head()
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
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Lat</th>
      <th>Long</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>...</th>
      <th>4/8/20</th>
      <th>4/9/20</th>
      <th>4/10/20</th>
      <th>4/11/20</th>
      <th>4/12/20</th>
      <th>4/13/20</th>
      <th>4/14/20</th>
      <th>4/15/20</th>
      <th>4/16/20</th>
      <th>4/17/20</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>Afghanistan</td>
      <td>33.0000</td>
      <td>65.0000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>444</td>
      <td>484</td>
      <td>521</td>
      <td>555</td>
      <td>607</td>
      <td>665</td>
      <td>714</td>
      <td>784</td>
      <td>840</td>
      <td>906</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>Albania</td>
      <td>41.1533</td>
      <td>20.1683</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>400</td>
      <td>409</td>
      <td>416</td>
      <td>433</td>
      <td>446</td>
      <td>467</td>
      <td>475</td>
      <td>494</td>
      <td>518</td>
      <td>539</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>Algeria</td>
      <td>28.0339</td>
      <td>1.6596</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1572</td>
      <td>1666</td>
      <td>1761</td>
      <td>1825</td>
      <td>1914</td>
      <td>1983</td>
      <td>2070</td>
      <td>2160</td>
      <td>2268</td>
      <td>2418</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>Andorra</td>
      <td>42.5063</td>
      <td>1.5218</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>564</td>
      <td>583</td>
      <td>601</td>
      <td>601</td>
      <td>638</td>
      <td>646</td>
      <td>659</td>
      <td>673</td>
      <td>673</td>
      <td>696</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Angola</td>
      <td>-11.2027</td>
      <td>17.8739</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 91 columns</p>
</div>




```python
dates = confirmed.keys()
world_cases     = [confirmed[i].sum() for i in dates]
total_deaths    = [deaths[i].sum() for i in dates] 
total_recovered = [recoveries[i].sum() for i in dates] 
```


```python
total_active    = (np.array(world_cases)-np.array(total_deaths)-np.array(total_recovered)).tolist()
```


```python
mortality_rate  = (np.array(total_deaths)/np.array(world_cases)).tolist()
recovery_rate   = (np.array(total_recovered)/np.array(world_cases)).tolist()
```

## Convert time string to actual datetime objects


```python
import datetime

start = '1/22/2020'
start_date = datetime.datetime.strptime(start, '%m/%d/%Y')
dates_dt = [(start_date + datetime.timedelta(days=i)).strftime('%m/%d/%Y') for i in range(len(dates))]
```

## Plot Coronavirus Over Time


```python
covid_df = pd.DataFrame(
    {'Confirmed': world_cases,
     'Deaths'   : total_deaths,
     'Recovered': total_recovered,
     'Active'   : total_active
    }, index=dates_dt)
covid_df.plot(figsize=(16, 9), linewidth=4).grid(axis='y')
plt.title('Coronavirus Over Time', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_21_0.png)



```python
covid_df.pct_change().plot(figsize=(16, 9), linewidth=4).grid(axis='y')
plt.title('Daily Change Over Time (in %)', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_22_0.png)



```python
daily_change = covid_df.diff()
daily_change.plot.bar(figsize=(16, 9), linewidth=4, width=2).grid(axis='y')
plt.title('Daily Change Over Time', size=30)
plt.xticks(rotation=45);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_23_0.png)



```python
daily_change.columns
```




    Index(['Confirmed', 'Deaths', 'Recovered', 'Active'], dtype='object')




```python
daily_change.Confirmed.plot.bar(figsize=(20, 8), linewidth=4, width=0.8).grid(axis='y')
plt.title('Global Confirmed COVID-19 Cases Daily Change Over Time', size=30)
#plt.xticks(rotation=45);
```




    Text(0.5, 1.0, 'Global Confirmed COVID-19 Cases Daily Change Over Time')




![_config.yml]({{ site.baseurl}}/images/20200418_output_25_1.png)



```python
daily_change.Deaths.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='darkgoldenrod').grid(axis='y')
plt.title('Global Daily Deaths from COVID-19 Over Time', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_26_0.png)



```python
daily_change.Recovered.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='green').grid(axis='y')
plt.title('Global Daily Recovered Cases from COVID-19 Over Time', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_27_0.png)



```python
daily_change.Active.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='firebrick').grid(axis='y')
plt.title('Global Daily Change in COVID-19 Active Cases', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_28_0.png)



```python
daily_change.Confirmed.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, label='Confirmed Cases', legend=True).grid(axis='y')
daily_change.Active.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='firebrick', label='Active', legend=True).grid(axis='y')
daily_change.Recovered.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='green', label='Recovered', legend=True).grid(axis='y')
daily_change.Deaths.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='darkgoldenrod', label='Deaths', legend=True).grid(axis='y')
plt.title('Global COVID-19 Daily Changes', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_29_0.png)



```python
def get_data(country):
    cases = [confirmed_df[confirmed_df['Country/Region']==country][i].sum() for i in dates]
    daily_increase = pd.Series(cases).diff().tolist()
    deaths = [deaths_df[deaths_df['Country/Region']==country][i].sum() for i in dates]
    daily_death = pd.Series(deaths).diff().tolist()
    recoveries  = [recoveries_df[recoveries_df['Country/Region']==country][i].sum() for i in dates]
    daily_recovery = pd.Series(recoveries).diff().tolist()
    return (cases, deaths, recoveries, daily_increase, daily_death, daily_recovery)

def country_plot(x, y1, y2, y3, y4, country):
    plt.figure(figsize=(16, 9))
    plt.plot(x, y1, linewidth=4)
    plt.title('{} Confirmed Cases'.format(country), size=30)
    plt.ylabel('# of Cases', size=30)
    plt.xticks(size=10)
    plt.xticks(rotation=90)
    plt.yticks(size=20)
    plt.grid(axis='y')
    plt.show()

    plt.figure(figsize=(16, 9))
    plt.bar(x, y2)
    plt.title('{} Daily Increases in Confirmed Cases'.format(country), size=30)
    plt.ylabel('# of Cases', size=30)
    plt.xticks(size=10)
    plt.xticks(rotation=90)
    plt.yticks(size=20)
    plt.grid(axis='y')
    plt.show()

    plt.figure(figsize=(16, 9))
    plt.bar(x, y3, color='darkgoldenrod')
    plt.title('{} Daily Increases in Deaths'.format(country), size=30)
    plt.ylabel('# of Cases', size=30)
    plt.xticks(size=10)
    plt.xticks(rotation=90)
    plt.yticks(size=20)
    plt.grid(axis='y')
    plt.show()

    plt.figure(figsize=(16, 9))
    plt.bar(x, y4, color='green')
    plt.title('{} Daily Increases in Recoveries'.format(country), size=30)
    plt.ylabel('# of Cases', size=30)
    plt.xticks(size=10)
    plt.xticks(rotation=90)
    plt.yticks(size=20)
    plt.grid(axis='y')
    plt.show()
```


```python
country = 'China'
china_cases, china_deaths, china_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
country_plot(dates, china_cases, daily_increase, daily_death, daily_recovery, country)
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_31_0.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_31_1.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_31_2.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_31_3.png)



```python
country = 'US'
us_cases, us_deaths, us_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
country_plot(dates, us_cases, daily_increase, daily_death, daily_recovery, country)
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_32_0.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_32_1.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_32_2.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_32_3.png)



```python
country = 'Italy'
italy_cases, italy_deaths, italy_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
country_plot(dates, italy_cases, daily_increase, daily_death, daily_recovery, country)
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_33_0.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_33_1.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_33_2.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_33_3.png)



```python
country = 'Spain'
spain_cases, spain_deaths, spain_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
# country_plot(dates, spain_cases, daily_increase, daily_death, daily_recovery, country)
```


```python
country = 'France'
france_cases, france_deaths, france_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
# country_plot(dates, france_cases, daily_increase, daily_death, daily_recovery, country)
```


```python
country = 'Germany'
germany_cases, germany_deaths, germany_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
# country_plot(dates, germany_cases, daily_increase, daily_death, daily_recovery, country)
```


```python
country = 'Philippines'
philippine_cases, philippine_deaths, philippine_recoveries, daily_increase, daily_death,daily_recovery = get_data(country)
country_plot(dates, philippine_cases, daily_increase, daily_death, daily_recovery, country)
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_37_0.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_37_1.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_37_2.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_37_3.png)



```python
# philippine_cases, philippine_deaths, philippine_recoveries, daily_increase, daily_death,daily_recovery
philippine_total_active = (np.array(philippine_cases)-np.array(philippine_deaths)-np.array(philippine_recoveries)).tolist()
philippine_covid_df = pd.DataFrame(
    {'Confirmed': philippine_cases,
     'Deaths'   : philippine_deaths,
     'Recovered': philippine_recoveries,
     'Active'   : philippine_total_active
    }, index=dates_dt)
covid_df.plot(figsize=(16, 9), linewidth=4).grid(axis='y')
plt.title('Philippine Coronavirus Over Time', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_38_0.png)



```python
ph_daily_change = philippine_covid_df.diff()
ph_daily_change.Confirmed.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, label='Confirmed Cases', legend=True).grid(axis='y')
ph_daily_change.Active.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='firebrick', label='Active', legend=True).grid(axis='y')
ph_daily_change.Recovered.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='green', label='Recovered', legend=True).grid(axis='y')
ph_daily_change.Deaths.plot.bar(figsize=(20, 8), linewidth=4, width=0.8, color='darkgoldenrod', label='Deaths', legend=True).grid(axis='y')
plt.title('Philippine COVID-19 Daily Changes', size=30)
plt.xlabel('Days', size=30)
plt.ylabel('# of Cases', size=30);
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_39_0.png)



```python
plt.figure(figsize=(16, 9))
plt.plot(dates, china_cases, 'ro-', markersize=8)
plt.plot(dates, italy_cases, 'gv-', markersize=8)
plt.plot(dates, us_cases, 'bs-', markersize=8)
plt.plot(dates, spain_cases, 'mp-', markersize=8)
plt.plot(dates, france_cases, 'y*-', markersize=8)
plt.plot(dates, germany_cases, 'cd-', markersize=8)
plt.plot(dates, philippine_cases, 'k.-', markersize=8)
plt.title('# of Coronavirus Cases', size=30)
plt.xlabel('Days', size=30)
plt.ylabel('# of Cases', size=30)
plt.legend(['China', 'Italy', 'US', 'Spain', 'France', 'Germany','Philippines'], prop={'size': 20})
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()

plt.figure(figsize=(16, 9))
plt.plot(dates, china_deaths, 'ro-', markersize=8)
plt.plot(dates, italy_deaths, 'gv-', markersize=8)
plt.plot(dates, us_deaths, 'bs-', markersize=8)
plt.plot(dates, spain_deaths, 'mp-', markersize=8)
plt.plot(dates, france_deaths, 'y*-', markersize=8)
plt.plot(dates, germany_deaths, 'cd-', markersize=8)
plt.plot(dates, philippine_deaths, 'k.-', markersize=8)
plt.title('# of Coronavirus Deaths', size=30)
plt.xlabel('Days', size=30)
plt.ylabel('# of Cases', size=30)
plt.legend(['China', 'Italy', 'US', 'Spain', 'France', 'Germany','Philippines'], prop={'size': 20})
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()

plt.figure(figsize=(16, 9))
plt.plot(dates, china_recoveries, 'ro-', markersize=8)
plt.plot(dates, italy_recoveries, 'gv-', markersize=8)
plt.plot(dates, us_recoveries, 'bs-', markersize=8)
plt.plot(dates, spain_recoveries, 'mp-', markersize=8)
plt.plot(dates, france_recoveries, 'y*-', markersize=8)
plt.plot(dates, germany_recoveries, 'cd-', markersize=8)
plt.plot(dates, philippine_recoveries, 'k.-', markersize=8)
plt.title('# of Coronavirus Recoveries', size=30)
plt.xlabel('Days', size=30)
plt.ylabel('# of Cases', size=30)
plt.legend(['China', 'Italy', 'US', 'Spain', 'France', 'Germany','Philippines'], prop={'size': 20})
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_40_0.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_40_1.png)



![_config.yml]({{ site.baseurl}}/images/20200418_output_40_2.png)



```python
fig, axs = plt.subplots(6,1, figsize=(15, 64), facecolor='w', edgecolor='k')
fig.subplots_adjust(hspace = .5)
axs = axs.ravel()

ASEAN = ['Indonesia', 'Malaysia', 'Philippines', 'Singapore', 'Thailand', 'Brunei', 'Laos', 'Myanmar', 'Cambodia', 'Vietnam']
 
for country in ASEAN:    
    cases, deaths, recoveries, daily_increase, daily_death, daily_recovery = get_data(country)
    
    axs[0].plot(dates, cases, markersize=8, linewidth=4)
    axs[0].set_title('# of Coronavirus Cases', size=30)
    axs[0].set_xlabel('Days', size=30)
    axs[0].set_ylabel('# of Cases', size=30)
    axs[0].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[0].set_xticklabels(dates, rotation=90)

    
    axs[1].plot(dates, deaths, markersize=8, linewidth=4)
    axs[1].set_title('# of Coronavirus Deaths', size=30)
    axs[1].set_xlabel('Days', size=30)
    axs[1].set_ylabel('# of Cases', size=30)
    axs[1].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[1].set_xticklabels(dates, rotation=90)


    axs[2].plot(dates, recoveries, markersize=8, linewidth=4)
    axs[2].set_title('# of Coronavirus Recoveries', size=30)
    axs[2].set_xlabel('Days', size=30)
    axs[2].set_ylabel('# of Cases', size=30)
    axs[2].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[2].set_xticklabels(dates, rotation=90)

    
    axs[3].plot(dates, daily_increase, markersize=8, linewidth=4)
    axs[3].set_title('# of Coronavirus Daily Increase', size=30)
    axs[3].set_xlabel('Days', size=30)
    axs[3].set_ylabel('# of Cases', size=30)
    axs[3].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[3].set_xticklabels(dates, rotation=90)


    axs[4].plot(dates, daily_death, markersize=8, linewidth=4)
    axs[4].set_title('# of Coronavirus Daily Deaths', size=30)
    axs[4].set_xlabel('Days', size=30)
    axs[4].set_ylabel('# of Cases', size=30)
    axs[4].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[4].set_xticklabels(dates, rotation=90)

    
    axs[5].plot(dates, daily_recovery, markersize=8, linewidth=4)
    axs[5].set_title('# of Coronavirus Recoveries Daily Increase', size=30)
    axs[5].set_xlabel('Days', size=30)
    axs[5].set_ylabel('# of Cases', size=30)
    axs[5].legend(ASEAN, prop={'size': 20}, loc="upper left")
    axs[5].set_xticklabels(dates, rotation=90)

```


![_config.yml]({{ site.baseurl}}/images/20200418_output_41_0.png)


## Mortality


```python
mean_mortality_rate = np.mean(mortality_rate)
plt.figure(figsize=(16, 9))
plt.plot(dates, mortality_rate, color='orange', linewidth=4)
plt.axhline(y = mean_mortality_rate,linestyle='--', color='black')
plt.title('Mortality Rate of Coronavirus Over Time', size=30)
plt.legend(['mortality rate', 'y='+str(mean_mortality_rate)], prop={'size': 20})
plt.xlabel('Days', size=30)
plt.ylabel('Mortality Rate', size=30)
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_43_0.png)


## Recovery Rate


```python
mean_recovery_rate = np.mean(recovery_rate)
plt.figure(figsize=(16, 9))
plt.plot(dates, recovery_rate, color='blue', linewidth=4)
plt.axhline(y = mean_recovery_rate,linestyle='--', color='black')
plt.title('Recovery Rate of Coronavirus Over Time', size=30)
plt.legend(['recovery rate', 'y='+str(mean_recovery_rate)], prop={'size': 20})
plt.xlabel('Days', size=30)
plt.ylabel('Recovery Rate', size=30)
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_45_0.png)



```python
plt.figure(figsize=(16, 9))
plt.plot(dates, total_deaths, color='red', linewidth=4)
plt.plot(dates, total_recovered, color='green', linewidth=4)
plt.legend(['death', 'recoveries'], loc='best', fontsize=20)
plt.title('# of Coronavirus Cases', size=30)
plt.xlabel('Days', size=30)
plt.ylabel('# of Cases', size=30)
plt.xticks(rotation=90)
plt.yticks(size=20)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_46_0.png)


## COVID-19 By Country


```python
import operator

unique_countries =  list(latest_data['Country_Region'].unique())

country_confirmed_cases = []
country_death_cases = [] 
country_active_cases = []
country_recovery_cases = []
country_mortality_rate = [] 

no_cases = []
for i in unique_countries:
    cases = latest_data[latest_data['Country_Region']==i]['Confirmed'].sum()
    if cases > 0:
        country_confirmed_cases.append(cases)
    else:
        no_cases.append(i)
        
for i in no_cases:
    unique_countries.remove(i)
    
# sort countries by the number of confirmed cases
unique_countries = [k for k, v in sorted(zip(unique_countries, country_confirmed_cases), key=operator.itemgetter(1), reverse=True)]
for i in range(len(unique_countries)):
    country_confirmed_cases[i] = latest_data[latest_data['Country_Region']==unique_countries[i]]['Confirmed'].sum()
    country_death_cases.append(latest_data[latest_data['Country_Region']==unique_countries[i]]['Deaths'].sum())
    country_recovery_cases.append(latest_data[latest_data['Country_Region']==unique_countries[i]]['Recovered'].sum())
    country_active_cases.append(country_confirmed_cases[i] - country_death_cases[i] - country_recovery_cases[i])
    country_mortality_rate.append(country_death_cases[i]/country_confirmed_cases[i])
```


```python
country_df = pd.DataFrame({'Country Name': unique_countries, 'Number of Confirmed Cases': country_confirmed_cases,
                          'Number of Deaths': country_death_cases, 'Number of Recoveries' : country_recovery_cases, 
                          'Number of Active Cases' : country_active_cases,
                          'Mortality Rate': country_mortality_rate})
# number of cases per country/region

country_df.style.background_gradient(cmap='Greens')
```




<style  type="text/css" >
    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col1 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col2 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col3 {
            background-color:  #2f984f;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col4 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col5 {
            background-color:  #ceecc8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col1 {
            background-color:  #c0e6b9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col2 {
            background-color:  #50b264;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col3 {
            background-color:  #005221;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col4 {
            background-color:  #ddf2d8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col5 {
            background-color:  #7fc97f;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col1 {
            background-color:  #c7e9c0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col2 {
            background-color:  #359e53;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col3 {
            background-color:  #6ec173;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col4 {
            background-color:  #d7efd1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col5 {
            background-color:  #4eb264;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col1 {
            background-color:  #ceecc8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col2 {
            background-color:  #62bb6d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col3 {
            background-color:  #90d18d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col4 {
            background-color:  #dbf1d6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col5 {
            background-color:  #60ba6c;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col1 {
            background-color:  #d2edcc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col2 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col3 {
            background-color:  #00491d;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col4 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col5 {
            background-color:  #e4f5df;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col1 {
            background-color:  #def2d9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col2 {
            background-color:  #91d28e;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col4 {
            background-color:  #def2d9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col5 {
            background-color:  #4db163;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col1 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col2 {
            background-color:  #e8f6e4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col3 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col5 {
            background-color:  #d8f0d2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col1 {
            background-color:  #e7f6e2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col2 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col3 {
            background-color:  #37a055;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col4 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col5 {
            background-color:  #bee5b8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col1 {
            background-color:  #e7f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col2 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col3 {
            background-color:  #eaf7e6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col4 {
            background-color:  #e7f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col1 {
            background-color:  #f0f9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col2 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col3 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col4 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col5 {
            background-color:  #40aa5d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col1 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col2 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col3 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col4 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col5 {
            background-color:  #d7efd1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col1 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col2 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col3 {
            background-color:  #d9f0d3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col4 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col5 {
            background-color:  #bde5b6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col1 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col2 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col4 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col5 {
            background-color:  #70c274;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col1 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col3 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col4 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col5 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col1 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col2 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col3 {
            background-color:  #d3eecd;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col5 {
            background-color:  #cfecc9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col1 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col4 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col5 {
            background-color:  #dff3da;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col1 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col3 {
            background-color:  #e7f6e2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col1 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col3 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col5 {
            background-color:  #dff3da;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col1 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col4 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col5 {
            background-color:  #dbf1d6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col1 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col3 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col5 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col1 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col2 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col5 {
            background-color:  #7cc87c;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col1 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col3 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col1 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col3 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col1 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col3 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col5 {
            background-color:  #f0f9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col1 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col5 {
            background-color:  #eaf7e6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col1 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col5 {
            background-color:  #ceecc8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col1 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col5 {
            background-color:  #d9f0d3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col3 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col5 {
            background-color:  #ccebc6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col3 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col5 {
            background-color:  #d2edcc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col3 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col3 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col5 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col5 {
            background-color:  #e6f5e1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col3 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col5 {
            background-color:  #aadda4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col5 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col5 {
            background-color:  #bce4b5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col5 {
            background-color:  #97d492;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col3 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col5 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col5 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col5 {
            background-color:  #cbeac4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col5 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col5 {
            background-color:  #eaf7e6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col3 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col5 {
            background-color:  #d3eecd;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col5 {
            background-color:  #afdfa8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col1 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col3 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col5 {
            background-color:  #d3eecd;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col5 {
            background-color:  #c6e8bf;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col5 {
            background-color:  #319a50;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col5 {
            background-color:  #d0edca;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col5 {
            background-color:  #e7f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col5 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col5 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col5 {
            background-color:  #9cd797;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col5 {
            background-color:  #dbf1d5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col5 {
            background-color:  #e7f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col5 {
            background-color:  #c7e9c0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col5 {
            background-color:  #f0f9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col5 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col5 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col5 {
            background-color:  #f0f9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col5 {
            background-color:  #cfecc9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col5 {
            background-color:  #dbf1d6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col5 {
            background-color:  #edf8ea;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col5 {
            background-color:  #d5efcf;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col5 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col5 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col5 {
            background-color:  #e1f3dc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col5 {
            background-color:  #dcf2d7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col5 {
            background-color:  #d3eecd;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col5 {
            background-color:  #d0edca;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col5 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col5 {
            background-color:  #ceecc8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col5 {
            background-color:  #e1f3dc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col5 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col5 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col5 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col5 {
            background-color:  #e8f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col5 {
            background-color:  #c3e7bc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col5 {
            background-color:  #cdecc7;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col5 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col5 {
            background-color:  #e7f6e3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col5 {
            background-color:  #e4f5df;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col5 {
            background-color:  #bae3b3;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col5 {
            background-color:  #a3da9d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col5 {
            background-color:  #98d594;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col5 {
            background-color:  #edf8ea;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col5 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col5 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col5 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col5 {
            background-color:  #eff9eb;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col5 {
            background-color:  #a3da9d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col5 {
            background-color:  #e4f5df;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col5 {
            background-color:  #d0edca;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col5 {
            background-color:  #d4eece;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col5 {
            background-color:  #e7f6e2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col5 {
            background-color:  #d2edcc;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col5 {
            background-color:  #abdda5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col5 {
            background-color:  #dbf1d6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col5 {
            background-color:  #ddf2d8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col5 {
            background-color:  #d5efcf;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col5 {
            background-color:  #b4e1ad;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col5 {
            background-color:  #d5efcf;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col5 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col5 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col5 {
            background-color:  #d0edca;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col5 {
            background-color:  #bee5b8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col5 {
            background-color:  #bde5b6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col5 {
            background-color:  #b8e3b2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col5 {
            background-color:  #83cb82;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col5 {
            background-color:  #78c679;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col5 {
            background-color:  #349d53;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col5 {
            background-color:  #eaf7e6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col5 {
            background-color:  #d6efd0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col5 {
            background-color:  #afdfa8;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col5 {
            background-color:  #c1e6ba;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col5 {
            background-color:  #2e964d;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col5 {
            background-color:  #50b264;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col5 {
            background-color:  #50b264;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col5 {
            background-color:  #7dc87e;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col5 {
            background-color:  #73c476;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col5 {
            background-color:  #bde5b6;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col5 {
            background-color:  #5ab769;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col5 {
            background-color:  #b8e3b2;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col5 {
            background-color:  #86cc85;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col5 {
            background-color:  #73c476;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col5 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col5 {
            background-color:  #73c476;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col5 {
            background-color:  #3da65a;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col5 {
            background-color:  #006428;
            color:  #f1f1f1;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col1 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }</style><table id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9d" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Country Name</th>        <th class="col_heading level0 col1" >Number of Confirmed Cases</th>        <th class="col_heading level0 col2" >Number of Deaths</th>        <th class="col_heading level0 col3" >Number of Recoveries</th>        <th class="col_heading level0 col4" >Number of Active Cases</th>        <th class="col_heading level0 col5" >Mortality Rate</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row0" class="row_heading level0 row0" >0</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col0" class="data row0 col0" >US</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col1" class="data row0 col1" >667801</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col2" class="data row0 col2" >32916</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col3" class="data row0 col3" >54703</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col4" class="data row0 col4" >580182</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow0_col5" class="data row0 col5" >0.049290</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col0" class="data row1 col0" >Spain</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col1" class="data row1 col1" >184948</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col2" class="data row1 col2" >19315</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col3" class="data row1 col3" >74797</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col4" class="data row1 col4" >90836</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow1_col5" class="data row1 col5" >0.104435</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col0" class="data row2 col0" >Italy</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col1" class="data row2 col1" >168941</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col2" class="data row2 col2" >22170</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col3" class="data row2 col3" >40164</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col4" class="data row2 col4" >106607</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow2_col5" class="data row2 col5" >0.131229</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row3" class="row_heading level0 row3" >3</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col0" class="data row3 col0" >France</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col1" class="data row3 col1" >147091</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col2" class="data row3 col2" >17941</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col3" class="data row3 col3" >33327</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col4" class="data row3 col4" >95823</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow3_col5" class="data row3 col5" >0.121972</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row4" class="row_heading level0 row4" >4</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col0" class="data row4 col0" >Germany</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col1" class="data row4 col1" >137698</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col2" class="data row4 col2" >4052</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col3" class="data row4 col3" >77000</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col4" class="data row4 col4" >56646</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow4_col5" class="data row4 col5" >0.029427</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row5" class="row_heading level0 row5" >5</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col0" class="data row5 col0" >United Kingdom</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col1" class="data row5 col1" >104145</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col2" class="data row5 col2" >13759</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col3" class="data row5 col3" >375</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col4" class="data row5 col4" >90011</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow5_col5" class="data row5 col5" >0.132114</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row6" class="row_heading level0 row6" >6</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col0" class="data row6 col0" >China</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col1" class="data row6 col1" >83403</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col2" class="data row6 col2" >3346</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col3" class="data row6 col3" >78401</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col4" class="data row6 col4" >1656</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow6_col5" class="data row6 col5" >0.040118</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row7" class="row_heading level0 row7" >7</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col0" class="data row7 col0" >Iran</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col1" class="data row7 col1" >77995</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col2" class="data row7 col2" >4869</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col3" class="data row7 col3" >52229</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col4" class="data row7 col4" >20897</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow7_col5" class="data row7 col5" >0.062427</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row8" class="row_heading level0 row8" >8</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col0" class="data row8 col0" >Turkey</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col1" class="data row8 col1" >74193</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col2" class="data row8 col2" >1643</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col3" class="data row8 col3" >7089</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col4" class="data row8 col4" >65461</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow8_col5" class="data row8 col5" >0.022145</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row9" class="row_heading level0 row9" >9</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col0" class="data row9 col0" >Belgium</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col1" class="data row9 col1" >34809</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col2" class="data row9 col2" >4857</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col3" class="data row9 col3" >7562</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col4" class="data row9 col4" >22390</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow9_col5" class="data row9 col5" >0.139533</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row10" class="row_heading level0 row10" >10</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col0" class="data row10 col0" >Canada</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col1" class="data row10 col1" >30809</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col2" class="data row10 col2" >1258</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col3" class="data row10 col3" >9698</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col4" class="data row10 col4" >19853</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow10_col5" class="data row10 col5" >0.040832</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row11" class="row_heading level0 row11" >11</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col0" class="data row11 col0" >Brazil</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col1" class="data row11 col1" >30425</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col2" class="data row11 col2" >1924</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col3" class="data row11 col3" >14026</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col4" class="data row11 col4" >14475</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow11_col5" class="data row11 col5" >0.063237</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row12" class="row_heading level0 row12" >12</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col0" class="data row12 col0" >Netherlands</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col1" class="data row12 col1" >29383</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col2" class="data row12 col2" >3327</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col3" class="data row12 col3" >311</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col4" class="data row12 col4" >25745</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow12_col5" class="data row12 col5" >0.113229</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row13" class="row_heading level0 row13" >13</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col0" class="data row13 col0" >Russia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col1" class="data row13 col1" >27938</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col2" class="data row13 col2" >232</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col3" class="data row13 col3" >2304</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col4" class="data row13 col4" >25402</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow13_col5" class="data row13 col5" >0.008304</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row14" class="row_heading level0 row14" >14</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col0" class="data row14 col0" >Switzerland</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col1" class="data row14 col1" >26732</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col2" class="data row14 col2" >1281</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col3" class="data row14 col3" >15900</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col4" class="data row14 col4" >9551</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow14_col5" class="data row14 col5" >0.047920</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row15" class="row_heading level0 row15" >15</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col0" class="data row15 col0" >Portugal</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col1" class="data row15 col1" >18841</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col2" class="data row15 col2" >629</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col3" class="data row15 col3" >493</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col4" class="data row15 col4" >17719</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow15_col5" class="data row15 col5" >0.033385</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row16" class="row_heading level0 row16" >16</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col0" class="data row16 col0" >Austria</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col1" class="data row16 col1" >14476</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col2" class="data row16 col2" >410</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col3" class="data row16 col3" >8986</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col4" class="data row16 col4" >5080</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow16_col5" class="data row16 col5" >0.028323</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row17" class="row_heading level0 row17" >17</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col0" class="data row17 col0" >India</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col1" class="data row17 col1" >13430</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col2" class="data row17 col2" >448</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col3" class="data row17 col3" >1768</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col4" class="data row17 col4" >11214</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow17_col5" class="data row17 col5" >0.033358</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row18" class="row_heading level0 row18" >18</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col0" class="data row18 col0" >Ireland</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col1" class="data row18 col1" >13271</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col2" class="data row18 col2" >486</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col3" class="data row18 col3" >77</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col4" class="data row18 col4" >12708</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow18_col5" class="data row18 col5" >0.036621</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row19" class="row_heading level0 row19" >19</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col0" class="data row19 col0" >Israel</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col1" class="data row19 col1" >12758</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col2" class="data row19 col2" >142</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col3" class="data row19 col3" >2818</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col4" class="data row19 col4" >9798</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow19_col5" class="data row19 col5" >0.011130</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row20" class="row_heading level0 row20" >20</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col0" class="data row20 col0" >Sweden</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col1" class="data row20 col1" >12540</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col2" class="data row20 col2" >1333</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col3" class="data row20 col3" >550</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col4" class="data row20 col4" >10657</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow20_col5" class="data row20 col5" >0.106300</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row21" class="row_heading level0 row21" >21</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col0" class="data row21 col0" >Peru</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col1" class="data row21 col1" >12491</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col2" class="data row21 col2" >274</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col3" class="data row21 col3" >6120</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col4" class="data row21 col4" >6097</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow21_col5" class="data row21 col5" >0.021936</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row22" class="row_heading level0 row22" >22</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col0" class="data row22 col0" >Korea, South</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col1" class="data row22 col1" >10613</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col2" class="data row22 col2" >229</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col3" class="data row22 col3" >7757</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col4" class="data row22 col4" >2627</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow22_col5" class="data row22 col5" >0.021577</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row23" class="row_heading level0 row23" >23</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col0" class="data row23 col0" >Chile</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col1" class="data row23 col1" >8807</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col2" class="data row23 col2" >105</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col3" class="data row23 col3" >3299</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col4" class="data row23 col4" >5403</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow23_col5" class="data row23 col5" >0.011922</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row24" class="row_heading level0 row24" >24</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col0" class="data row24 col0" >Japan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col1" class="data row24 col1" >8626</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col2" class="data row24 col2" >178</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col3" class="data row24 col3" >901</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col4" class="data row24 col4" >7547</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow24_col5" class="data row24 col5" >0.020635</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row25" class="row_heading level0 row25" >25</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col0" class="data row25 col0" >Ecuador</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col1" class="data row25 col1" >8225</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col2" class="data row25 col2" >403</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col3" class="data row25 col3" >838</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col4" class="data row25 col4" >6984</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow25_col5" class="data row25 col5" >0.048997</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row26" class="row_heading level0 row26" >26</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col0" class="data row26 col0" >Poland</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col1" class="data row26 col1" >7918</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col2" class="data row26 col2" >314</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col3" class="data row26 col3" >774</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col4" class="data row26 col4" >6830</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow26_col5" class="data row26 col5" >0.039656</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row27" class="row_heading level0 row27" >27</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col0" class="data row27 col0" >Romania</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col1" class="data row27 col1" >7707</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col2" class="data row27 col2" >392</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col3" class="data row27 col3" >1357</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col4" class="data row27 col4" >5958</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow27_col5" class="data row27 col5" >0.050863</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row28" class="row_heading level0 row28" >28</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col0" class="data row28 col0" >Denmark</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col1" class="data row28 col1" >7074</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col2" class="data row28 col2" >321</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col3" class="data row28 col3" >3203</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col4" class="data row28 col4" >3550</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow28_col5" class="data row28 col5" >0.045377</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row29" class="row_heading level0 row29" >29</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col0" class="data row29 col0" >Pakistan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col1" class="data row29 col1" >6919</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col2" class="data row29 col2" >128</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col3" class="data row29 col3" >1645</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col4" class="data row29 col4" >5146</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow29_col5" class="data row29 col5" >0.018500</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row30" class="row_heading level0 row30" >30</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col0" class="data row30 col0" >Norway</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col1" class="data row30 col1" >6896</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col2" class="data row30 col2" >152</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col3" class="data row30 col3" >32</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col4" class="data row30 col4" >6712</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow30_col5" class="data row30 col5" >0.022042</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row31" class="row_heading level0 row31" >31</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col0" class="data row31 col0" >Australia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col1" class="data row31 col1" >6462</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col2" class="data row31 col2" >63</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col3" class="data row31 col3" >2355</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col4" class="data row31 col4" >4044</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow31_col5" class="data row31 col5" >0.009749</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row32" class="row_heading level0 row32" >32</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col0" class="data row32 col0" >Czechia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col1" class="data row32 col1" >6433</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col2" class="data row32 col2" >169</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col3" class="data row32 col3" >972</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col4" class="data row32 col4" >5292</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow32_col5" class="data row32 col5" >0.026271</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row33" class="row_heading level0 row33" >33</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col0" class="data row33 col0" >Saudi Arabia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col1" class="data row33 col1" >6380</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col2" class="data row33 col2" >83</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col3" class="data row33 col3" >990</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col4" class="data row33 col4" >5307</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow33_col5" class="data row33 col5" >0.013009</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row34" class="row_heading level0 row34" >34</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col0" class="data row34 col0" >Mexico</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col1" class="data row34 col1" >5847</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col2" class="data row34 col2" >449</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col3" class="data row34 col3" >2125</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col4" class="data row34 col4" >3273</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow34_col5" class="data row34 col5" >0.076792</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row35" class="row_heading level0 row35" >35</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col0" class="data row35 col0" >United Arab Emirates</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col1" class="data row35 col1" >5825</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col2" class="data row35 col2" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col3" class="data row35 col3" >1095</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col4" class="data row35 col4" >4695</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow35_col5" class="data row35 col5" >0.006009</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row36" class="row_heading level0 row36" >36</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col0" class="data row36 col0" >Philippines</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col1" class="data row36 col1" >5660</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col2" class="data row36 col2" >362</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col3" class="data row36 col3" >435</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col4" class="data row36 col4" >4863</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow36_col5" class="data row36 col5" >0.063958</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row37" class="row_heading level0 row37" >37</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col0" class="data row37 col0" >Indonesia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col1" class="data row37 col1" >5516</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col2" class="data row37 col2" >496</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col3" class="data row37 col3" >548</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col4" class="data row37 col4" >4472</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow37_col5" class="data row37 col5" >0.089920</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row38" class="row_heading level0 row38" >38</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col0" class="data row38 col0" >Serbia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col1" class="data row38 col1" >5318</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col2" class="data row38 col2" >103</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col3" class="data row38 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col4" class="data row38 col4" >5215</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow38_col5" class="data row38 col5" >0.019368</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row39" class="row_heading level0 row39" >39</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col0" class="data row39 col0" >Malaysia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col1" class="data row39 col1" >5182</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col2" class="data row39 col2" >84</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col3" class="data row39 col3" >2766</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col4" class="data row39 col4" >2332</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow39_col5" class="data row39 col5" >0.016210</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row40" class="row_heading level0 row40" >40</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col0" class="data row40 col0" >Singapore</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col1" class="data row40 col1" >4427</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col2" class="data row40 col2" >10</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col3" class="data row40 col3" >683</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col4" class="data row40 col4" >3734</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow40_col5" class="data row40 col5" >0.002259</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row41" class="row_heading level0 row41" >41</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col0" class="data row41 col0" >Belarus</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col1" class="data row41 col1" >4204</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col2" class="data row41 col2" >40</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col3" class="data row41 col3" >203</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col4" class="data row41 col4" >3961</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow41_col5" class="data row41 col5" >0.009515</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row42" class="row_heading level0 row42" >42</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col0" class="data row42 col0" >Ukraine</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col1" class="data row42 col1" >4161</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col2" class="data row42 col2" >116</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col3" class="data row42 col3" >186</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col4" class="data row42 col4" >3859</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow42_col5" class="data row42 col5" >0.027878</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row43" class="row_heading level0 row43" >43</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col0" class="data row43 col0" >Qatar</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col1" class="data row43 col1" >4103</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col2" class="data row43 col2" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col3" class="data row43 col3" >415</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col4" class="data row43 col4" >3681</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow43_col5" class="data row43 col5" >0.001706</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row44" class="row_heading level0 row44" >44</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col0" class="data row44 col0" >Dominican Republic</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col1" class="data row44 col1" >3755</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col2" class="data row44 col2" >196</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col3" class="data row44 col3" >215</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col4" class="data row44 col4" >3344</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow44_col5" class="data row44 col5" >0.052197</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row45" class="row_heading level0 row45" >45</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col0" class="data row45 col0" >Panama</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col1" class="data row45 col1" >3751</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col2" class="data row45 col2" >103</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col3" class="data row45 col3" >75</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col4" class="data row45 col4" >3573</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow45_col5" class="data row45 col5" >0.027459</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row46" class="row_heading level0 row46" >46</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col0" class="data row46 col0" >Luxembourg</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col1" class="data row46 col1" >3444</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col2" class="data row46 col2" >69</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col3" class="data row46 col3" >552</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col4" class="data row46 col4" >2823</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow46_col5" class="data row46 col5" >0.020035</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row47" class="row_heading level0 row47" >47</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col0" class="data row47 col0" >Finland</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col1" class="data row47 col1" >3369</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col2" class="data row47 col2" >75</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col3" class="data row47 col3" >1700</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col4" class="data row47 col4" >1594</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow47_col5" class="data row47 col5" >0.022262</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row48" class="row_heading level0 row48" >48</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col0" class="data row48 col0" >Colombia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col1" class="data row48 col1" >3233</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col2" class="data row48 col2" >144</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col3" class="data row48 col3" >550</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col4" class="data row48 col4" >2539</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow48_col5" class="data row48 col5" >0.044541</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row49" class="row_heading level0 row49" >49</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col0" class="data row49 col0" >Egypt</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col1" class="data row49 col1" >2673</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col2" class="data row49 col2" >196</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col3" class="data row49 col3" >596</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col4" class="data row49 col4" >1881</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow49_col5" class="data row49 col5" >0.073326</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row50" class="row_heading level0 row50" >50</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col0" class="data row50 col0" >Thailand</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col1" class="data row50 col1" >2672</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col2" class="data row50 col2" >46</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col3" class="data row50 col3" >1593</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col4" class="data row50 col4" >1033</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow50_col5" class="data row50 col5" >0.017216</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row51" class="row_heading level0 row51" >51</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col0" class="data row51 col0" >South Africa</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col1" class="data row51 col1" >2605</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col2" class="data row51 col2" >48</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col3" class="data row51 col3" >903</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col4" class="data row51 col4" >1654</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow51_col5" class="data row51 col5" >0.018426</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row52" class="row_heading level0 row52" >52</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col0" class="data row52 col0" >Argentina</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col1" class="data row52 col1" >2571</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col2" class="data row52 col2" >115</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col3" class="data row52 col3" >631</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col4" class="data row52 col4" >1825</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow52_col5" class="data row52 col5" >0.044730</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row53" class="row_heading level0 row53" >53</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col0" class="data row53 col0" >Morocco</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col1" class="data row53 col1" >2283</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col2" class="data row53 col2" >130</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col3" class="data row53 col3" >249</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col4" class="data row53 col4" >1904</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow53_col5" class="data row53 col5" >0.056943</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row54" class="row_heading level0 row54" >54</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col0" class="data row54 col0" >Algeria</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col1" class="data row54 col1" >2268</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col2" class="data row54 col2" >348</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col3" class="data row54 col3" >783</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col4" class="data row54 col4" >1137</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow54_col5" class="data row54 col5" >0.153439</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row55" class="row_heading level0 row55" >55</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col0" class="data row55 col0" >Greece</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col1" class="data row55 col1" >2207</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col2" class="data row55 col2" >105</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col3" class="data row55 col3" >269</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col4" class="data row55 col4" >1833</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow55_col5" class="data row55 col5" >0.047576</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row56" class="row_heading level0 row56" >56</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col0" class="data row56 col0" >Moldova</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col1" class="data row56 col1" >2154</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col2" class="data row56 col2" >54</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col3" class="data row56 col3" >235</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col4" class="data row56 col4" >1865</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow56_col5" class="data row56 col5" >0.025070</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row57" class="row_heading level0 row57" >57</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col0" class="data row57 col0" >Croatia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col1" class="data row57 col1" >1791</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col2" class="data row57 col2" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col3" class="data row57 col3" >529</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col4" class="data row57 col4" >1227</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow57_col5" class="data row57 col5" >0.019542</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row58" class="row_heading level0 row58" >58</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col0" class="data row58 col0" >Iceland</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col1" class="data row58 col1" >1739</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col2" class="data row58 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col3" class="data row58 col3" >1144</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col4" class="data row58 col4" >587</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow58_col5" class="data row58 col5" >0.004600</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row59" class="row_heading level0 row59" >59</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col0" class="data row59 col0" >Bahrain</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col1" class="data row59 col1" >1700</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col2" class="data row59 col2" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col3" class="data row59 col3" >703</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col4" class="data row59 col4" >990</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow59_col5" class="data row59 col5" >0.004118</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row60" class="row_heading level0 row60" >60</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col0" class="data row60 col0" >Hungary</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col1" class="data row60 col1" >1652</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col2" class="data row60 col2" >142</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col3" class="data row60 col3" >199</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col4" class="data row60 col4" >1311</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow60_col5" class="data row60 col5" >0.085956</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row61" class="row_heading level0 row61" >61</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col0" class="data row61 col0" >Bangladesh</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col1" class="data row61 col1" >1572</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col2" class="data row61 col2" >60</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col3" class="data row61 col3" >49</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col4" class="data row61 col4" >1463</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow61_col5" class="data row61 col5" >0.038168</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row62" class="row_heading level0 row62" >62</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col0" class="data row62 col0" >Kuwait</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col1" class="data row62 col1" >1524</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col2" class="data row62 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col3" class="data row62 col3" >225</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col4" class="data row62 col4" >1296</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow62_col5" class="data row62 col5" >0.001969</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row63" class="row_heading level0 row63" >63</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col0" class="data row63 col0" >Estonia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col1" class="data row63 col1" >1434</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col2" class="data row63 col2" >36</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col3" class="data row63 col3" >133</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col4" class="data row63 col4" >1265</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow63_col5" class="data row63 col5" >0.025105</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row64" class="row_heading level0 row64" >64</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col0" class="data row64 col0" >Iraq</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col1" class="data row64 col1" >1434</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col2" class="data row64 col2" >80</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col3" class="data row64 col3" >856</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col4" class="data row64 col4" >498</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow64_col5" class="data row64 col5" >0.055788</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row65" class="row_heading level0 row65" >65</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col0" class="data row65 col0" >Kazakhstan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col1" class="data row65 col1" >1402</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col2" class="data row65 col2" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col3" class="data row65 col3" >277</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col4" class="data row65 col4" >1108</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow65_col5" class="data row65 col5" >0.012126</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row66" class="row_heading level0 row66" >66</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col0" class="data row66 col0" >New Zealand</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col1" class="data row66 col1" >1401</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col2" class="data row66 col2" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col3" class="data row66 col3" >770</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col4" class="data row66 col4" >622</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow66_col5" class="data row66 col5" >0.006424</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row67" class="row_heading level0 row67" >67</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col0" class="data row67 col0" >Uzbekistan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col1" class="data row67 col1" >1349</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col2" class="data row67 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col3" class="data row67 col3" >129</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col4" class="data row67 col4" >1216</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow67_col5" class="data row67 col5" >0.002965</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row68" class="row_heading level0 row68" >68</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col0" class="data row68 col0" >Azerbaijan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col1" class="data row68 col1" >1283</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col2" class="data row68 col2" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col3" class="data row68 col3" >460</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col4" class="data row68 col4" >808</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow68_col5" class="data row68 col5" >0.011691</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row69" class="row_heading level0 row69" >69</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col0" class="data row69 col0" >Slovenia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col1" class="data row69 col1" >1268</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col2" class="data row69 col2" >61</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col3" class="data row69 col3" >174</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col4" class="data row69 col4" >1033</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow69_col5" class="data row69 col5" >0.048107</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row70" class="row_heading level0 row70" >70</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col0" class="data row70 col0" >Bosnia and Herzegovina</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col1" class="data row70 col1" >1167</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col2" class="data row70 col2" >43</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col3" class="data row70 col3" >277</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col4" class="data row70 col4" >847</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow70_col5" class="data row70 col5" >0.036847</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row71" class="row_heading level0 row71" >71</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col0" class="data row71 col0" >Armenia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col1" class="data row71 col1" >1159</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col2" class="data row71 col2" >18</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col3" class="data row71 col3" >358</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col4" class="data row71 col4" >783</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow71_col5" class="data row71 col5" >0.015531</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row72" class="row_heading level0 row72" >72</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col0" class="data row72 col0" >Lithuania</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col1" class="data row72 col1" >1128</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col2" class="data row72 col2" >32</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col3" class="data row72 col3" >178</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col4" class="data row72 col4" >918</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow72_col5" class="data row72 col5" >0.028369</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row73" class="row_heading level0 row73" >73</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col0" class="data row73 col0" >North Macedonia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col1" class="data row73 col1" >1081</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col2" class="data row73 col2" >46</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col3" class="data row73 col3" >121</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col4" class="data row73 col4" >914</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow73_col5" class="data row73 col5" >0.042553</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row74" class="row_heading level0 row74" >74</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col0" class="data row74 col0" >Oman</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col1" class="data row74 col1" >1019</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col2" class="data row74 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col3" class="data row74 col3" >176</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col4" class="data row74 col4" >839</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow74_col5" class="data row74 col5" >0.003925</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row75" class="row_heading level0 row75" >75</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col0" class="data row75 col0" >Cameroon</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col1" class="data row75 col1" >996</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col2" class="data row75 col2" >22</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col3" class="data row75 col3" >164</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col4" class="data row75 col4" >810</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow75_col5" class="data row75 col5" >0.022088</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row76" class="row_heading level0 row76" >76</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col0" class="data row76 col0" >Slovakia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col1" class="data row76 col1" >977</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col2" class="data row76 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col3" class="data row76 col3" >167</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col4" class="data row76 col4" >802</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow76_col5" class="data row76 col5" >0.008188</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row77" class="row_heading level0 row77" >77</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col0" class="data row77 col0" >Cuba</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col1" class="data row77 col1" >862</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col2" class="data row77 col2" >27</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col3" class="data row77 col3" >171</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col4" class="data row77 col4" >664</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow77_col5" class="data row77 col5" >0.031323</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row78" class="row_heading level0 row78" >78</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col0" class="data row78 col0" >Afghanistan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col1" class="data row78 col1" >840</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col2" class="data row78 col2" >30</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col3" class="data row78 col3" >54</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col4" class="data row78 col4" >756</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow78_col5" class="data row78 col5" >0.035714</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row79" class="row_heading level0 row79" >79</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col0" class="data row79 col0" >Tunisia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col1" class="data row79 col1" >822</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col2" class="data row79 col2" >37</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col3" class="data row79 col3" >43</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col4" class="data row79 col4" >742</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow79_col5" class="data row79 col5" >0.045012</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row80" class="row_heading level0 row80" >80</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col0" class="data row80 col0" >Bulgaria</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col1" class="data row80 col1" >800</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col2" class="data row80 col2" >38</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col3" class="data row80 col3" >122</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col4" class="data row80 col4" >640</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow80_col5" class="data row80 col5" >0.047500</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row81" class="row_heading level0 row81" >81</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col0" class="data row81 col0" >Cyprus</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col1" class="data row81 col1" >735</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col2" class="data row81 col2" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col3" class="data row81 col3" >77</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col4" class="data row81 col4" >646</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow81_col5" class="data row81 col5" >0.016327</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row82" class="row_heading level0 row82" >82</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col0" class="data row82 col0" >Diamond Princess</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col1" class="data row82 col1" >712</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col2" class="data row82 col2" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col3" class="data row82 col3" >644</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col4" class="data row82 col4" >56</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow82_col5" class="data row82 col5" >0.016854</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row83" class="row_heading level0 row83" >83</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col0" class="data row83 col0" >Latvia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col1" class="data row83 col1" >675</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col2" class="data row83 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col3" class="data row83 col3" >57</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col4" class="data row83 col4" >613</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow83_col5" class="data row83 col5" >0.007407</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row84" class="row_heading level0 row84" >84</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col0" class="data row84 col0" >Andorra</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col1" class="data row84 col1" >673</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col2" class="data row84 col2" >33</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col3" class="data row84 col3" >169</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col4" class="data row84 col4" >471</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow84_col5" class="data row84 col5" >0.049034</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row85" class="row_heading level0 row85" >85</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col0" class="data row85 col0" >Lebanon</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col1" class="data row85 col1" >663</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col2" class="data row85 col2" >21</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col3" class="data row85 col3" >86</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col4" class="data row85 col4" >556</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow85_col5" class="data row85 col5" >0.031674</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row86" class="row_heading level0 row86" >86</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col0" class="data row86 col0" >Cote d'Ivoire</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col1" class="data row86 col1" >654</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col2" class="data row86 col2" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col3" class="data row86 col3" >146</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col4" class="data row86 col4" >502</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow86_col5" class="data row86 col5" >0.009174</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row87" class="row_heading level0 row87" >87</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col0" class="data row87 col0" >Costa Rica</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col1" class="data row87 col1" >642</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col2" class="data row87 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col3" class="data row87 col3" >74</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col4" class="data row87 col4" >564</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow87_col5" class="data row87 col5" >0.006231</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row88" class="row_heading level0 row88" >88</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col0" class="data row88 col0" >Ghana</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col1" class="data row88 col1" >641</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col2" class="data row88 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col3" class="data row88 col3" >83</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col4" class="data row88 col4" >550</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow88_col5" class="data row88 col5" >0.012480</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row89" class="row_heading level0 row89" >89</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col0" class="data row89 col0" >Djibouti</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col1" class="data row89 col1" >591</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col2" class="data row89 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col3" class="data row89 col3" >73</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col4" class="data row89 col4" >516</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow89_col5" class="data row89 col5" >0.003384</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row90" class="row_heading level0 row90" >90</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col0" class="data row90 col0" >Niger</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col1" class="data row90 col1" >584</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col2" class="data row90 col2" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col3" class="data row90 col3" >90</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col4" class="data row90 col4" >480</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow90_col5" class="data row90 col5" >0.023973</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row91" class="row_heading level0 row91" >91</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col0" class="data row91 col0" >Burkina Faso</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col1" class="data row91 col1" >546</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col2" class="data row91 col2" >32</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col3" class="data row91 col3" >257</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col4" class="data row91 col4" >257</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow91_col5" class="data row91 col5" >0.058608</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row92" class="row_heading level0 row92" >92</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col0" class="data row92 col0" >Albania</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col1" class="data row92 col1" >518</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col2" class="data row92 col2" >26</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col3" class="data row92 col3" >277</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col4" class="data row92 col4" >215</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow92_col5" class="data row92 col5" >0.050193</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row93" class="row_heading level0 row93" >93</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col0" class="data row93 col0" >Uruguay</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col1" class="data row93 col1" >502</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col2" class="data row93 col2" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col3" class="data row93 col3" >286</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col4" class="data row93 col4" >207</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow93_col5" class="data row93 col5" >0.017928</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row94" class="row_heading level0 row94" >94</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col0" class="data row94 col0" >Kyrgyzstan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col1" class="data row94 col1" >466</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col2" class="data row94 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col3" class="data row94 col3" >91</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col4" class="data row94 col4" >370</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow94_col5" class="data row94 col5" >0.010730</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row95" class="row_heading level0 row95" >95</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col0" class="data row95 col0" >Kosovo</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col1" class="data row95 col1" >449</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col2" class="data row95 col2" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col3" class="data row95 col3" >79</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col4" class="data row95 col4" >359</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow95_col5" class="data row95 col5" >0.024499</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row96" class="row_heading level0 row96" >96</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col0" class="data row96 col0" >Nigeria</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col1" class="data row96 col1" >442</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col2" class="data row96 col2" >13</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col3" class="data row96 col3" >152</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col4" class="data row96 col4" >277</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow96_col5" class="data row96 col5" >0.029412</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row97" class="row_heading level0 row97" >97</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col0" class="data row97 col0" >Bolivia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col1" class="data row97 col1" >441</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col2" class="data row97 col2" >29</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col3" class="data row97 col3" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col4" class="data row97 col4" >398</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow97_col5" class="data row97 col5" >0.065760</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row98" class="row_heading level0 row98" >98</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col0" class="data row98 col0" >Guinea</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col1" class="data row98 col1" >438</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col2" class="data row98 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col3" class="data row98 col3" >49</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col4" class="data row98 col4" >388</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow98_col5" class="data row98 col5" >0.002283</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row99" class="row_heading level0 row99" >99</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col0" class="data row99 col0" >Honduras</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col1" class="data row99 col1" >426</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col2" class="data row99 col2" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col3" class="data row99 col3" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col4" class="data row99 col4" >382</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow99_col5" class="data row99 col5" >0.082160</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row100" class="row_heading level0 row100" >100</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col0" class="data row100 col0" >San Marino</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col1" class="data row100 col1" >426</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col2" class="data row100 col2" >38</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col3" class="data row100 col3" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col4" class="data row100 col4" >333</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow100_col5" class="data row100 col5" >0.089202</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row101" class="row_heading level0 row101" >101</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col0" class="data row101 col0" >Malta</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col1" class="data row101 col1" >412</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col2" class="data row101 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col3" class="data row101 col3" >82</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col4" class="data row101 col4" >327</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow101_col5" class="data row101 col5" >0.007282</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row102" class="row_heading level0 row102" >102</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col0" class="data row102 col0" >Jordan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col1" class="data row102 col1" >402</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col2" class="data row102 col2" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col3" class="data row102 col3" >259</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col4" class="data row102 col4" >136</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow102_col5" class="data row102 col5" >0.017413</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row103" class="row_heading level0 row103" >103</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col0" class="data row103 col0" >Taiwan*</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col1" class="data row103 col1" >395</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col2" class="data row103 col2" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col3" class="data row103 col3" >155</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col4" class="data row103 col4" >234</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow103_col5" class="data row103 col5" >0.015190</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row104" class="row_heading level0 row104" >104</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col0" class="data row104 col0" >West Bank and Gaza</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col1" class="data row104 col1" >374</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col2" class="data row104 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col3" class="data row104 col3" >63</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col4" class="data row104 col4" >309</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow104_col5" class="data row104 col5" >0.005348</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row105" class="row_heading level0 row105" >105</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col0" class="data row105 col0" >Georgia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col1" class="data row105 col1" >348</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col2" class="data row105 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col3" class="data row105 col3" >76</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col4" class="data row105 col4" >269</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow105_col5" class="data row105 col5" >0.008621</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row106" class="row_heading level0 row106" >106</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col0" class="data row106 col0" >Senegal</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col1" class="data row106 col1" >335</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col2" class="data row106 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col3" class="data row106 col3" >194</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col4" class="data row106 col4" >139</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow106_col5" class="data row106 col5" >0.005970</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row107" class="row_heading level0 row107" >107</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col0" class="data row107 col0" >Mauritius</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col1" class="data row107 col1" >324</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col2" class="data row107 col2" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col3" class="data row107 col3" >81</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col4" class="data row107 col4" >234</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow107_col5" class="data row107 col5" >0.027778</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row108" class="row_heading level0 row108" >108</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col0" class="data row108 col0" >Montenegro</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col1" class="data row108 col1" >303</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col2" class="data row108 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col3" class="data row108 col3" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col4" class="data row108 col4" >244</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow108_col5" class="data row108 col5" >0.013201</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row109" class="row_heading level0 row109" >109</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col0" class="data row109 col0" >Vietnam</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col1" class="data row109 col1" >268</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col2" class="data row109 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col3" class="data row109 col3" >177</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col4" class="data row109 col4" >91</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow109_col5" class="data row109 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row110" class="row_heading level0 row110" >110</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col0" class="data row110 col0" >Congo (Kinshasa)</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col1" class="data row110 col1" >267</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col2" class="data row110 col2" >22</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col3" class="data row110 col3" >23</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col4" class="data row110 col4" >222</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow110_col5" class="data row110 col5" >0.082397</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row111" class="row_heading level0 row111" >111</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col0" class="data row111 col0" >Sri Lanka</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col1" class="data row111 col1" >238</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col2" class="data row111 col2" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col3" class="data row111 col3" >68</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col4" class="data row111 col4" >163</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow111_col5" class="data row111 col5" >0.029412</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row112" class="row_heading level0 row112" >112</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col0" class="data row112 col0" >Kenya</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col1" class="data row112 col1" >234</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col2" class="data row112 col2" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col3" class="data row112 col3" >53</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col4" class="data row112 col4" >170</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow112_col5" class="data row112 col5" >0.047009</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row113" class="row_heading level0 row113" >113</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col0" class="data row113 col0" >Venezuela</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col1" class="data row113 col1" >204</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col2" class="data row113 col2" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col3" class="data row113 col3" >111</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col4" class="data row113 col4" >84</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow113_col5" class="data row113 col5" >0.044118</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row114" class="row_heading level0 row114" >114</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col0" class="data row114 col0" >Guatemala</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col1" class="data row114 col1" >196</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col2" class="data row114 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col3" class="data row114 col3" >19</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col4" class="data row114 col4" >172</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow114_col5" class="data row114 col5" >0.025510</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row115" class="row_heading level0 row115" >115</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col0" class="data row115 col0" >Paraguay</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col1" class="data row115 col1" >174</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col2" class="data row115 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col3" class="data row115 col3" >30</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col4" class="data row115 col4" >136</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow115_col5" class="data row115 col5" >0.045977</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row116" class="row_heading level0 row116" >116</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col0" class="data row116 col0" >Mali</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col1" class="data row116 col1" >171</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col2" class="data row116 col2" >13</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col3" class="data row116 col3" >34</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col4" class="data row116 col4" >124</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow116_col5" class="data row116 col5" >0.076023</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row117" class="row_heading level0 row117" >117</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col0" class="data row117 col0" >El Salvador</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col1" class="data row117 col1" >164</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col2" class="data row117 col2" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col3" class="data row117 col3" >33</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col4" class="data row117 col4" >125</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow117_col5" class="data row117 col5" >0.036585</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row118" class="row_heading level0 row118" >118</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col0" class="data row118 col0" >Jamaica</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col1" class="data row118 col1" >143</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col2" class="data row118 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col3" class="data row118 col3" >21</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col4" class="data row118 col4" >117</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow118_col5" class="data row118 col5" >0.034965</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row119" class="row_heading level0 row119" >119</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col0" class="data row119 col0" >Rwanda</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col1" class="data row119 col1" >138</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col2" class="data row119 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col3" class="data row119 col3" >60</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col4" class="data row119 col4" >78</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow119_col5" class="data row119 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row120" class="row_heading level0 row120" >120</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col0" class="data row120 col0" >Brunei</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col1" class="data row120 col1" >136</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col2" class="data row120 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col3" class="data row120 col3" >108</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col4" class="data row120 col4" >27</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow120_col5" class="data row120 col5" >0.007353</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row121" class="row_heading level0 row121" >121</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col0" class="data row121 col0" >Cambodia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col1" class="data row121 col1" >122</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col2" class="data row121 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col3" class="data row121 col3" >98</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col4" class="data row121 col4" >24</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow121_col5" class="data row121 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row122" class="row_heading level0 row122" >122</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col0" class="data row122 col0" >Congo (Brazzaville)</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col1" class="data row122 col1" >117</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col2" class="data row122 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col3" class="data row122 col3" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col4" class="data row122 col4" >101</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow122_col5" class="data row122 col5" >0.042735</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row123" class="row_heading level0 row123" >123</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col0" class="data row123 col0" >Trinidad and Tobago</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col1" class="data row123 col1" >114</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col2" class="data row123 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col3" class="data row123 col3" >20</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col4" class="data row123 col4" >86</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow123_col5" class="data row123 col5" >0.070175</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row124" class="row_heading level0 row124" >124</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col0" class="data row124 col0" >Madagascar</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col1" class="data row124 col1" >111</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col2" class="data row124 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col3" class="data row124 col3" >33</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col4" class="data row124 col4" >78</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow124_col5" class="data row124 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row125" class="row_heading level0 row125" >125</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col0" class="data row125 col0" >Tanzania</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col1" class="data row125 col1" >94</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col2" class="data row125 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col3" class="data row125 col3" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col4" class="data row125 col4" >79</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow125_col5" class="data row125 col5" >0.042553</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row126" class="row_heading level0 row126" >126</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col0" class="data row126 col0" >Monaco</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col1" class="data row126 col1" >93</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col2" class="data row126 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col3" class="data row126 col3" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col4" class="data row126 col4" >78</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow126_col5" class="data row126 col5" >0.032258</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row127" class="row_heading level0 row127" >127</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col0" class="data row127 col0" >Ethiopia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col1" class="data row127 col1" >92</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col2" class="data row127 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col3" class="data row127 col3" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col4" class="data row127 col4" >74</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow127_col5" class="data row127 col5" >0.032609</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row128" class="row_heading level0 row128" >128</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col0" class="data row128 col0" >Burma</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col1" class="data row128 col1" >85</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col2" class="data row128 col2" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col3" class="data row128 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col4" class="data row128 col4" >79</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow128_col5" class="data row128 col5" >0.047059</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row129" class="row_heading level0 row129" >129</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col0" class="data row129 col0" >Togo</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col1" class="data row129 col1" >81</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col2" class="data row129 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col3" class="data row129 col3" >45</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col4" class="data row129 col4" >31</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow129_col5" class="data row129 col5" >0.061728</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row130" class="row_heading level0 row130" >130</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col0" class="data row130 col0" >Gabon</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col1" class="data row130 col1" >80</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col2" class="data row130 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col3" class="data row130 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col4" class="data row130 col4" >75</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow130_col5" class="data row130 col5" >0.012500</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row131" class="row_heading level0 row131" >131</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col0" class="data row131 col0" >Somalia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col1" class="data row131 col1" >80</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col2" class="data row131 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col3" class="data row131 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col4" class="data row131 col4" >73</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow131_col5" class="data row131 col5" >0.062500</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row132" class="row_heading level0 row132" >132</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col0" class="data row132 col0" >Liechtenstein</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col1" class="data row132 col1" >79</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col2" class="data row132 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col3" class="data row132 col3" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col4" class="data row132 col4" >23</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow132_col5" class="data row132 col5" >0.012658</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row133" class="row_heading level0 row133" >133</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col0" class="data row133 col0" >Barbados</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col1" class="data row133 col1" >75</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col2" class="data row133 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col3" class="data row133 col3" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col4" class="data row133 col4" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow133_col5" class="data row133 col5" >0.066667</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row134" class="row_heading level0 row134" >134</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col0" class="data row134 col0" >Liberia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col1" class="data row134 col1" >59</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col2" class="data row134 col2" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col3" class="data row134 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col4" class="data row134 col4" >49</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow134_col5" class="data row134 col5" >0.101695</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row135" class="row_heading level0 row135" >135</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col0" class="data row135 col0" >Cabo Verde</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col1" class="data row135 col1" >56</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col2" class="data row135 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col3" class="data row135 col3" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col4" class="data row135 col4" >54</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow135_col5" class="data row135 col5" >0.017857</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row136" class="row_heading level0 row136" >136</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col0" class="data row136 col0" >Guyana</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col1" class="data row136 col1" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col2" class="data row136 col2" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col3" class="data row136 col3" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col4" class="data row136 col4" >41</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow136_col5" class="data row136 col5" >0.109091</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row137" class="row_heading level0 row137" >137</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col0" class="data row137 col0" >Uganda</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col1" class="data row137 col1" >55</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col2" class="data row137 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col3" class="data row137 col3" >20</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col4" class="data row137 col4" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow137_col5" class="data row137 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row138" class="row_heading level0 row138" >138</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col0" class="data row138 col0" >Bahamas</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col1" class="data row138 col1" >53</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col2" class="data row138 col2" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col3" class="data row138 col3" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col4" class="data row138 col4" >39</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow138_col5" class="data row138 col5" >0.150943</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row139" class="row_heading level0 row139" >139</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col0" class="data row139 col0" >Equatorial Guinea</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col1" class="data row139 col1" >51</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col2" class="data row139 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col3" class="data row139 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col4" class="data row139 col4" >47</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow139_col5" class="data row139 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row140" class="row_heading level0 row140" >140</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col0" class="data row140 col0" >Libya</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col1" class="data row140 col1" >49</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col2" class="data row140 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col3" class="data row140 col3" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col4" class="data row140 col4" >37</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow140_col5" class="data row140 col5" >0.020408</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row141" class="row_heading level0 row141" >141</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col0" class="data row141 col0" >Zambia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col1" class="data row141 col1" >48</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col2" class="data row141 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col3" class="data row141 col3" >30</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col4" class="data row141 col4" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow141_col5" class="data row141 col5" >0.041667</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row142" class="row_heading level0 row142" >142</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col0" class="data row142 col0" >Guinea-Bissau</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col1" class="data row142 col1" >43</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col2" class="data row142 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col3" class="data row142 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col4" class="data row142 col4" >43</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow142_col5" class="data row142 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row143" class="row_heading level0 row143" >143</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col0" class="data row143 col0" >Haiti</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col1" class="data row143 col1" >41</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col2" class="data row143 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col3" class="data row143 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col4" class="data row143 col4" >38</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow143_col5" class="data row143 col5" >0.073171</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row144" class="row_heading level0 row144" >144</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col0" class="data row144 col0" >Benin</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col1" class="data row144 col1" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col2" class="data row144 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col3" class="data row144 col3" >18</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col4" class="data row144 col4" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow144_col5" class="data row144 col5" >0.028571</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row145" class="row_heading level0 row145" >145</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col0" class="data row145 col0" >Eritrea</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col1" class="data row145 col1" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col2" class="data row145 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col3" class="data row145 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col4" class="data row145 col4" >35</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow145_col5" class="data row145 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row146" class="row_heading level0 row146" >146</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col0" class="data row146 col0" >Syria</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col1" class="data row146 col1" >33</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col2" class="data row146 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col3" class="data row146 col3" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col4" class="data row146 col4" >26</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow146_col5" class="data row146 col5" >0.060606</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row147" class="row_heading level0 row147" >147</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col0" class="data row147 col0" >Sudan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col1" class="data row147 col1" >32</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col2" class="data row147 col2" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col3" class="data row147 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col4" class="data row147 col4" >23</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow147_col5" class="data row147 col5" >0.156250</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row148" class="row_heading level0 row148" >148</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col0" class="data row148 col0" >Mongolia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col1" class="data row148 col1" >31</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col2" class="data row148 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col3" class="data row148 col3" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col4" class="data row148 col4" >26</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow148_col5" class="data row148 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row149" class="row_heading level0 row149" >149</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col0" class="data row149 col0" >Mozambique</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col1" class="data row149 col1" >31</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col2" class="data row149 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col3" class="data row149 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col4" class="data row149 col4" >29</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow149_col5" class="data row149 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row150" class="row_heading level0 row150" >150</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col0" class="data row150 col0" >Chad</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col1" class="data row150 col1" >27</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col2" class="data row150 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col3" class="data row150 col3" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col4" class="data row150 col4" >22</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow150_col5" class="data row150 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row151" class="row_heading level0 row151" >151</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col0" class="data row151 col0" >Maldives</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col1" class="data row151 col1" >25</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col2" class="data row151 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col3" class="data row151 col3" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col4" class="data row151 col4" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow151_col5" class="data row151 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row152" class="row_heading level0 row152" >152</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col0" class="data row152 col0" >Antigua and Barbuda</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col1" class="data row152 col1" >23</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col2" class="data row152 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col3" class="data row152 col3" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col4" class="data row152 col4" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow152_col5" class="data row152 col5" >0.130435</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row153" class="row_heading level0 row153" >153</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col0" class="data row153 col0" >Zimbabwe</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col1" class="data row153 col1" >23</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col2" class="data row153 col2" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col3" class="data row153 col3" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col4" class="data row153 col4" >19</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow153_col5" class="data row153 col5" >0.130435</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row154" class="row_heading level0 row154" >154</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col0" class="data row154 col0" >Angola</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col1" class="data row154 col1" >19</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col2" class="data row154 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col3" class="data row154 col3" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col4" class="data row154 col4" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow154_col5" class="data row154 col5" >0.105263</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row155" class="row_heading level0 row155" >155</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col0" class="data row155 col0" >Laos</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col1" class="data row155 col1" >19</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col2" class="data row155 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col3" class="data row155 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col4" class="data row155 col4" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow155_col5" class="data row155 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row156" class="row_heading level0 row156" >156</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col0" class="data row156 col0" >Belize</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col1" class="data row156 col1" >18</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col2" class="data row156 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col3" class="data row156 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col4" class="data row156 col4" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow156_col5" class="data row156 col5" >0.111111</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row157" class="row_heading level0 row157" >157</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col0" class="data row157 col0" >Timor-Leste</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col1" class="data row157 col1" >18</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col2" class="data row157 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col3" class="data row157 col3" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col4" class="data row157 col4" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow157_col5" class="data row157 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row158" class="row_heading level0 row158" >158</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col0" class="data row158 col0" >Fiji</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col1" class="data row158 col1" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col2" class="data row158 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col3" class="data row158 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col4" class="data row158 col4" >17</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow158_col5" class="data row158 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row159" class="row_heading level0 row159" >159</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col0" class="data row159 col0" >Dominica</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col1" class="data row159 col1" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col2" class="data row159 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col3" class="data row159 col3" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col4" class="data row159 col4" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow159_col5" class="data row159 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row160" class="row_heading level0 row160" >160</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col0" class="data row160 col0" >Eswatini</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col1" class="data row160 col1" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col2" class="data row160 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col3" class="data row160 col3" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col4" class="data row160 col4" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow160_col5" class="data row160 col5" >0.062500</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row161" class="row_heading level0 row161" >161</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col0" class="data row161 col0" >Malawi</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col1" class="data row161 col1" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col2" class="data row161 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col3" class="data row161 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col4" class="data row161 col4" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow161_col5" class="data row161 col5" >0.125000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row162" class="row_heading level0 row162" >162</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col0" class="data row162 col0" >Namibia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col1" class="data row162 col1" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col2" class="data row162 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col3" class="data row162 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col4" class="data row162 col4" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow162_col5" class="data row162 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row163" class="row_heading level0 row163" >163</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col0" class="data row163 col0" >Nepal</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col1" class="data row163 col1" >16</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col2" class="data row163 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col3" class="data row163 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col4" class="data row163 col4" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow163_col5" class="data row163 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row164" class="row_heading level0 row164" >164</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col0" class="data row164 col0" >Botswana</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col1" class="data row164 col1" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col2" class="data row164 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col3" class="data row164 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col4" class="data row164 col4" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow164_col5" class="data row164 col5" >0.066667</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row165" class="row_heading level0 row165" >165</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col0" class="data row165 col0" >Saint Lucia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col1" class="data row165 col1" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col2" class="data row165 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col3" class="data row165 col3" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col4" class="data row165 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow165_col5" class="data row165 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row166" class="row_heading level0 row166" >166</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col0" class="data row166 col0" >Sierra Leone</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col1" class="data row166 col1" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col2" class="data row166 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col3" class="data row166 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col4" class="data row166 col4" >15</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow166_col5" class="data row166 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row167" class="row_heading level0 row167" >167</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col0" class="data row167 col0" >Grenada</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col1" class="data row167 col1" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col2" class="data row167 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col3" class="data row167 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col4" class="data row167 col4" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow167_col5" class="data row167 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row168" class="row_heading level0 row168" >168</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col0" class="data row168 col0" >Saint Kitts and Nevis</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col1" class="data row168 col1" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col2" class="data row168 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col3" class="data row168 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col4" class="data row168 col4" >14</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow168_col5" class="data row168 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row169" class="row_heading level0 row169" >169</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col0" class="data row169 col0" >Central African Republic</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col1" class="data row169 col1" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col2" class="data row169 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col3" class="data row169 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col4" class="data row169 col4" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow169_col5" class="data row169 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row170" class="row_heading level0 row170" >170</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col0" class="data row170 col0" >Saint Vincent and the Grenadines</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col1" class="data row170 col1" >12</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col2" class="data row170 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col3" class="data row170 col3" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col4" class="data row170 col4" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow170_col5" class="data row170 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row171" class="row_heading level0 row171" >171</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col0" class="data row171 col0" >Seychelles</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col1" class="data row171 col1" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col2" class="data row171 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col3" class="data row171 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col4" class="data row171 col4" >11</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow171_col5" class="data row171 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row172" class="row_heading level0 row172" >172</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col0" class="data row172 col0" >Suriname</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col1" class="data row172 col1" >10</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col2" class="data row172 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col3" class="data row172 col3" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col4" class="data row172 col4" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow172_col5" class="data row172 col5" >0.100000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row173" class="row_heading level0 row173" >173</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col0" class="data row173 col0" >Gambia</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col1" class="data row173 col1" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col2" class="data row173 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col3" class="data row173 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col4" class="data row173 col4" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow173_col5" class="data row173 col5" >0.111111</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row174" class="row_heading level0 row174" >174</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col0" class="data row174 col0" >MS Zaandam</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col1" class="data row174 col1" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col2" class="data row174 col2" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col3" class="data row174 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col4" class="data row174 col4" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow174_col5" class="data row174 col5" >0.222222</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row175" class="row_heading level0 row175" >175</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col0" class="data row175 col0" >Nicaragua</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col1" class="data row175 col1" >9</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col2" class="data row175 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col3" class="data row175 col3" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col4" class="data row175 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow175_col5" class="data row175 col5" >0.111111</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row176" class="row_heading level0 row176" >176</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col0" class="data row176 col0" >Holy See</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col1" class="data row176 col1" >8</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col2" class="data row176 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col3" class="data row176 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col4" class="data row176 col4" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow176_col5" class="data row176 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row177" class="row_heading level0 row177" >177</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col0" class="data row177 col0" >Mauritania</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col1" class="data row177 col1" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col2" class="data row177 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col3" class="data row177 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col4" class="data row177 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow177_col5" class="data row177 col5" >0.142857</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row178" class="row_heading level0 row178" >178</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col0" class="data row178 col0" >Papua New Guinea</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col1" class="data row178 col1" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col2" class="data row178 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col3" class="data row178 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col4" class="data row178 col4" >7</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow178_col5" class="data row178 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row179" class="row_heading level0 row179" >179</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col0" class="data row179 col0" >Western Sahara</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col1" class="data row179 col1" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col2" class="data row179 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col3" class="data row179 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col4" class="data row179 col4" >6</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow179_col5" class="data row179 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row180" class="row_heading level0 row180" >180</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col0" class="data row180 col0" >Bhutan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col1" class="data row180 col1" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col2" class="data row180 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col3" class="data row180 col3" >2</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col4" class="data row180 col4" >3</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow180_col5" class="data row180 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row181" class="row_heading level0 row181" >181</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col0" class="data row181 col0" >Burundi</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col1" class="data row181 col1" >5</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col2" class="data row181 col2" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col3" class="data row181 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col4" class="data row181 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow181_col5" class="data row181 col5" >0.200000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row182" class="row_heading level0 row182" >182</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col0" class="data row182 col0" >Sao Tome and Principe</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col1" class="data row182 col1" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col2" class="data row182 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col3" class="data row182 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col4" class="data row182 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow182_col5" class="data row182 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row183" class="row_heading level0 row183" >183</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col0" class="data row183 col0" >South Sudan</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col1" class="data row183 col1" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col2" class="data row183 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col3" class="data row183 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col4" class="data row183 col4" >4</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow183_col5" class="data row183 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9dlevel0_row184" class="row_heading level0 row184" >184</th>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col0" class="data row184 col0" >Yemen</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col1" class="data row184 col1" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col2" class="data row184 col2" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col3" class="data row184 col3" >0</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col4" class="data row184 col4" >1</td>
                        <td id="T_549fc6e6_8128_11ea_a64b_ec5c68c44a9drow184_col5" class="data row184 col5" >0.000000</td>
            </tr>
    </tbody></table>



## COVID-19 By Provinces/State/City


```python
unique_provinces =  list(latest_data['Province_State'].unique())

province_confirmed_cases = []
province_country = [] 
province_death_cases = [] 
province_recovery_cases = []
province_mortality_rate = [] 

no_cases = [] 
for i in unique_provinces:
    cases = latest_data[latest_data['Province_State']==i]['Confirmed'].sum()
    if cases > 0:
        province_confirmed_cases.append(cases)
    else:
        no_cases.append(i)
 
# remove areas with no confirmed cases
for i in no_cases:
    unique_provinces.remove(i)
    
unique_provinces = [k for k, v in sorted(zip(unique_provinces, province_confirmed_cases), key=operator.itemgetter(1), reverse=True)]
for i in range(len(unique_provinces)):
    province_confirmed_cases[i] = latest_data[latest_data['Province_State']==unique_provinces[i]]['Confirmed'].sum()
    province_country.append(latest_data[latest_data['Province_State']==unique_provinces[i]]['Country_Region'].unique()[0])
    province_death_cases.append(latest_data[latest_data['Province_State']==unique_provinces[i]]['Deaths'].sum())
    province_recovery_cases.append(latest_data[latest_data['Province_State']==unique_provinces[i]]['Recovered'].sum())
    province_mortality_rate.append(province_death_cases[i]/province_confirmed_cases[i])
```


```python
# number of cases per province/state/city
province_df = pd.DataFrame({'Province/State Name': unique_provinces, 'Country': province_country, 'Number of Confirmed Cases': province_confirmed_cases,
                          'Number of Deaths': province_death_cases, 'Number of Recoveries' : province_recovery_cases,
                          'Mortality Rate': province_mortality_rate})
# number of cases per country/region

province_df.style.background_gradient(cmap='Greens')
```




<style  type="text/css" >
    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col2 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col3 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col5 {
            background-color:  #91d28e;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col2 {
            background-color:  #acdea6;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col3 {
            background-color:  #cbeac4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col5 {
            background-color:  #bae3b3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col2 {
            background-color:  #b7e2b1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col3 {
            background-color:  #cfecc9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col4 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col5 {
            background-color:  #b7e2b1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col2 {
            background-color:  #e1f3dc;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col3 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col5 {
            background-color:  #cfecc9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col2 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col3 {
            background-color:  #e3f4de;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col5 {
            background-color:  #8ace88;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col2 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col3 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col5 {
            background-color:  #d6efd0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col2 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col3 {
            background-color:  #eef8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col5 {
            background-color:  #ceecc8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col2 {
            background-color:  #e7f6e2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col3 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col5 {
            background-color:  #c3e7bc;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col2 {
            background-color:  #e8f6e4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col3 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col5 {
            background-color:  #d8f0d2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col2 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col3 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col5 {
            background-color:  #b0dfaa;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col2 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col3 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col5 {
            background-color:  #def2d9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col2 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col3 {
            background-color:  #eef8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col5 {
            background-color:  #9cd797;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col2 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col3 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col5 {
            background-color:  #c7e9c0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col2 {
            background-color:  #edf8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col3 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col5 {
            background-color:  #cbeac4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col2 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col3 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col5 {
            background-color:  #afdfa8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col2 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col3 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col5 {
            background-color:  #d7efd1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col2 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col3 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col5 {
            background-color:  #b4e1ad;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col2 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col3 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col5 {
            background-color:  #b2e0ac;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col2 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col3 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col5 {
            background-color:  #b6e2af;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col2 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col3 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col5 {
            background-color:  #c1e6ba;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col2 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col5 {
            background-color:  #d6efd0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col2 {
            background-color:  #f3faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col5 {
            background-color:  #e3f4de;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col2 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col5 {
            background-color:  #dbf1d5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col2 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col5 {
            background-color:  #d2edcc;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col5 {
            background-color:  #d5efcf;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col5 {
            background-color:  #cdecc7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col5 {
            background-color:  #d9f0d3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col3 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col5 {
            background-color:  #b1e0ab;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col5 {
            background-color:  #cdecc7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col2 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col5 {
            background-color:  #def2d9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col5 {
            background-color:  #c1e6ba;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col5 {
            background-color:  #f0f9ec;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col2 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col5 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col5 {
            background-color:  #aedea7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col5 {
            background-color:  #a8dca2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col5 {
            background-color:  #cfecc9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col5 {
            background-color:  #d9f0d3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col5 {
            background-color:  #dbf1d5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col5 {
            background-color:  #dff3da;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col5 {
            background-color:  #b7e2b1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col5 {
            background-color:  #cbebc5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col5 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col5 {
            background-color:  #b4e1ad;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col5 {
            background-color:  #dcf2d7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col4 {
            background-color:  #f4fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col3 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col5 {
            background-color:  #b7e2b1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col5 {
            background-color:  #def2d9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col5 {
            background-color:  #edf8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col5 {
            background-color:  #e8f6e3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col5 {
            background-color:  #d9f0d3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col5 {
            background-color:  #abdda5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col4 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col5 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col5 {
            background-color:  #f4fbf1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col5 {
            background-color:  #f2faef;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col5 {
            background-color:  #e2f4dd;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col2 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col5 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col5 {
            background-color:  #e9f7e5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col5 {
            background-color:  #d0edca;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col4 {
            background-color:  #f5fbf3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col5 {
            background-color:  #c1e6ba;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col5 {
            background-color:  #e8f6e4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col5 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col5 {
            background-color:  #eef8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col5 {
            background-color:  #edf8e9;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col5 {
            background-color:  #e8f6e3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col5 {
            background-color:  #c3e7bc;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col5 {
            background-color:  #eff9ec;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col5 {
            background-color:  #e8f6e3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col5 {
            background-color:  #e0f3db;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col5 {
            background-color:  #f5fbf2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col5 {
            background-color:  #e7f6e2;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col5 {
            background-color:  #d6efd0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col5 {
            background-color:  #f1faee;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col4 {
            background-color:  #f6fcf4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col5 {
            background-color:  #f0f9ed;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col5 {
            background-color:  #e5f5e0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col5 {
            background-color:  #ecf8e8;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col5 {
            background-color:  #f2faf0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col5 {
            background-color:  #e8f6e4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col5 {
            background-color:  #edf8ea;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col5 {
            background-color:  #cdecc7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col5 {
            background-color:  #cdecc7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col5 {
            background-color:  #b1e0ab;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col5 {
            background-color:  #a9dca3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col5 {
            background-color:  #ebf7e7;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col5 {
            background-color:  #eaf7e6;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col5 {
            background-color:  #cbeac4;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col5 {
            background-color:  #d7efd1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col5 {
            background-color:  #eff9eb;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col5 {
            background-color:  #e3f4de;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col5 {
            background-color:  #9bd696;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col5 {
            background-color:  #c7e9c0;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col5 {
            background-color:  #e8f6e3;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col5 {
            background-color:  #00441b;
            color:  #f1f1f1;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col5 {
            background-color:  #e5f5e1;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col5 {
            background-color:  #a5db9f;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col5 {
            background-color:  #86cc85;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col5 {
            background-color:  #004c1e;
            color:  #f1f1f1;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col5 {
            background-color:  #55b567;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col2 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col3 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col4 {
            background-color:  #f7fcf5;
            color:  #000000;
        }    #T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col5 {
            background-color:  #f7fcf5;
            color:  #000000;
        }</style><table id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9d" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Province/State Name</th>        <th class="col_heading level0 col1" >Country</th>        <th class="col_heading level0 col2" >Number of Confirmed Cases</th>        <th class="col_heading level0 col3" >Number of Deaths</th>        <th class="col_heading level0 col4" >Number of Recoveries</th>        <th class="col_heading level0 col5" >Mortality Rate</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row0" class="row_heading level0 row0" >0</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col0" class="data row0 col0" >New York</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col1" class="data row0 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col2" class="data row0 col2" >223691</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col3" class="data row0 col3" >14832</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col4" class="data row0 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow0_col5" class="data row0 col5" >0.066306</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col0" class="data row1 col0" >New Jersey</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col1" class="data row1 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col2" class="data row1 col2" >75317</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col3" class="data row1 col3" >3518</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col4" class="data row1 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow1_col5" class="data row1 col5" >0.046709</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col0" class="data row2 col0" >Hubei</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col1" class="data row2 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col2" class="data row2 col2" >67803</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col3" class="data row2 col3" >3222</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col4" class="data row2 col4" >64435</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow2_col5" class="data row2 col5" >0.047520</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row3" class="row_heading level0 row3" >3</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col0" class="data row3 col0" >Massachusetts</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col1" class="data row3 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col2" class="data row3 col2" >32181</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col3" class="data row3 col3" >1108</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col4" class="data row3 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow3_col5" class="data row3 col5" >0.034430</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row4" class="row_heading level0 row4" >4</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col0" class="data row4 col0" >Michigan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col1" class="data row4 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col2" class="data row4 col2" >28809</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col3" class="data row4 col3" >1996</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col4" class="data row4 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow4_col5" class="data row4 col5" >0.069284</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row5" class="row_heading level0 row5" >5</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col0" class="data row5 col0" >Pennsylvania</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col1" class="data row5 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col2" class="data row5 col2" >28258</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col3" class="data row5 col3" >841</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col4" class="data row5 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow5_col5" class="data row5 col5" >0.029761</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row6" class="row_heading level0 row6" >6</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col0" class="data row6 col0" >California</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col1" class="data row6 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col2" class="data row6 col2" >27677</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col3" class="data row6 col3" >956</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col4" class="data row6 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow6_col5" class="data row6 col5" >0.034541</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row7" class="row_heading level0 row7" >7</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col0" class="data row7 col0" >Illinois</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col1" class="data row7 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col2" class="data row7 col2" >25734</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col3" class="data row7 col3" >1072</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col4" class="data row7 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow7_col5" class="data row7 col5" >0.041657</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row8" class="row_heading level0 row8" >8</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col0" class="data row8 col0" >Florida</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col1" class="data row8 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col2" class="data row8 col2" >23343</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col3" class="data row8 col3" >668</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col4" class="data row8 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow8_col5" class="data row8 col5" >0.028617</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row9" class="row_heading level0 row9" >9</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col0" class="data row9 col0" >Louisiana</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col1" class="data row9 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col2" class="data row9 col2" >22532</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col3" class="data row9 col3" >1156</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col4" class="data row9 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow9_col5" class="data row9 col5" >0.051305</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row10" class="row_heading level0 row10" >10</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col0" class="data row10 col0" >Texas</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col1" class="data row10 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col2" class="data row10 col2" >16876</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col3" class="data row10 col3" >414</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col4" class="data row10 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow10_col5" class="data row10 col5" >0.024532</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row11" class="row_heading level0 row11" >11</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col0" class="data row11 col0" >Connecticut</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col1" class="data row11 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col2" class="data row11 col2" >15884</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col3" class="data row11 col3" >971</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col4" class="data row11 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow11_col5" class="data row11 col5" >0.061131</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row12" class="row_heading level0 row12" >12</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col0" class="data row12 col0" >Quebec</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col1" class="data row12 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col2" class="data row12 col2" >15857</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col3" class="data row12 col3" >630</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col4" class="data row12 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow12_col5" class="data row12 col5" >0.039730</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row13" class="row_heading level0 row13" >13</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col0" class="data row13 col0" >Georgia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col1" class="data row13 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col2" class="data row13 col2" >15669</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col3" class="data row13 col3" >587</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col4" class="data row13 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow13_col5" class="data row13 col5" >0.037463</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row14" class="row_heading level0 row14" >14</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col0" class="data row14 col0" >Washington</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col1" class="data row14 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col2" class="data row14 col2" >11057</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col3" class="data row14 col3" >579</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col4" class="data row14 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow14_col5" class="data row14 col5" >0.052365</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row15" class="row_heading level0 row15" >15</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col0" class="data row15 col0" >Maryland</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col1" class="data row15 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col2" class="data row15 col2" >10784</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col3" class="data row15 col3" >319</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col4" class="data row15 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow15_col5" class="data row15 col5" >0.029581</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row16" class="row_heading level0 row16" >16</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col0" class="data row16 col0" >Ontario</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col1" class="data row16 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col2" class="data row16 col2" >9840</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col3" class="data row16 col3" >490</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col4" class="data row16 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow16_col5" class="data row16 col5" >0.049797</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row17" class="row_heading level0 row17" >17</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col0" class="data row17 col0" >Indiana</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col1" class="data row17 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col2" class="data row17 col2" >9542</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col3" class="data row17 col3" >477</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col4" class="data row17 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow17_col5" class="data row17 col5" >0.049990</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row18" class="row_heading level0 row18" >18</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col0" class="data row18 col0" >Ohio</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col1" class="data row18 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col2" class="data row18 col2" >8414</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col3" class="data row18 col3" >407</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col4" class="data row18 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow18_col5" class="data row18 col5" >0.048372</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row19" class="row_heading level0 row19" >19</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col0" class="data row19 col0" >Colorado</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col1" class="data row19 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col2" class="data row19 col2" >8286</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col3" class="data row19 col3" >355</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col4" class="data row19 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow19_col5" class="data row19 col5" >0.042843</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row20" class="row_heading level0 row20" >20</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col0" class="data row20 col0" >Virginia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col1" class="data row20 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col2" class="data row20 col2" >6889</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col3" class="data row20 col3" >208</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col4" class="data row20 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow20_col5" class="data row20 col5" >0.030193</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row21" class="row_heading level0 row21" >21</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col0" class="data row21 col0" >Tennessee</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col1" class="data row21 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col2" class="data row21 col2" >6375</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col3" class="data row21 col3" >136</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col4" class="data row21 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow21_col5" class="data row21 col5" >0.021333</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row22" class="row_heading level0 row22" >22</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col0" class="data row22 col0" >North Carolina</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col1" class="data row22 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col2" class="data row22 col2" >5639</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col3" class="data row22 col3" >150</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col4" class="data row22 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow22_col5" class="data row22 col5" >0.026600</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row23" class="row_heading level0 row23" >23</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col0" class="data row23 col0" >Missouri</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col1" class="data row23 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col2" class="data row23 col2" >5174</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col3" class="data row23 col3" >169</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col4" class="data row23 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow23_col5" class="data row23 col5" >0.032663</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row24" class="row_heading level0 row24" >24</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col0" class="data row24 col0" >Alabama</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col1" class="data row24 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col2" class="data row24 col2" >4345</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col3" class="data row24 col3" >133</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col4" class="data row24 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow24_col5" class="data row24 col5" >0.030610</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row25" class="row_heading level0 row25" >25</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col0" class="data row25 col0" >Arizona</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col1" class="data row25 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col2" class="data row25 col2" >4237</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col3" class="data row25 col3" >150</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col4" class="data row25 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow25_col5" class="data row25 col5" >0.035402</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row26" class="row_heading level0 row26" >26</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col0" class="data row26 col0" >South Carolina</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col1" class="data row26 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col2" class="data row26 col2" >3931</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col3" class="data row26 col3" >111</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col4" class="data row26 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow26_col5" class="data row26 col5" >0.028237</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row27" class="row_heading level0 row27" >27</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col0" class="data row27 col0" >Wisconsin</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col1" class="data row27 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col2" class="data row27 col2" >3875</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col3" class="data row27 col3" >197</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col4" class="data row27 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow27_col5" class="data row27 col5" >0.050839</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row28" class="row_heading level0 row28" >28</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col0" class="data row28 col0" >Mississippi</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col1" class="data row28 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col2" class="data row28 col2" >3624</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col3" class="data row28 col3" >129</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col4" class="data row28 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow28_col5" class="data row28 col5" >0.035596</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row29" class="row_heading level0 row29" >29</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col0" class="data row29 col0" >Rhode Island</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col1" class="data row29 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col2" class="data row29 col2" >3529</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col3" class="data row29 col3" >87</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col4" class="data row29 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow29_col5" class="data row29 col5" >0.024653</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row30" class="row_heading level0 row30" >30</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col0" class="data row30 col0" >Nevada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col1" class="data row30 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col2" class="data row30 col2" >3214</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col3" class="data row30 col3" >137</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col4" class="data row30 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow30_col5" class="data row30 col5" >0.042626</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row31" class="row_heading level0 row31" >31</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col0" class="data row31 col0" >New South Wales</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col1" class="data row31 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col2" class="data row31 col2" >2897</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col3" class="data row31 col3" >25</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col4" class="data row31 col4" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow31_col5" class="data row31 col5" >0.008630</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row32" class="row_heading level0 row32" >32</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col0" class="data row32 col0" >Utah</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col1" class="data row32 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col2" class="data row32 col2" >2683</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col3" class="data row32 col3" >20</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col4" class="data row32 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow32_col5" class="data row32 col5" >0.007454</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row33" class="row_heading level0 row33" >33</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col0" class="data row33 col0" >Kentucky</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col1" class="data row33 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col2" class="data row33 col2" >2435</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col3" class="data row33 col3" >129</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col4" class="data row33 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow33_col5" class="data row33 col5" >0.052977</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row34" class="row_heading level0 row34" >34</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col0" class="data row34 col0" >Oklahoma</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col1" class="data row34 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col2" class="data row34 col2" >2357</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col3" class="data row34 col3" >131</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col4" class="data row34 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow34_col5" class="data row34 col5" >0.055579</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row35" class="row_heading level0 row35" >35</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col0" class="data row35 col0" >District of Columbia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col1" class="data row35 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col2" class="data row35 col2" >2350</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col3" class="data row35 col3" >81</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col4" class="data row35 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow35_col5" class="data row35 col5" >0.034468</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row36" class="row_heading level0 row36" >36</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col0" class="data row36 col0" >Iowa</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col1" class="data row36 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col2" class="data row36 col2" >2141</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col3" class="data row36 col3" >60</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col4" class="data row36 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow36_col5" class="data row36 col5" >0.028024</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row37" class="row_heading level0 row37" >37</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col0" class="data row37 col0" >Delaware</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col1" class="data row37 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col2" class="data row37 col2" >2070</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col3" class="data row37 col3" >55</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col4" class="data row37 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow37_col5" class="data row37 col5" >0.026570</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row38" class="row_heading level0 row38" >38</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col0" class="data row38 col0" >Alberta</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col1" class="data row38 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col2" class="data row38 col2" >1996</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col3" class="data row38 col3" >48</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col4" class="data row38 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow38_col5" class="data row38 col5" >0.024048</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row39" class="row_heading level0 row39" >39</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col0" class="data row39 col0" >Minnesota</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col1" class="data row39 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col2" class="data row39 col2" >1809</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col3" class="data row39 col3" >87</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col4" class="data row39 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow39_col5" class="data row39 col5" >0.048093</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row40" class="row_heading level0 row40" >40</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col0" class="data row40 col0" >Oregon</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col1" class="data row40 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col2" class="data row40 col2" >1736</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col3" class="data row40 col3" >64</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col4" class="data row40 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow40_col5" class="data row40 col5" >0.036866</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row41" class="row_heading level0 row41" >41</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col0" class="data row41 col0" >Arkansas</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col1" class="data row41 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col2" class="data row41 col2" >1620</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col3" class="data row41 col3" >37</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col4" class="data row41 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow41_col5" class="data row41 col5" >0.022840</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row42" class="row_heading level0 row42" >42</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col0" class="data row42 col0" >Kansas</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col1" class="data row42 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col2" class="data row42 col2" >1615</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col3" class="data row42 col3" >80</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col4" class="data row42 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow42_col5" class="data row42 col5" >0.049536</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row43" class="row_heading level0 row43" >43</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col0" class="data row43 col0" >Idaho</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col1" class="data row43 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col2" class="data row43 col2" >1587</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col3" class="data row43 col3" >41</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col4" class="data row43 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow43_col5" class="data row43 col5" >0.025835</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row44" class="row_heading level0 row44" >44</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col0" class="data row44 col0" >Guangdong</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col1" class="data row44 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col2" class="data row44 col2" >1571</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col3" class="data row44 col3" >8</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col4" class="data row44 col4" >1471</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow44_col5" class="data row44 col5" >0.005092</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row45" class="row_heading level0 row45" >45</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col0" class="data row45 col0" >British Columbia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col1" class="data row45 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col2" class="data row45 col2" >1561</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col3" class="data row45 col3" >75</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col4" class="data row45 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow45_col5" class="data row45 col5" >0.048046</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row46" class="row_heading level0 row46" >46</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col0" class="data row46 col0" >New Mexico</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col1" class="data row46 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col2" class="data row46 col2" >1484</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col3" class="data row46 col3" >36</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col4" class="data row46 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow46_col5" class="data row46 col5" >0.024259</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row47" class="row_heading level0 row47" >47</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col0" class="data row47 col0" >South Dakota</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col1" class="data row47 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col2" class="data row47 col2" >1311</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col3" class="data row47 col3" >7</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col4" class="data row47 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow47_col5" class="data row47 col5" >0.005339</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row48" class="row_heading level0 row48" >48</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col0" class="data row48 col0" >Victoria</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col1" class="data row48 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col2" class="data row48 col2" >1299</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col3" class="data row48 col3" >14</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col4" class="data row48 col4" >1137</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow48_col5" class="data row48 col5" >0.010778</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row49" class="row_heading level0 row49" >49</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col0" class="data row49 col0" >Henan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col1" class="data row49 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col2" class="data row49 col2" >1276</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col3" class="data row49 col3" >22</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col4" class="data row49 col4" >1254</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow49_col5" class="data row49 col5" >0.017241</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row50" class="row_heading level0 row50" >50</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col0" class="data row50 col0" >Zhejiang</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col1" class="data row50 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col2" class="data row50 col2" >1268</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col3" class="data row50 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col4" class="data row50 col4" >1244</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow50_col5" class="data row50 col5" >0.000789</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row51" class="row_heading level0 row51" >51</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col0" class="data row51 col0" >New Hampshire</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col1" class="data row51 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col2" class="data row51 col2" >1139</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col3" class="data row51 col3" >32</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col4" class="data row51 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow51_col5" class="data row51 col5" >0.028095</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row52" class="row_heading level0 row52" >52</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col0" class="data row52 col0" >Puerto Rico</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col1" class="data row52 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col2" class="data row52 col2" >1043</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col3" class="data row52 col3" >56</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col4" class="data row52 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow52_col5" class="data row52 col5" >0.053691</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row53" class="row_heading level0 row53" >53</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col0" class="data row53 col0" >Hunan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col1" class="data row53 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col2" class="data row53 col2" >1019</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col3" class="data row53 col3" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col4" class="data row53 col4" >1014</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow53_col5" class="data row53 col5" >0.003925</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row54" class="row_heading level0 row54" >54</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col0" class="data row54 col0" >Hong Kong</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col1" class="data row54 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col2" class="data row54 col2" >1017</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col3" class="data row54 col3" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col4" class="data row54 col4" >485</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow54_col5" class="data row54 col5" >0.003933</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row55" class="row_heading level0 row55" >55</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col0" class="data row55 col0" >Queensland</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col1" class="data row55 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col2" class="data row55 col2" >1001</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col3" class="data row55 col3" >5</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col4" class="data row55 col4" >442</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow55_col5" class="data row55 col5" >0.004995</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row56" class="row_heading level0 row56" >56</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col0" class="data row56 col0" >Anhui</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col1" class="data row56 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col2" class="data row56 col2" >991</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col3" class="data row56 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col4" class="data row56 col4" >984</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow56_col5" class="data row56 col5" >0.006054</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row57" class="row_heading level0 row57" >57</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col0" class="data row57 col0" >Nebraska</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col1" class="data row57 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col2" class="data row57 col2" >952</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col3" class="data row57 col3" >21</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col4" class="data row57 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow57_col5" class="data row57 col5" >0.022059</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row58" class="row_heading level0 row58" >58</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col0" class="data row58 col0" >Jiangxi</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col1" class="data row58 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col2" class="data row58 col2" >937</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col3" class="data row58 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col4" class="data row58 col4" >936</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow58_col5" class="data row58 col5" >0.001067</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row59" class="row_heading level0 row59" >59</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col0" class="data row59 col0" >Heilongjiang</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col1" class="data row59 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col2" class="data row59 col2" >861</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col3" class="data row59 col3" >13</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col4" class="data row59 col4" >471</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow59_col5" class="data row59 col5" >0.015099</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row60" class="row_heading level0 row60" >60</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col0" class="data row60 col0" >Maine</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col1" class="data row60 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col2" class="data row60 col2" >796</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col3" class="data row60 col3" >27</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col4" class="data row60 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow60_col5" class="data row60 col5" >0.033920</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row61" class="row_heading level0 row61" >61</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col0" class="data row61 col0" >Shandong</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col1" class="data row61 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col2" class="data row61 col2" >784</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col3" class="data row61 col3" >7</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col4" class="data row61 col4" >761</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow61_col5" class="data row61 col5" >0.008929</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row62" class="row_heading level0 row62" >62</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col0" class="data row62 col0" >Vermont</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col1" class="data row62 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col2" class="data row62 col2" >774</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col3" class="data row62 col3" >33</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col4" class="data row62 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow62_col5" class="data row62 col5" >0.042636</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row63" class="row_heading level0 row63" >63</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col0" class="data row63 col0" >West Virginia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col1" class="data row63 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col2" class="data row63 col2" >728</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col3" class="data row63 col3" >12</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col4" class="data row63 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow63_col5" class="data row63 col5" >0.016484</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row64" class="row_heading level0 row64" >64</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col0" class="data row64 col0" >Jiangsu</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col1" class="data row64 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col2" class="data row64 col2" >653</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col3" class="data row64 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col4" class="data row64 col4" >642</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow64_col5" class="data row64 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row65" class="row_heading level0 row65" >65</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col0" class="data row65 col0" >Shanghai</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col1" class="data row65 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col2" class="data row65 col2" >628</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col3" class="data row65 col3" >7</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col4" class="data row65 col4" >489</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow65_col5" class="data row65 col5" >0.011146</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row66" class="row_heading level0 row66" >66</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col0" class="data row66 col0" >Beijing</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col1" class="data row66 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col2" class="data row66 col2" >593</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col3" class="data row66 col3" >8</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col4" class="data row66 col4" >503</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow66_col5" class="data row66 col5" >0.013491</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row67" class="row_heading level0 row67" >67</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col0" class="data row67 col0" >Chongqing</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col1" class="data row67 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col2" class="data row67 col2" >579</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col3" class="data row67 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col4" class="data row67 col4" >570</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow67_col5" class="data row67 col5" >0.010363</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row68" class="row_heading level0 row68" >68</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col0" class="data row68 col0" >Nova Scotia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col1" class="data row68 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col2" class="data row68 col2" >579</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col3" class="data row68 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col4" class="data row68 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow68_col5" class="data row68 col5" >0.005181</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row69" class="row_heading level0 row69" >69</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col0" class="data row69 col0" >Sichuan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col1" class="data row69 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col2" class="data row69 col2" >560</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col3" class="data row69 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col4" class="data row69 col4" >552</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow69_col5" class="data row69 col5" >0.005357</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row70" class="row_heading level0 row70" >70</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col0" class="data row70 col0" >Western Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col1" class="data row70 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col2" class="data row70 col2" >532</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col3" class="data row70 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col4" class="data row70 col4" >338</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow70_col5" class="data row70 col5" >0.011278</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row71" class="row_heading level0 row71" >71</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col0" class="data row71 col0" >Hawaii</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col1" class="data row71 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col2" class="data row71 col2" >530</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col3" class="data row71 col3" >9</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col4" class="data row71 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow71_col5" class="data row71 col5" >0.016981</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row72" class="row_heading level0 row72" >72</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col0" class="data row72 col0" >Channel Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col1" class="data row72 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col2" class="data row72 col2" >457</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col3" class="data row72 col3" >19</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col4" class="data row72 col4" >73</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow72_col5" class="data row72 col5" >0.041575</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row73" class="row_heading level0 row73" >73</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col0" class="data row73 col0" >South Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col1" class="data row73 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col2" class="data row73 col2" >433</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col3" class="data row73 col3" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col4" class="data row73 col4" >279</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow73_col5" class="data row73 col5" >0.009238</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row74" class="row_heading level0 row74" >74</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col0" class="data row74 col0" >Montana</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col1" class="data row74 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col2" class="data row74 col2" >415</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col3" class="data row74 col3" >7</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col4" class="data row74 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow74_col5" class="data row74 col5" >0.016867</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row75" class="row_heading level0 row75" >75</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col0" class="data row75 col0" >Reunion</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col1" class="data row75 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col2" class="data row75 col2" >394</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col3" class="data row75 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col4" class="data row75 col4" >237</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow75_col5" class="data row75 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row76" class="row_heading level0 row76" >76</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col0" class="data row76 col0" >North Dakota</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col1" class="data row76 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col2" class="data row76 col2" >393</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col3" class="data row76 col3" >9</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col4" class="data row76 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow76_col5" class="data row76 col5" >0.022901</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row77" class="row_heading level0 row77" >77</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col0" class="data row77 col0" >Fujian</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col1" class="data row77 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col2" class="data row77 col2" >353</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col3" class="data row77 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col4" class="data row77 col4" >333</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow77_col5" class="data row77 col5" >0.002833</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row78" class="row_heading level0 row78" >78</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col0" class="data row78 col0" >Hebei</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col1" class="data row78 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col2" class="data row78 col2" >328</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col3" class="data row78 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col4" class="data row78 col4" >315</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow78_col5" class="data row78 col5" >0.018293</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row79" class="row_heading level0 row79" >79</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col0" class="data row79 col0" >Saskatchewan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col1" class="data row79 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col2" class="data row79 col2" >305</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col3" class="data row79 col3" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col4" class="data row79 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow79_col5" class="data row79 col5" >0.013115</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row80" class="row_heading level0 row80" >80</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col0" class="data row80 col0" >Alaska</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col1" class="data row80 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col2" class="data row80 col2" >300</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col3" class="data row80 col3" >9</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col4" class="data row80 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow80_col5" class="data row80 col5" >0.030000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row81" class="row_heading level0 row81" >81</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col0" class="data row81 col0" >Wyoming</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col1" class="data row81 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col2" class="data row81 col2" >296</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col3" class="data row81 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col4" class="data row81 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow81_col5" class="data row81 col5" >0.006757</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row82" class="row_heading level0 row82" >82</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col0" class="data row82 col0" >Isle of Man</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col1" class="data row82 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col2" class="data row82 col2" >284</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col3" class="data row82 col3" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col4" class="data row82 col4" >154</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow82_col5" class="data row82 col5" >0.014085</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row83" class="row_heading level0 row83" >83</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col0" class="data row83 col0" >Shaanxi</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col1" class="data row83 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col2" class="data row83 col2" >256</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col3" class="data row83 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col4" class="data row83 col4" >251</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow83_col5" class="data row83 col5" >0.011719</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row84" class="row_heading level0 row84" >84</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col0" class="data row84 col0" >Guangxi</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col1" class="data row84 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col2" class="data row84 col2" >254</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col3" class="data row84 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col4" class="data row84 col4" >252</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow84_col5" class="data row84 col5" >0.007874</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row85" class="row_heading level0 row85" >85</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col0" class="data row85 col0" >Newfoundland and Labrador</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col1" class="data row85 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col2" class="data row85 col2" >252</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col3" class="data row85 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col4" class="data row85 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow85_col5" class="data row85 col5" >0.011905</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row86" class="row_heading level0 row86" >86</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col0" class="data row86 col0" >Manitoba</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col1" class="data row86 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col2" class="data row86 col2" >250</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col3" class="data row86 col3" >5</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col4" class="data row86 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow86_col5" class="data row86 col5" >0.020000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row87" class="row_heading level0 row87" >87</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col0" class="data row87 col0" >Mayotte</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col1" class="data row87 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col2" class="data row87 col2" >233</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col3" class="data row87 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col4" class="data row87 col4" >69</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow87_col5" class="data row87 col5" >0.012876</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row88" class="row_heading level0 row88" >88</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col0" class="data row88 col0" >Shanxi</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col1" class="data row88 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col2" class="data row88 col2" >194</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col3" class="data row88 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col4" class="data row88 col4" >135</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow88_col5" class="data row88 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row89" class="row_heading level0 row89" >89</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col0" class="data row89 col0" >Inner Mongolia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col1" class="data row89 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col2" class="data row89 col2" >193</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col3" class="data row89 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col4" class="data row89 col4" >94</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow89_col5" class="data row89 col5" >0.005181</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row90" class="row_heading level0 row90" >90</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col0" class="data row90 col0" >Tianjin</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col1" class="data row90 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col2" class="data row90 col2" >186</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col3" class="data row90 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col4" class="data row90 col4" >172</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow90_col5" class="data row90 col5" >0.016129</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row91" class="row_heading level0 row91" >91</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col0" class="data row91 col0" >Faroe Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col1" class="data row91 col1" >Denmark</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col2" class="data row91 col2" >184</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col3" class="data row91 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col4" class="data row91 col4" >169</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow91_col5" class="data row91 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row92" class="row_heading level0 row92" >92</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col0" class="data row92 col0" >Yunnan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col1" class="data row92 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col2" class="data row92 col2" >184</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col3" class="data row92 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col4" class="data row92 col4" >176</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow92_col5" class="data row92 col5" >0.010870</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row93" class="row_heading level0 row93" >93</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col0" class="data row93 col0" >Tasmania</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col1" class="data row93 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col2" class="data row93 col2" >169</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col3" class="data row93 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col4" class="data row93 col4" >67</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow93_col5" class="data row93 col5" >0.035503</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row94" class="row_heading level0 row94" >94</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col0" class="data row94 col0" >Hainan</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col1" class="data row94 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col2" class="data row94 col2" >168</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col3" class="data row94 col3" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col4" class="data row94 col4" >162</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow94_col5" class="data row94 col5" >0.035714</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row95" class="row_heading level0 row95" >95</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col0" class="data row95 col0" >Martinique</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col1" class="data row95 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col2" class="data row95 col2" >158</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col3" class="data row95 col3" >8</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col4" class="data row95 col4" >73</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow95_col5" class="data row95 col5" >0.050633</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row96" class="row_heading level0 row96" >96</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col0" class="data row96 col0" >Guizhou</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col1" class="data row96 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col2" class="data row96 col2" >146</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col3" class="data row96 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col4" class="data row96 col4" >144</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow96_col5" class="data row96 col5" >0.013699</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row97" class="row_heading level0 row97" >97</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col0" class="data row97 col0" >Guadeloupe</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col1" class="data row97 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col2" class="data row97 col2" >145</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col3" class="data row97 col3" >8</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col4" class="data row97 col4" >67</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow97_col5" class="data row97 col5" >0.055172</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row98" class="row_heading level0 row98" >98</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col0" class="data row98 col0" >Liaoning</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col1" class="data row98 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col2" class="data row98 col2" >145</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col3" class="data row98 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col4" class="data row98 col4" >140</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow98_col5" class="data row98 col5" >0.013793</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row99" class="row_heading level0 row99" >99</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col0" class="data row99 col0" >Gansu</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col1" class="data row99 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col2" class="data row99 col2" >139</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col3" class="data row99 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col4" class="data row99 col4" >137</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow99_col5" class="data row99 col5" >0.014388</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row100" class="row_heading level0 row100" >100</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col0" class="data row100 col0" >Guam</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col1" class="data row100 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col2" class="data row100 col2" >135</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col3" class="data row100 col3" >5</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col4" class="data row100 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow100_col5" class="data row100 col5" >0.037037</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row101" class="row_heading level0 row101" >101</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col0" class="data row101 col0" >Gibraltar</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col1" class="data row101 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col2" class="data row101 col2" >131</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col3" class="data row101 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col4" class="data row101 col4" >104</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow101_col5" class="data row101 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row102" class="row_heading level0 row102" >102</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col0" class="data row102 col0" >New Brunswick</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col1" class="data row102 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col2" class="data row102 col2" >117</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col3" class="data row102 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col4" class="data row102 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow102_col5" class="data row102 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row103" class="row_heading level0 row103" >103</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col0" class="data row103 col0" >Grand Princess</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col1" class="data row103 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col2" class="data row103 col2" >116</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col3" class="data row103 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col4" class="data row103 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow103_col5" class="data row103 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row104" class="row_heading level0 row104" >104</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col0" class="data row104 col0" >Australian Capital Territory</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col1" class="data row104 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col2" class="data row104 col2" >103</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col3" class="data row104 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col4" class="data row104 col4" >82</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow104_col5" class="data row104 col5" >0.029126</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row105" class="row_heading level0 row105" >105</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col0" class="data row105 col0" >Jilin</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col1" class="data row105 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col2" class="data row105 col2" >102</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col3" class="data row105 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col4" class="data row105 col4" >96</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow105_col5" class="data row105 col5" >0.009804</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row106" class="row_heading level0 row106" >106</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col0" class="data row106 col0" >Aruba</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col1" class="data row106 col1" >Netherlands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col2" class="data row106 col2" >95</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col3" class="data row106 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col4" class="data row106 col4" >39</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow106_col5" class="data row106 col5" >0.021053</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row107" class="row_heading level0 row107" >107</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col0" class="data row107 col0" >French Guiana</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col1" class="data row107 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col2" class="data row107 col2" >86</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col3" class="data row107 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col4" class="data row107 col4" >51</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow107_col5" class="data row107 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row108" class="row_heading level0 row108" >108</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col0" class="data row108 col0" >Bermuda</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col1" class="data row108 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col2" class="data row108 col2" >81</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col3" class="data row108 col3" >5</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col4" class="data row108 col4" >33</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow108_col5" class="data row108 col5" >0.061728</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row109" class="row_heading level0 row109" >109</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col0" class="data row109 col0" >Xinjiang</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col1" class="data row109 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col2" class="data row109 col2" >76</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col3" class="data row109 col3" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col4" class="data row109 col4" >73</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow109_col5" class="data row109 col5" >0.039474</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row110" class="row_heading level0 row110" >110</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col0" class="data row110 col0" >Ningxia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col1" class="data row110 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col2" class="data row110 col2" >75</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col3" class="data row110 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col4" class="data row110 col4" >75</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow110_col5" class="data row110 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row111" class="row_heading level0 row111" >111</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col0" class="data row111 col0" >Cayman Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col1" class="data row111 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col2" class="data row111 col2" >60</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col3" class="data row111 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col4" class="data row111 col4" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow111_col5" class="data row111 col5" >0.016667</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row112" class="row_heading level0 row112" >112</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col0" class="data row112 col0" >Sint Maarten</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col1" class="data row112 col1" >Netherlands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col2" class="data row112 col2" >57</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col3" class="data row112 col3" >9</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col4" class="data row112 col4" >12</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow112_col5" class="data row112 col5" >0.157895</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row113" class="row_heading level0 row113" >113</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col0" class="data row113 col0" >French Polynesia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col1" class="data row113 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col2" class="data row113 col2" >55</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col3" class="data row113 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col4" class="data row113 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow113_col5" class="data row113 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row114" class="row_heading level0 row114" >114</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col0" class="data row114 col0" >Virgin Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col1" class="data row114 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col2" class="data row114 col2" >51</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col3" class="data row114 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col4" class="data row114 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow114_col5" class="data row114 col5" >0.019608</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row115" class="row_heading level0 row115" >115</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col0" class="data row115 col0" >Diamond Princess</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col1" class="data row115 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col2" class="data row115 col2" >49</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col3" class="data row115 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col4" class="data row115 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow115_col5" class="data row115 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row116" class="row_heading level0 row116" >116</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col0" class="data row116 col0" >Macau</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col1" class="data row116 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col2" class="data row116 col2" >45</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col3" class="data row116 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col4" class="data row116 col4" >16</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow116_col5" class="data row116 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row117" class="row_heading level0 row117" >117</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col0" class="data row117 col0" >St Martin</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col1" class="data row117 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col2" class="data row117 col2" >35</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col3" class="data row117 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col4" class="data row117 col4" >13</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow117_col5" class="data row117 col5" >0.057143</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row118" class="row_heading level0 row118" >118</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col0" class="data row118 col0" >Northern Territory</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col1" class="data row118 col1" >Australia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col2" class="data row118 col2" >28</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col3" class="data row118 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col4" class="data row118 col4" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow118_col5" class="data row118 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row119" class="row_heading level0 row119" >119</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col0" class="data row119 col0" >Prince Edward Island</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col1" class="data row119 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col2" class="data row119 col2" >26</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col3" class="data row119 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col4" class="data row119 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow119_col5" class="data row119 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row120" class="row_heading level0 row120" >120</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col0" class="data row120 col0" >New Caledonia</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col1" class="data row120 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col2" class="data row120 col2" >18</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col3" class="data row120 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col4" class="data row120 col4" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow120_col5" class="data row120 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row121" class="row_heading level0 row121" >121</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col0" class="data row121 col0" >Qinghai</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col1" class="data row121 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col2" class="data row121 col2" >18</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col3" class="data row121 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col4" class="data row121 col4" >18</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow121_col5" class="data row121 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row122" class="row_heading level0 row122" >122</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col0" class="data row122 col0" >Curacao</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col1" class="data row122 col1" >Netherlands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col2" class="data row122 col2" >14</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col3" class="data row122 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col4" class="data row122 col4" >10</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow122_col5" class="data row122 col5" >0.071429</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row123" class="row_heading level0 row123" >123</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col0" class="data row123 col0" >Northern Mariana Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col1" class="data row123 col1" >US</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col2" class="data row123 col2" >13</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col3" class="data row123 col3" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col4" class="data row123 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow123_col5" class="data row123 col5" >0.153846</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row124" class="row_heading level0 row124" >124</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col0" class="data row124 col0" >Falkland Islands (Malvinas)</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col1" class="data row124 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col2" class="data row124 col2" >11</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col3" class="data row124 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col4" class="data row124 col4" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow124_col5" class="data row124 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row125" class="row_heading level0 row125" >125</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col0" class="data row125 col0" >Greenland</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col1" class="data row125 col1" >Denmark</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col2" class="data row125 col2" >11</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col3" class="data row125 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col4" class="data row125 col4" >11</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow125_col5" class="data row125 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row126" class="row_heading level0 row126" >126</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col0" class="data row126 col0" >Montserrat</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col1" class="data row126 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col2" class="data row126 col2" >11</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col3" class="data row126 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col4" class="data row126 col4" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow126_col5" class="data row126 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row127" class="row_heading level0 row127" >127</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col0" class="data row127 col0" >Turks and Caicos Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col1" class="data row127 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col2" class="data row127 col2" >11</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col3" class="data row127 col3" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col4" class="data row127 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow127_col5" class="data row127 col5" >0.090909</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row128" class="row_heading level0 row128" >128</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col0" class="data row128 col0" >Yukon</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col1" class="data row128 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col2" class="data row128 col2" >8</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col3" class="data row128 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col4" class="data row128 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow128_col5" class="data row128 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row129" class="row_heading level0 row129" >129</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col0" class="data row129 col0" >Saint Barthelemy</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col1" class="data row129 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col2" class="data row129 col2" >6</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col3" class="data row129 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col4" class="data row129 col4" >4</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow129_col5" class="data row129 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row130" class="row_heading level0 row130" >130</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col0" class="data row130 col0" >Northwest Territories</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col1" class="data row130 col1" >Canada</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col2" class="data row130 col2" >5</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col3" class="data row130 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col4" class="data row130 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow130_col5" class="data row130 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row131" class="row_heading level0 row131" >131</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col0" class="data row131 col0" >Anguilla</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col1" class="data row131 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col2" class="data row131 col2" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col3" class="data row131 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col4" class="data row131 col4" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow131_col5" class="data row131 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row132" class="row_heading level0 row132" >132</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col0" class="data row132 col0" >Bonaire, Sint Eustatius and Saba</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col1" class="data row132 col1" >Netherlands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col2" class="data row132 col2" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col3" class="data row132 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col4" class="data row132 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow132_col5" class="data row132 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row133" class="row_heading level0 row133" >133</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col0" class="data row133 col0" >British Virgin Islands</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col1" class="data row133 col1" >United Kingdom</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col2" class="data row133 col2" >3</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col3" class="data row133 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col4" class="data row133 col4" >2</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow133_col5" class="data row133 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row134" class="row_heading level0 row134" >134</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col0" class="data row134 col0" >Saint Pierre and Miquelon</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col1" class="data row134 col1" >France</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col2" class="data row134 col2" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col3" class="data row134 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col4" class="data row134 col4" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow134_col5" class="data row134 col5" >0.000000</td>
            </tr>
            <tr>
                        <th id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9dlevel0_row135" class="row_heading level0 row135" >135</th>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col0" class="data row135 col0" >Tibet</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col1" class="data row135 col1" >China</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col2" class="data row135 col2" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col3" class="data row135 col3" >0</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col4" class="data row135 col4" >1</td>
                        <td id="T_557b0f1c_8128_11ea_a64b_ec5c68c44a9drow135_col5" class="data row135 col5" >0.000000</td>
            </tr>
    </tbody></table>




```python
nan_indices = [] 

# handle nan if there is any, it is usually a float: float('nan')

for i in range(len(unique_provinces)):
    if type(unique_provinces[i]) == float:
        nan_indices.append(i)

unique_provinces = list(unique_provinces)
province_confirmed_cases = list(province_confirmed_cases)

for i in nan_indices:
    unique_provinces.pop(i)
    province_confirmed_cases.pop(i)
```


```python
china_confirmed = latest_data[latest_data['Country_Region']=='China']['Confirmed'].sum()
outside_mainland_china_confirmed = np.sum(country_confirmed_cases) - china_confirmed
plt.figure(figsize=(16, 9))
plt.barh('Mainland China', china_confirmed)
plt.barh('Outside Mainland China', outside_mainland_china_confirmed)
plt.title('# of Coronavirus Confirmed Cases', size=20)
plt.xticks(size=20)
plt.yticks(size=20)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_54_0.png)



```python
print('Outside Mainland China: {} cases'.format(outside_mainland_china_confirmed))
print('Mainland China: {} cases'.format(china_confirmed))
print('Total: {} cases'.format(china_confirmed+outside_mainland_china_confirmed))
```

    Outside Mainland China: 2069244 cases
    Mainland China: 83403 cases
    Total: 2152647 cases



```python
# Only show 10 countries with the most confirmed cases, the rest are grouped into the other category
visual_unique_countries = [] 
visual_confirmed_cases = []
others = np.sum(country_confirmed_cases[10:])

for i in range(len(country_confirmed_cases[:10])):
    visual_unique_countries.append(unique_countries[i])
    visual_confirmed_cases.append(country_confirmed_cases[i])
    
visual_unique_countries.append('Others')
visual_confirmed_cases.append(others)
```


```python
def plot_bar_graphs(x, y, title):
    plt.figure(figsize=(16, 9))
    plt.barh(x, y)
    plt.title(title, size=20)
    plt.xticks(size=20)
    plt.yticks(size=20)
    plt.show()
```


```python
plot_bar_graphs(visual_unique_countries, visual_confirmed_cases, '# of Covid-19 Confirmed Cases in Countries/Regions')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_58_0.png)



```python
# Only show 10 provinces with the most confirmed cases, the rest are grouped into the other category
visual_unique_provinces = [] 
visual_confirmed_cases2 = []
others = np.sum(province_confirmed_cases[10:])
for i in range(len(province_confirmed_cases[:10])):
    visual_unique_provinces.append(unique_provinces[i])
    visual_confirmed_cases2.append(province_confirmed_cases[i])

visual_unique_provinces.append('Others')
visual_confirmed_cases2.append(others)
```


```python
plot_bar_graphs(visual_unique_provinces, visual_confirmed_cases2, '# of Coronavirus Confirmed Cases in Provinces/States')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_60_0.png)



```python
import matplotlib.colors as mcolors


def plot_pie_charts(x, y, title): 
    random.seed(23)
    c = random.choices(list(mcolors.CSS4_COLORS.values()),k = len(unique_countries))
    plt.figure(figsize=(20,15))
    plt.title(title, size=20)
    plt.pie(y, colors=c)
    plt.legend(x, loc='best', fontsize=15)
    plt.show()
```


```python
plot_pie_charts(visual_unique_countries, visual_confirmed_cases, 'Covid-19 Confirmed Cases per Country')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_62_0.png)



```python
plot_pie_charts(visual_unique_provinces, visual_confirmed_cases2, 'Covid-19 Confirmed Cases per State/Province/Region')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_63_0.png)



```python
# Plotting countries with regional data using a pie chart 

def plot_pie_country_with_regions(country_name, title):
    regions = list(latest_data[latest_data['Country_Region']==country_name]['Province_State'].unique())
    confirmed_cases = []
    no_cases = [] 
    for i in regions:
        cases = latest_data[latest_data['Province_State']==i]['Confirmed'].sum()
        if cases > 0:
            confirmed_cases.append(cases)
        else:
            no_cases.append(i)

    # remove areas with no confirmed cases
    for i in no_cases:
        regions.remove(i)

    # only show the top 10 states
    regions = [k for k, v in sorted(zip(regions, confirmed_cases), key=operator.itemgetter(1), reverse=True)]

    for i in range(len(regions)):
        confirmed_cases[i] = latest_data[latest_data['Province_State']==regions[i]]['Confirmed'].sum()  
    
    # additional province/state will be considered "others"
    
    if(len(regions)>10):
        regions_10 = regions[:10]
        regions_10.append('Others')
        confirmed_cases_10 = confirmed_cases[:10]
        confirmed_cases_10.append(np.sum(confirmed_cases[10:]))
        plot_pie_charts(regions_10,confirmed_cases_10, title)
    else:
        plot_pie_charts(regions,confirmed_cases, title)
```


```python
plot_pie_country_with_regions('US', 'COVID-19 Confirmed Cases in the United States')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_65_0.png)



```python
plot_pie_country_with_regions('China', 'COVID-19 Confirmed Cases in China')
```


![_config.yml]({{ site.baseurl}}/images/20200418_output_66_0.png)

-mkc2020
