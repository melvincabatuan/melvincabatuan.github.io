
Walkthrough of datacamp tutorial of : (https://www.datacamp.com/community/tutorials/deep-learning-python)

## Load the data


```python
import pandas as pd

# Read in white wine data 
white = pd.read_csv("http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv", sep=';')

# Read in red wine data 
red = pd.read_csv("http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv", sep=';')
```


```python
white.head()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7.0</td>
      <td>0.27</td>
      <td>0.36</td>
      <td>20.7</td>
      <td>0.045</td>
      <td>45.0</td>
      <td>170.0</td>
      <td>1.0010</td>
      <td>3.00</td>
      <td>0.45</td>
      <td>8.8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.3</td>
      <td>0.30</td>
      <td>0.34</td>
      <td>1.6</td>
      <td>0.049</td>
      <td>14.0</td>
      <td>132.0</td>
      <td>0.9940</td>
      <td>3.30</td>
      <td>0.49</td>
      <td>9.5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.1</td>
      <td>0.28</td>
      <td>0.40</td>
      <td>6.9</td>
      <td>0.050</td>
      <td>30.0</td>
      <td>97.0</td>
      <td>0.9951</td>
      <td>3.26</td>
      <td>0.44</td>
      <td>10.1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7.2</td>
      <td>0.23</td>
      <td>0.32</td>
      <td>8.5</td>
      <td>0.058</td>
      <td>47.0</td>
      <td>186.0</td>
      <td>0.9956</td>
      <td>3.19</td>
      <td>0.40</td>
      <td>9.9</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.2</td>
      <td>0.23</td>
      <td>0.32</td>
      <td>8.5</td>
      <td>0.058</td>
      <td>47.0</td>
      <td>186.0</td>
      <td>0.9956</td>
      <td>3.19</td>
      <td>0.40</td>
      <td>9.9</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
red.head()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7.8</td>
      <td>0.88</td>
      <td>0.00</td>
      <td>2.6</td>
      <td>0.098</td>
      <td>25.0</td>
      <td>67.0</td>
      <td>0.9968</td>
      <td>3.20</td>
      <td>0.68</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.8</td>
      <td>0.76</td>
      <td>0.04</td>
      <td>2.3</td>
      <td>0.092</td>
      <td>15.0</td>
      <td>54.0</td>
      <td>0.9970</td>
      <td>3.26</td>
      <td>0.65</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11.2</td>
      <td>0.28</td>
      <td>0.56</td>
      <td>1.9</td>
      <td>0.075</td>
      <td>17.0</td>
      <td>60.0</td>
      <td>0.9980</td>
      <td>3.16</td>
      <td>0.58</td>
      <td>9.8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



## Describe and Familiarize your Data


```python
white.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4898 entries, 0 to 4897
    Data columns (total 12 columns):
    fixed acidity           4898 non-null float64
    volatile acidity        4898 non-null float64
    citric acid             4898 non-null float64
    residual sugar          4898 non-null float64
    chlorides               4898 non-null float64
    free sulfur dioxide     4898 non-null float64
    total sulfur dioxide    4898 non-null float64
    density                 4898 non-null float64
    pH                      4898 non-null float64
    sulphates               4898 non-null float64
    alcohol                 4898 non-null float64
    quality                 4898 non-null int64
    dtypes: float64(11), int64(1)
    memory usage: 459.3 KB



```python
red.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1599 entries, 0 to 1598
    Data columns (total 12 columns):
    fixed acidity           1599 non-null float64
    volatile acidity        1599 non-null float64
    citric acid             1599 non-null float64
    residual sugar          1599 non-null float64
    chlorides               1599 non-null float64
    free sulfur dioxide     1599 non-null float64
    total sulfur dioxide    1599 non-null float64
    density                 1599 non-null float64
    pH                      1599 non-null float64
    sulphates               1599 non-null float64
    alcohol                 1599 non-null float64
    quality                 1599 non-null int64
    dtypes: float64(11), int64(1)
    memory usage: 150.0 KB



```python
white.sample()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017</th>
      <td>9.0</td>
      <td>0.55</td>
      <td>0.3</td>
      <td>8.1</td>
      <td>0.026</td>
      <td>14.0</td>
      <td>71.0</td>
      <td>0.993</td>
      <td>2.94</td>
      <td>0.36</td>
      <td>11.8</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
red.sample()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1146</th>
      <td>7.8</td>
      <td>0.5</td>
      <td>0.12</td>
      <td>1.8</td>
      <td>0.178</td>
      <td>6.0</td>
      <td>21.0</td>
      <td>0.996</td>
      <td>3.28</td>
      <td>0.87</td>
      <td>9.8</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
white.describe()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
      <td>4898.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>6.854788</td>
      <td>0.278241</td>
      <td>0.334192</td>
      <td>6.391415</td>
      <td>0.045772</td>
      <td>35.308085</td>
      <td>138.360657</td>
      <td>0.994027</td>
      <td>3.188267</td>
      <td>0.489847</td>
      <td>10.514267</td>
      <td>5.877909</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.843868</td>
      <td>0.100795</td>
      <td>0.121020</td>
      <td>5.072058</td>
      <td>0.021848</td>
      <td>17.007137</td>
      <td>42.498065</td>
      <td>0.002991</td>
      <td>0.151001</td>
      <td>0.114126</td>
      <td>1.230621</td>
      <td>0.885639</td>
    </tr>
    <tr>
      <th>min</th>
      <td>3.800000</td>
      <td>0.080000</td>
      <td>0.000000</td>
      <td>0.600000</td>
      <td>0.009000</td>
      <td>2.000000</td>
      <td>9.000000</td>
      <td>0.987110</td>
      <td>2.720000</td>
      <td>0.220000</td>
      <td>8.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>6.300000</td>
      <td>0.210000</td>
      <td>0.270000</td>
      <td>1.700000</td>
      <td>0.036000</td>
      <td>23.000000</td>
      <td>108.000000</td>
      <td>0.991723</td>
      <td>3.090000</td>
      <td>0.410000</td>
      <td>9.500000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>6.800000</td>
      <td>0.260000</td>
      <td>0.320000</td>
      <td>5.200000</td>
      <td>0.043000</td>
      <td>34.000000</td>
      <td>134.000000</td>
      <td>0.993740</td>
      <td>3.180000</td>
      <td>0.470000</td>
      <td>10.400000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7.300000</td>
      <td>0.320000</td>
      <td>0.390000</td>
      <td>9.900000</td>
      <td>0.050000</td>
      <td>46.000000</td>
      <td>167.000000</td>
      <td>0.996100</td>
      <td>3.280000</td>
      <td>0.550000</td>
      <td>11.400000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>14.200000</td>
      <td>1.100000</td>
      <td>1.660000</td>
      <td>65.800000</td>
      <td>0.346000</td>
      <td>289.000000</td>
      <td>440.000000</td>
      <td>1.038980</td>
      <td>3.820000</td>
      <td>1.080000</td>
      <td>14.200000</td>
      <td>9.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
red.describe()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
      <td>1599.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8.319637</td>
      <td>0.527821</td>
      <td>0.270976</td>
      <td>2.538806</td>
      <td>0.087467</td>
      <td>15.874922</td>
      <td>46.467792</td>
      <td>0.996747</td>
      <td>3.311113</td>
      <td>0.658149</td>
      <td>10.422983</td>
      <td>5.636023</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.741096</td>
      <td>0.179060</td>
      <td>0.194801</td>
      <td>1.409928</td>
      <td>0.047065</td>
      <td>10.460157</td>
      <td>32.895324</td>
      <td>0.001887</td>
      <td>0.154386</td>
      <td>0.169507</td>
      <td>1.065668</td>
      <td>0.807569</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.600000</td>
      <td>0.120000</td>
      <td>0.000000</td>
      <td>0.900000</td>
      <td>0.012000</td>
      <td>1.000000</td>
      <td>6.000000</td>
      <td>0.990070</td>
      <td>2.740000</td>
      <td>0.330000</td>
      <td>8.400000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>7.100000</td>
      <td>0.390000</td>
      <td>0.090000</td>
      <td>1.900000</td>
      <td>0.070000</td>
      <td>7.000000</td>
      <td>22.000000</td>
      <td>0.995600</td>
      <td>3.210000</td>
      <td>0.550000</td>
      <td>9.500000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>7.900000</td>
      <td>0.520000</td>
      <td>0.260000</td>
      <td>2.200000</td>
      <td>0.079000</td>
      <td>14.000000</td>
      <td>38.000000</td>
      <td>0.996750</td>
      <td>3.310000</td>
      <td>0.620000</td>
      <td>10.200000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>9.200000</td>
      <td>0.640000</td>
      <td>0.420000</td>
      <td>2.600000</td>
      <td>0.090000</td>
      <td>21.000000</td>
      <td>62.000000</td>
      <td>0.997835</td>
      <td>3.400000</td>
      <td>0.730000</td>
      <td>11.100000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>15.900000</td>
      <td>1.580000</td>
      <td>1.000000</td>
      <td>15.500000</td>
      <td>0.611000</td>
      <td>72.000000</td>
      <td>289.000000</td>
      <td>1.003690</td>
      <td>4.010000</td>
      <td>2.000000</td>
      <td>14.900000</td>
      <td>8.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
plt.rcParams["figure.figsize"] = (20,10)
import seaborn as sns

sns.set_style("whitegrid")
sns.boxplot(data = white) 
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fa90017c5f8>




```python
sns.boxplot(data = red) 
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fa8ffc03f98>




![_config.yml]({{ site.baseurl}}/images/20191101_output_14_1.png)


## Preprocess Data


```python
# Add `type` column to `red` with value 1
red['type'] = 1

# Add `type` column to `white` with value 0
white['type'] = 0

# Append `white` to `red`
wines = red.append(white, ignore_index=True)
```


```python
wines.sample()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1182</th>
      <td>10.2</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>2.5</td>
      <td>0.068</td>
      <td>41.0</td>
      <td>54.0</td>
      <td>0.99754</td>
      <td>3.38</td>
      <td>0.86</td>
      <td>10.5</td>
      <td>6</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
import seaborn as sns
corr = wines.corr()
sns.heatmap(corr, 
            xticklabels=corr.columns.values,
            yticklabels=corr.columns.values)
plt.show()
```


![_config.yml]({{ site.baseurl}}/images/20191101_output_18_0.png)


# Split Train and Test


```python
from sklearn.model_selection import train_test_split
import numpy as np

# Specify the data 
X=wines.iloc[:,0:-1]  # 12 dimensions

# Specify the target labels and flatten the array 
y=np.ravel(wines.type)

# Split the data up in train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
```


```python
X_train.head()
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1700</th>
      <td>7.1</td>
      <td>0.12</td>
      <td>0.32</td>
      <td>9.6</td>
      <td>0.054</td>
      <td>64.0</td>
      <td>162.0</td>
      <td>0.99620</td>
      <td>3.40</td>
      <td>0.41</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5199</th>
      <td>6.8</td>
      <td>0.12</td>
      <td>0.30</td>
      <td>12.9</td>
      <td>0.049</td>
      <td>32.0</td>
      <td>88.0</td>
      <td>0.99654</td>
      <td>3.20</td>
      <td>0.35</td>
      <td>9.9</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3340</th>
      <td>7.7</td>
      <td>0.38</td>
      <td>0.40</td>
      <td>2.0</td>
      <td>0.038</td>
      <td>28.0</td>
      <td>152.0</td>
      <td>0.99060</td>
      <td>3.18</td>
      <td>0.32</td>
      <td>12.9</td>
      <td>6</td>
    </tr>
    <tr>
      <th>86</th>
      <td>8.6</td>
      <td>0.49</td>
      <td>0.28</td>
      <td>1.9</td>
      <td>0.110</td>
      <td>20.0</td>
      <td>136.0</td>
      <td>0.99720</td>
      <td>2.93</td>
      <td>1.95</td>
      <td>9.9</td>
      <td>6</td>
    </tr>
    <tr>
      <th>5587</th>
      <td>6.1</td>
      <td>0.20</td>
      <td>0.17</td>
      <td>1.6</td>
      <td>0.048</td>
      <td>46.0</td>
      <td>129.0</td>
      <td>0.99100</td>
      <td>3.30</td>
      <td>0.43</td>
      <td>11.4</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
X_train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4352 entries, 1700 to 860
    Data columns (total 12 columns):
    fixed acidity           4352 non-null float64
    volatile acidity        4352 non-null float64
    citric acid             4352 non-null float64
    residual sugar          4352 non-null float64
    chlorides               4352 non-null float64
    free sulfur dioxide     4352 non-null float64
    total sulfur dioxide    4352 non-null float64
    density                 4352 non-null float64
    pH                      4352 non-null float64
    sulphates               4352 non-null float64
    alcohol                 4352 non-null float64
    quality                 4352 non-null int64
    dtypes: float64(11), int64(1)
    memory usage: 442.0 KB



```python
X_test.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2145 entries, 3103 to 2683
    Data columns (total 12 columns):
    fixed acidity           2145 non-null float64
    volatile acidity        2145 non-null float64
    citric acid             2145 non-null float64
    residual sugar          2145 non-null float64
    chlorides               2145 non-null float64
    free sulfur dioxide     2145 non-null float64
    total sulfur dioxide    2145 non-null float64
    density                 2145 non-null float64
    pH                      2145 non-null float64
    sulphates               2145 non-null float64
    alcohol                 2145 non-null float64
    quality                 2145 non-null int64
    dtypes: float64(11), int64(1)
    memory usage: 217.9 KB


## Standardize The Data


```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler().fit(X_train)

X_train = scaler.transform(X_train)  # scaler should be fitted to train only not the entire set

X_test = scaler.transform(X_test)
```


```python
sns.boxplot(data = X_train) 
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fa8fedd4d68>




![_config.yml]({{ site.baseurl}}/images/20191101_output_26_1.png)



```python
sns.boxplot(data = X_test)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fa8fe4a4940>




![_config.yml]({{ site.baseurl}}/images/20191101_output_27_1.png)


## MLP Data Model


```python
import tensorflow as tf

from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

# Initialize the constructor
model = Sequential()

# Add an input layer 
model.add(Dense(12, activation='relu', input_shape=(12,)))

# Add one hidden layer 
model.add(Dense(8, activation='relu'))

# Add an output layer 
model.add(Dense(1, activation='sigmoid'))
```


```python
X_train.shape
```




    (4352, 12)




```python
model.input_shape
```




    (None, 12)




```python
model.output_shape
```




    (None, 1)




```python
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    dense (Dense)                (None, 12)                156       
    _________________________________________________________________
    dense_1 (Dense)              (None, 8)                 104       
    _________________________________________________________________
    dense_2 (Dense)              (None, 1)                 9         
    =================================================================
    Total params: 269
    Trainable params: 269
    Non-trainable params: 0
    _________________________________________________________________



```python
model.get_config()
```




    {'name': 'sequential',
     'layers': [{'class_name': 'Dense',
       'config': {'name': 'dense',
        'trainable': True,
        'batch_input_shape': (None, 12),
        'dtype': 'float32',
        'units': 12,
        'activation': 'relu',
        'use_bias': True,
        'kernel_initializer': {'class_name': 'GlorotUniform',
         'config': {'seed': None}},
        'bias_initializer': {'class_name': 'Zeros', 'config': {}},
        'kernel_regularizer': None,
        'bias_regularizer': None,
        'activity_regularizer': None,
        'kernel_constraint': None,
        'bias_constraint': None}},
      {'class_name': 'Dense',
       'config': {'name': 'dense_1',
        'trainable': True,
        'dtype': 'float32',
        'units': 8,
        'activation': 'relu',
        'use_bias': True,
        'kernel_initializer': {'class_name': 'GlorotUniform',
         'config': {'seed': None}},
        'bias_initializer': {'class_name': 'Zeros', 'config': {}},
        'kernel_regularizer': None,
        'bias_regularizer': None,
        'activity_regularizer': None,
        'kernel_constraint': None,
        'bias_constraint': None}},
      {'class_name': 'Dense',
       'config': {'name': 'dense_2',
        'trainable': True,
        'dtype': 'float32',
        'units': 1,
        'activation': 'sigmoid',
        'use_bias': True,
        'kernel_initializer': {'class_name': 'GlorotUniform',
         'config': {'seed': None}},
        'bias_initializer': {'class_name': 'Zeros', 'config': {}},
        'kernel_regularizer': None,
        'bias_regularizer': None,
        'activity_regularizer': None,
        'kernel_constraint': None,
        'bias_constraint': None}}]}




```python
model.get_weights()
```




    [array([[ 0.18509841, -0.1787318 , -0.1974895 ,  0.47024655, -0.22014785,
             -0.0336175 , -0.4499972 ,  0.00948608, -0.17663336, -0.13032675,
             -0.20205772,  0.48160243],
            [-0.01900232,  0.01159549,  0.37995243, -0.19818723, -0.10916328,
              0.14702225,  0.03217804, -0.25475132,  0.03949857,  0.42871344,
              0.0091548 , -0.28606224],
            [-0.22698724, -0.00181675,  0.28305113, -0.42118   , -0.4363984 ,
              0.396356  ,  0.34379554, -0.07786012, -0.20018017, -0.22760534,
             -0.41003287, -0.16024351],
            [-0.21674359,  0.13974214,  0.44481695,  0.03168631, -0.41572845,
              0.05836797, -0.40699494, -0.21628869,  0.4499054 , -0.29570246,
             -0.13334882,  0.1724205 ],
            [-0.16441262,  0.2967329 ,  0.33999276, -0.3159808 , -0.22747147,
             -0.24628317,  0.1189853 , -0.00377738,  0.4081782 , -0.08154869,
             -0.404258  , -0.3623103 ],
            [-0.26187372,  0.21067333, -0.10333037,  0.4204389 , -0.08828092,
             -0.44783163, -0.0311656 ,  0.1310252 , -0.21938753,  0.48668897,
             -0.21560037, -0.02168429],
            [-0.08842039, -0.09222972, -0.34199488,  0.0538379 , -0.11738276,
             -0.24480498,  0.2634858 , -0.16824949, -0.24320757, -0.06541574,
             -0.1200999 , -0.14858925],
            [ 0.32152343, -0.07561326, -0.42344034,  0.20426881, -0.46129727,
             -0.451545  , -0.07423818,  0.35979104, -0.45212865,  0.05693948,
              0.4395585 ,  0.02052164],
            [-0.37374985,  0.46638262,  0.40280735, -0.3428874 , -0.2306912 ,
              0.435421  ,  0.07876062,  0.36400735, -0.2985438 , -0.2733376 ,
             -0.08336091,  0.2731043 ],
            [-0.45887685, -0.42721224, -0.15539205,  0.15447009,  0.01825047,
             -0.20035589, -0.41170645,  0.28550863,  0.08846998, -0.4694103 ,
             -0.48388457, -0.35172224],
            [ 0.34384847,  0.31992245,  0.26032948,  0.31604683,  0.2847165 ,
             -0.07904387, -0.47391832, -0.48427117, -0.40004635,  0.4950372 ,
              0.1061877 , -0.4887694 ],
            [-0.07969391,  0.39160895, -0.19645143, -0.14542246, -0.49597108,
              0.4747815 , -0.11582911,  0.33142388,  0.16304743, -0.10827124,
              0.2970295 , -0.13576949]], dtype=float32),
     array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.], dtype=float32),
     array([[-0.12573722, -0.48548222,  0.4242099 , -0.38060397,  0.16979319,
              0.0918628 ,  0.03741366,  0.3321216 ],
            [-0.01776743,  0.19143659, -0.31154966, -0.30466095,  0.48119617,
             -0.1385485 ,  0.00675309,  0.17020297],
            [-0.40346807, -0.3330045 ,  0.41555822,  0.5237893 ,  0.35933822,
              0.345599  , -0.29075652, -0.34157613],
            [-0.48182094,  0.11174613, -0.31294814,  0.04721171, -0.33019376,
              0.22745019, -0.3051402 , -0.47627673],
            [-0.47520423, -0.35167938, -0.253953  ,  0.25971293, -0.23213515,
             -0.33326215,  0.4418798 ,  0.36893743],
            [ 0.19385517,  0.27362376,  0.51028967, -0.3290528 , -0.25164944,
              0.00835991,  0.3839218 ,  0.09181195],
            [-0.49004847,  0.48177254, -0.19130912, -0.48774895,  0.5439503 ,
             -0.5147951 ,  0.14143002, -0.06817371],
            [ 0.34650046,  0.40215737, -0.45702255, -0.21625286, -0.3195373 ,
             -0.13152903,  0.00269675,  0.1602022 ],
            [-0.0268448 ,  0.23023522, -0.28147897,  0.03006667,  0.32084173,
              0.2816946 , -0.34124613, -0.42064995],
            [ 0.37168366,  0.18144089,  0.30210078,  0.05433446, -0.15761724,
             -0.42090005,  0.2879694 , -0.42921987],
            [-0.41087604, -0.01969856, -0.2089597 , -0.35769606,  0.31698042,
             -0.21026021, -0.13029772, -0.5287101 ],
            [-0.4796678 ,  0.29415208, -0.32441646, -0.2662846 , -0.12991512,
             -0.39028126, -0.06847224,  0.25392455]], dtype=float32),
     array([0., 0., 0., 0., 0., 0., 0., 0.], dtype=float32),
     array([[ 0.43139648],
            [ 0.812376  ],
            [-0.18619502],
            [-0.5815563 ],
            [-0.2090925 ],
            [-0.11155289],
            [-0.7855999 ],
            [ 0.01194578]], dtype=float32),
     array([0.], dtype=float32)]



## Training


```python
model.compile(loss='binary_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])
                   
history = model.fit(X_train, y_train,epochs=20, batch_size=1, validation_data=(X_test, y_test), verbose=1)
```

    Train on 4352 samples, validate on 2145 samples
    Epoch 1/20
    4352/4352 [==============================] - 19s 4ms/sample - loss: 0.0883 - accuracy: 0.9754 - val_loss: 0.0444 - val_accuracy: 0.9874
    Epoch 2/20
    4352/4352 [==============================] - 17s 4ms/sample - loss: 0.0241 - accuracy: 0.9947 - val_loss: 0.0325 - val_accuracy: 0.9944
    Epoch 3/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0203 - accuracy: 0.9963 - val_loss: 0.0263 - val_accuracy: 0.9949
    Epoch 4/20
    4352/4352 [==============================] - 17s 4ms/sample - loss: 0.0165 - accuracy: 0.9968 - val_loss: 0.0253 - val_accuracy: 0.9958
    Epoch 5/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0147 - accuracy: 0.9975 - val_loss: 0.0223 - val_accuracy: 0.9958
    Epoch 6/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0132 - accuracy: 0.9970 - val_loss: 0.0253 - val_accuracy: 0.9958
    Epoch 7/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0137 - accuracy: 0.9972 - val_loss: 0.0229 - val_accuracy: 0.9963
    Epoch 8/20
    4352/4352 [==============================] - 17s 4ms/sample - loss: 0.0110 - accuracy: 0.9982 - val_loss: 0.0223 - val_accuracy: 0.9958
    Epoch 9/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0108 - accuracy: 0.9977 - val_loss: 0.0230 - val_accuracy: 0.9953
    Epoch 10/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0098 - accuracy: 0.9982 - val_loss: 0.0232 - val_accuracy: 0.9958
    Epoch 11/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0085 - accuracy: 0.9979 - val_loss: 0.0372 - val_accuracy: 0.9921
    Epoch 12/20
    4352/4352 [==============================] - 17s 4ms/sample - loss: 0.0100 - accuracy: 0.9977 - val_loss: 0.0276 - val_accuracy: 0.9949
    Epoch 13/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0105 - accuracy: 0.9977 - val_loss: 0.0284 - val_accuracy: 0.9949
    Epoch 14/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0081 - accuracy: 0.9984 - val_loss: 0.0246 - val_accuracy: 0.9958
    Epoch 15/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0075 - accuracy: 0.9979 - val_loss: 0.0257 - val_accuracy: 0.9953
    Epoch 16/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0097 - accuracy: 0.9975 - val_loss: 0.0229 - val_accuracy: 0.9953
    Epoch 17/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0072 - accuracy: 0.9982 - val_loss: 0.0272 - val_accuracy: 0.9953
    Epoch 18/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0074 - accuracy: 0.9986 - val_loss: 0.0263 - val_accuracy: 0.9958
    Epoch 19/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0079 - accuracy: 0.9982 - val_loss: 0.0283 - val_accuracy: 0.9953
    Epoch 20/20
    4352/4352 [==============================] - 18s 4ms/sample - loss: 0.0067 - accuracy: 0.9986 - val_loss: 0.0279 - val_accuracy: 0.9949



```python
print(history.history.keys())
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
```

    dict_keys(['loss', 'accuracy', 'val_loss', 'val_accuracy'])



![_config.yml]({{ site.baseurl}}/images/output_44_0\ (2).png)


## Evaluate Model


```python
score = model.evaluate(X_test, y_test,verbose=1)
print(score)
```

    2145/1 [==============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================] - 0s 111us/sample - loss: 0.0139 - accuracy: 0.9949



```python
preds = model.predict(X_test)
print(preds)
```


```python
preds = np.where(preds > 0.5, 1, 0)
print(preds)
```


```python
def plot_confusion_matrix(cm,
                          target_names,
                          title='Confusion matrix',
                          cmap=None,
                          normalize=True):
    """
    given a sklearn confusion matrix (cm), make a nice plot
cmap = matplotlib.cm.get_cmap('Reds')

    Arguments
    ---------
    cm:           confusion matrix from sklearn.metrics.confusion_matrix

    target_names: given classification classes such as [0, 1, 2]
                  the class names, for example: ['high', 'medium', 'low']

    title:        the text to display at the top of the matrix

    cmap:         the gradient of the values displayed from matplotlib.pyplot.cm
                  see http://matplotlib.org/examples/color/colormaps_reference.html
                  plt.get_cmap('jet') or plt.cm.Blues

    normalize:    If False, plot the raw numbers
                  If True, plot the proportions

    Usage
    -----
    plot_confusion_matrix(cm           = cm,                  # confusion matrix created by
                                                              # sklearn.metrics.confusion_matrix
                          normalize    = True,                # show proportions
                          target_names = y_labels_vals,       # list of names of the classes
                          title        = best_estimator_name) # title of graph

    Citiation
    ---------
    http://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html

    """
    import itertools

    accuracy = np.trace(cm) / float(np.sum(cm))
    misclass = 1 - accuracy

    if cmap is None:
        cmap = plt.get_cmap('Blues')

    plt.figure(figsize=(8, 8))
    ax = plt.imshow(cm, interpolation='nearest', cmap=cmap)
    plt.title(title)
    plt.colorbar()

    if target_names is not None:
        tick_marks = np.arange(len(target_names))
        plt.xticks(tick_marks, target_names, rotation=45)
        plt.yticks(tick_marks, target_names)

    if normalize:
        cm = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]


    thresh = cm.max() / 1.5 if normalize else cm.max() / 2
    for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):
        if normalize:
            plt.text(j, i, "{:0.4f}".format(cm[i, j]),
                     horizontalalignment="center",
                     verticalalignment="center",
                     color="white" if cm[i, j] > thresh else "black")
        else:
            plt.text(j, i, "{:,}".format(cm[i, j]),
                     horizontalalignment="center",
                     verticalalignment="center",
                     color="white" if cm[i, j] > thresh else "black")


    plt.tight_layout()
    plt.ylabel('True label')
    plt.xlabel('Predicted label\naccuracy={:0.4f}; misclass={:0.4f}'.format(accuracy, misclass))
    plt.show()
```


```python
from sklearn.metrics import confusion_matrix

title = 'Confusion matrix: '  
sns.set(style='ticks')
target_names = ['white','red']  # white = 0, red = 1
cm =confusion_matrix(y_test, preds)
plot_confusion_matrix(cm, target_names, title=title, cmap=None, normalize=False)
```


![_config.yml]({{ site.baseurl}}/images/20191101_output_44_0\ (1).png)



```python

```
