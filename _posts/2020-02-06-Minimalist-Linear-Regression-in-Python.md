## Necessary Imports


```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
```

## Input Data


```python
x = np.array([1,20,40,60,80,100,120,140,180])         # INPUT X
y = np.array([6,30,80,150,180,210,230,280,320])       # TARGET OUTPUT Y
```

## Predictive Model


```python
model = LinearRegression()
```


```python
# model.fit(x,y) # ERROR: x input data needs reshape
```


```python
#model?
```

## Preprocess Data


```python
x = x.reshape(-1,1)
x
```




    array([[  1],
           [ 20],
           [ 40],
           [ 60],
           [ 80],
           [100],
           [120],
           [140],
           [180]])



## Training


```python
model.fit(x,y)   
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)



## Prediction


```python
y_pred = model.predict(x)
```


```python
plt.scatter(x,y, color='black', label='prediction: 1.83x + 14')
plt.plot(x,y_pred, color = 'green', label='target (data)')
plt.legend()
plt.xlabel('x')
plt.ylabel('y')
plt.grid("on")
```


![_config.yml]({{ site.baseurl}}/images/20200206_output_14_0.png)



```python
model.score(x,y)
```




    0.9739481928782231




```python
model.coef_
```




    array([1.83479361])




```python
model.intercept_
```




    14.046436915887767




```python
x0 = 30
predicted = model.predict(np.array([[x0]]))
predicted[0]
```




    69.0902453271027




```python
# Manual Solution using line equation:
1.83479*x0 + 14.04643
```




    69.09013




```python
plt.scatter(x,y, color='black')
plt.plot(x,y_pred, color = 'green')
plt.axvline(x=[30], color='r', linestyle='--')
plt.axhline(y=predicted, color='r', linestyle='--')
plt.annotate("(30, 69.09)",
                 xy=(30, 69.09),
                 xytext=(5, -10),
                 textcoords='offset points')
plt.grid("on")
```


![_config.yml]({{ site.baseurl}}/images/20200206_output_20_0.png)

