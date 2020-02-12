Download and extract data from: https://data.worldbank.org/country/philippines (Both CSV and EXCEL for demo)

![](ph_data.png)

## 1. Loading .csv file


```python
import pandas as pd
```


```python
# Loading the file. MAKE SURE THEY ARE IN THE SAME DIRECTORY, FILE AND NOTEBOOK.
raw = pd.read_csv('API_PHL_DS2_en_csv_v2_713923.csv', skiprows=4)
```


```python
raw.head()
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
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>Unnamed: 64</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Child employment in services (% of economicall...</td>
      <td>SL.SRV.0714.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>40.19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Child employment in services, male (% of male ...</td>
      <td>SL.SRV.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>31.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Child employment in services, female (% of fem...</td>
      <td>SL.SRV.0714.FE.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>54.65</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Children in employment, self-employed (% of ch...</td>
      <td>SL.SLF.0714.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>4.78</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Children in employment, self-employed, male (%...</td>
      <td>SL.SLF.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.73</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 65 columns</p>
</div>




```python
raw.set_index('Indicator Name', inplace=True)
raw.head()
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
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>...</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>Unnamed: 64</th>
    </tr>
    <tr>
      <th>Indicator Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.SRV.0714.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>40.19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.SRV.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>31.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.SRV.0714.FE.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>54.65</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.SLF.0714.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>4.78</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.SLF.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.73</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 64 columns</p>
</div>




```python
data = raw.iloc[:,3:]
data.head()
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
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>1967</th>
      <th>1968</th>
      <th>1969</th>
      <th>...</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
      <th>Unnamed: 64</th>
    </tr>
    <tr>
      <th>Indicator Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>40.19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>31.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>54.65</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>4.78</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.73</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 61 columns</p>
</div>




```python
data = data.T
data.head()
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
      <th>Indicator Name</th>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, female (% of female children in employment, ages 7-14)</th>
      <th>Child employment in manufacturing (% of economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, female (% of female economically active children ages 7-14)</th>
      <th>Informal employment (% of total non-agricultural employment)</th>
      <th>...</th>
      <th>Net official flows from UN agencies, UNAIDS (current US$)</th>
      <th>Net financial flows, RDB nonconcessional (NFL, current US$)</th>
      <th>Net financial flows, RDB concessional (NFL, current US$)</th>
      <th>PPG, private creditors (NFL, current US$)</th>
      <th>PPG, other private creditors (NFL, current US$)</th>
      <th>PNG, commercial banks and other creditors (NFL, current US$)</th>
      <th>PNG, bonds (NFL, current US$)</th>
      <th>Commercial banks and other lending (PPG + PNG) (NFL, current US$)</th>
      <th>PPG, commercial banks (NFL, current US$)</th>
      <th>PPG, bonds (NFL, current US$)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1960</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1961</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1962</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1963</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1964</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1429 columns</p>
</div>




```python
data.tail()
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
      <th>Indicator Name</th>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, female (% of female children in employment, ages 7-14)</th>
      <th>Child employment in manufacturing (% of economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, female (% of female economically active children ages 7-14)</th>
      <th>Informal employment (% of total non-agricultural employment)</th>
      <th>...</th>
      <th>Net official flows from UN agencies, UNAIDS (current US$)</th>
      <th>Net financial flows, RDB nonconcessional (NFL, current US$)</th>
      <th>Net financial flows, RDB concessional (NFL, current US$)</th>
      <th>PPG, private creditors (NFL, current US$)</th>
      <th>PPG, other private creditors (NFL, current US$)</th>
      <th>PNG, commercial banks and other creditors (NFL, current US$)</th>
      <th>PNG, bonds (NFL, current US$)</th>
      <th>Commercial banks and other lending (PPG + PNG) (NFL, current US$)</th>
      <th>PPG, commercial banks (NFL, current US$)</th>
      <th>PPG, bonds (NFL, current US$)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>330000.0</td>
      <td>395910000.0</td>
      <td>-95158000.0</td>
      <td>-2.532091e+09</td>
      <td>-2256000.0</td>
      <td>-1.074806e+09</td>
      <td>-100000000.0</td>
      <td>-940524000.0</td>
      <td>136538000.0</td>
      <td>-2.666373e+09</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>190000.0</td>
      <td>126293000.0</td>
      <td>-96047000.0</td>
      <td>3.258590e+08</td>
      <td>-2225000.0</td>
      <td>-6.331680e+08</td>
      <td>700000000.0</td>
      <td>-678071000.0</td>
      <td>-42678000.0</td>
      <td>3.707620e+08</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>401180000.0</td>
      <td>-82567000.0</td>
      <td>2.587989e+09</td>
      <td>-2219000.0</td>
      <td>-1.538270e+08</td>
      <td>725000000.0</td>
      <td>-184586000.0</td>
      <td>-28540000.0</td>
      <td>2.618748e+09</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Unnamed: 64</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1429 columns</p>
</div>




```python
# Remove "Unnamed: 64" extra row
data = data.iloc[:-1]
data.tail()
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
      <th>Indicator Name</th>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, female (% of female children in employment, ages 7-14)</th>
      <th>Child employment in manufacturing (% of economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, female (% of female economically active children ages 7-14)</th>
      <th>Informal employment (% of total non-agricultural employment)</th>
      <th>...</th>
      <th>Net official flows from UN agencies, UNAIDS (current US$)</th>
      <th>Net financial flows, RDB nonconcessional (NFL, current US$)</th>
      <th>Net financial flows, RDB concessional (NFL, current US$)</th>
      <th>PPG, private creditors (NFL, current US$)</th>
      <th>PPG, other private creditors (NFL, current US$)</th>
      <th>PNG, commercial banks and other creditors (NFL, current US$)</th>
      <th>PNG, bonds (NFL, current US$)</th>
      <th>Commercial banks and other lending (PPG + PNG) (NFL, current US$)</th>
      <th>PPG, commercial banks (NFL, current US$)</th>
      <th>PPG, bonds (NFL, current US$)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>350000.0</td>
      <td>610131000.0</td>
      <td>-92764000.0</td>
      <td>2.497690e+09</td>
      <td>-2025000.0</td>
      <td>1.992369e+09</td>
      <td>-537502000.0</td>
      <td>1.895185e+09</td>
      <td>-95159000.0</td>
      <td>2.594874e+09</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>330000.0</td>
      <td>395910000.0</td>
      <td>-95158000.0</td>
      <td>-2.532091e+09</td>
      <td>-2256000.0</td>
      <td>-1.074806e+09</td>
      <td>-100000000.0</td>
      <td>-9.405240e+08</td>
      <td>136538000.0</td>
      <td>-2.666373e+09</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>190000.0</td>
      <td>126293000.0</td>
      <td>-96047000.0</td>
      <td>3.258590e+08</td>
      <td>-2225000.0</td>
      <td>-6.331680e+08</td>
      <td>700000000.0</td>
      <td>-6.780710e+08</td>
      <td>-42678000.0</td>
      <td>3.707620e+08</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>401180000.0</td>
      <td>-82567000.0</td>
      <td>2.587989e+09</td>
      <td>-2219000.0</td>
      <td>-1.538270e+08</td>
      <td>725000000.0</td>
      <td>-1.845860e+08</td>
      <td>-28540000.0</td>
      <td>2.618748e+09</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1429 columns</p>
</div>




```python
#Basic plotting
indicator = 'Population, total'
data[indicator].plot(grid=True, figsize=(14,8), title=indicator, lw = 4, marker='.', markersize=20)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f918f43a610>




![_config.yml]({{ site.baseurl}}/images/20200212_output_10_1.png)



```python
data.tail()
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
      <th>Indicator Name</th>
      <th>Child employment in services (% of economically active children ages 7-14)</th>
      <th>Child employment in services, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in services, female (% of female economically active children ages 7-14)</th>
      <th>Children in employment, self-employed (% of children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, male (% of male children in employment, ages 7-14)</th>
      <th>Children in employment, self-employed, female (% of female children in employment, ages 7-14)</th>
      <th>Child employment in manufacturing (% of economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in manufacturing, female (% of female economically active children ages 7-14)</th>
      <th>Informal employment (% of total non-agricultural employment)</th>
      <th>...</th>
      <th>Net official flows from UN agencies, UNAIDS (current US$)</th>
      <th>Net financial flows, RDB nonconcessional (NFL, current US$)</th>
      <th>Net financial flows, RDB concessional (NFL, current US$)</th>
      <th>PPG, private creditors (NFL, current US$)</th>
      <th>PPG, other private creditors (NFL, current US$)</th>
      <th>PNG, commercial banks and other creditors (NFL, current US$)</th>
      <th>PNG, bonds (NFL, current US$)</th>
      <th>Commercial banks and other lending (PPG + PNG) (NFL, current US$)</th>
      <th>PPG, commercial banks (NFL, current US$)</th>
      <th>PPG, bonds (NFL, current US$)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>330000.0</td>
      <td>395910000.0</td>
      <td>-95158000.0</td>
      <td>-2.532091e+09</td>
      <td>-2256000.0</td>
      <td>-1.074806e+09</td>
      <td>-100000000.0</td>
      <td>-940524000.0</td>
      <td>136538000.0</td>
      <td>-2.666373e+09</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>190000.0</td>
      <td>126293000.0</td>
      <td>-96047000.0</td>
      <td>3.258590e+08</td>
      <td>-2225000.0</td>
      <td>-6.331680e+08</td>
      <td>700000000.0</td>
      <td>-678071000.0</td>
      <td>-42678000.0</td>
      <td>3.707620e+08</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>401180000.0</td>
      <td>-82567000.0</td>
      <td>2.587989e+09</td>
      <td>-2219000.0</td>
      <td>-1.538270e+08</td>
      <td>725000000.0</td>
      <td>-184586000.0</td>
      <td>-28540000.0</td>
      <td>2.618748e+09</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Unnamed: 64</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1429 columns</p>
</div>




```python
indicators = ['Population, total','Population, male','Population, female']
data[indicators].plot(grid=True, figsize=(14,8), title=indicator, lw = 4, marker='.', markersize=16)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f918ed1da10>




![_config.yml]({{ site.baseurl}}/images/20200212_output_12_1.png)



```python
data[indicators].plot(grid=True, figsize=(16,8), kind='bar')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f918e6b4b90>




![_config.yml]({{ site.baseurl}}/images/20200212_output_13_1.png)


# 2. Loading .xls file


```python
# Loading the file. MAKE SURE THEY ARE IN THE SAME DIRECTORY, FILE AND NOTEBOOK.
raw = pd.read_excel("API_PHL_DS2_en_excel_v2_716225.xls", header=3, sheet_name='Data')
raw.head()
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
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Name</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>...</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Child employment in agriculture, male (% of ma...</td>
      <td>SL.AGR.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>64.40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Child employment in agriculture, female (% of ...</td>
      <td>SL.AGR.0714.FE.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>38.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Annualized average growth rate in per capita r...</td>
      <td>SI.SPR.PCAP.ZG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.56</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Survey mean consumption or income per capita, ...</td>
      <td>SI.SPR.PCAP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.92</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>Annualized average growth rate in per capita r...</td>
      <td>SI.SPR.PC40.ZG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.12</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 64 columns</p>
</div>




```python
raw.set_index('Indicator Name', inplace=True)
raw.head()
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
      <th>Country Name</th>
      <th>Country Code</th>
      <th>Indicator Code</th>
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>...</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
    <tr>
      <th>Indicator Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Child employment in agriculture, male (% of male economically active children ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.AGR.0714.MA.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>64.40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in agriculture, female (% of female economically active children ages 7-14)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SL.AGR.0714.FE.ZS</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>38.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, total population (%)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SI.SPR.PCAP.ZG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.56</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Survey mean consumption or income per capita, total population (2011 PPP $ per day)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SI.SPR.PCAP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.92</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, bottom 40% of population (%)</th>
      <td>Philippines</td>
      <td>PHL</td>
      <td>SI.SPR.PC40.ZG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.12</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>
</div>




```python
# Choose Numeric Values ONLY
data = raw.iloc[:,3:]
data.head()
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
      <th>1960</th>
      <th>1961</th>
      <th>1962</th>
      <th>1963</th>
      <th>1964</th>
      <th>1965</th>
      <th>1966</th>
      <th>1967</th>
      <th>1968</th>
      <th>1969</th>
      <th>...</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
    <tr>
      <th>Indicator Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Child employment in agriculture, male (% of male economically active children ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>64.40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Child employment in agriculture, female (% of female economically active children ages 7-14)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>38.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, total population (%)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.56</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Survey mean consumption or income per capita, total population (2011 PPP $ per day)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.92</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, bottom 40% of population (%)</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.12</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 60 columns</p>
</div>




```python
# Transpose
data = data.T
data.tail()
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
      <th>Indicator Name</th>
      <th>Child employment in agriculture, male (% of male economically active children ages 7-14)</th>
      <th>Child employment in agriculture, female (% of female economically active children ages 7-14)</th>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, total population (%)</th>
      <th>Survey mean consumption or income per capita, total population (2011 PPP $ per day)</th>
      <th>Annualized average growth rate in per capita real survey mean consumption or income, bottom 40% of population (%)</th>
      <th>Survey mean consumption or income per capita, bottom 40% of population (2011 PPP $ per day)</th>
      <th>Average transaction cost of sending remittances from a specific country (%)</th>
      <th>Average transaction cost of sending remittances to a specific country (%)</th>
      <th>Urban poverty headcount ratio at national poverty lines (% of urban population)</th>
      <th>Urban poverty gap at national poverty lines (%)</th>
      <th>...</th>
      <th>Net official flows from UN agencies, UNAIDS (current US$)</th>
      <th>Net financial flows, RDB nonconcessional (NFL, current US$)</th>
      <th>Net financial flows, RDB concessional (NFL, current US$)</th>
      <th>PPG, private creditors (NFL, current US$)</th>
      <th>PPG, other private creditors (NFL, current US$)</th>
      <th>PNG, commercial banks and other creditors (NFL, current US$)</th>
      <th>PNG, bonds (NFL, current US$)</th>
      <th>Commercial banks and other lending (PPG + PNG) (NFL, current US$)</th>
      <th>PPG, commercial banks (NFL, current US$)</th>
      <th>PPG, bonds (NFL, current US$)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.56</td>
      <td>7.46</td>
      <td>5.12</td>
      <td>2.79</td>
      <td>NaN</td>
      <td>6.248535</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>350000.0</td>
      <td>610131000.0</td>
      <td>-92764000.0</td>
      <td>2.497690e+09</td>
      <td>-2025000.0</td>
      <td>1.992369e+09</td>
      <td>-537502000.0</td>
      <td>1.895185e+09</td>
      <td>-95159000.0</td>
      <td>2.594874e+09</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.727137</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>330000.0</td>
      <td>395910000.0</td>
      <td>-95158000.0</td>
      <td>-2.532091e+09</td>
      <td>-2256000.0</td>
      <td>-1.074806e+09</td>
      <td>-100000000.0</td>
      <td>-9.405240e+08</td>
      <td>136538000.0</td>
      <td>-2.666373e+09</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.462566</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>190000.0</td>
      <td>126293000.0</td>
      <td>-96047000.0</td>
      <td>3.258590e+08</td>
      <td>-2225000.0</td>
      <td>-6.331680e+08</td>
      <td>700000000.0</td>
      <td>-6.780710e+08</td>
      <td>-42678000.0</td>
      <td>3.707620e+08</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>401180000.0</td>
      <td>-82567000.0</td>
      <td>2.587989e+09</td>
      <td>-2219000.0</td>
      <td>-1.538270e+08</td>
      <td>725000000.0</td>
      <td>-1.845860e+08</td>
      <td>-28540000.0</td>
      <td>2.618748e+09</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1429 columns</p>
</div>




```python
#Basic plotting
indicator = 'Population, total'
data[indicator].plot(grid=True, figsize=(14,8), title=indicator, lw = 4, marker='.', markersize=20)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f918be63710>




![_config.yml]({{ site.baseurl}}/images/20200212_output_19_1.png)



```python
indicators = ['Population, total','Population, male','Population, female']
data[indicators].plot(grid=True, figsize=(14,8), title=indicator, lw = 4, marker='.', markersize=16)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f919085b950>




![_config.yml]({{ site.baseurl}}/images/20200212_output_20_1.png)



```python
data[indicators].plot(grid=True, figsize=(16,8), kind='bar')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f918bfa1e90>




![_config.yml]({{ site.baseurl}}/images/20200212_output_21_1.png)

-mkc
