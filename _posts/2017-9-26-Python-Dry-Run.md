
Source: http://cs231n.github.io/python-numpy-tutorial/  


```python
!python --version
```

    Python 3.5.3 :: Continuum Analytics, Inc.
    


```python
print("Hello World!")
```

    Hello World!
    


```python
def add2(x,y):
    return (x+y)
```


```python
print(add2(2,10))
```

    12
    


```python
x = 12
print(type(x))
```

    <class 'int'>
    


```python
print(x)
```

    12
    


```python
print(x + 1)
```

    13
    


```python
print(x - 1)
```

    11
    


```python
print(x * 2)
```

    24
    


```python
print(x ** 2)
```

    144
    


```python
x += 1
print(x)
```

    13
    


```python
x *= 2
print(x)
```

    26
    


```python
y = 2.5
print(type(y))
```

    <class 'float'>
    


```python
print(y, y+1, y*2, y ** 2)
```

    2.5 3.5 5.0 6.25
    


```python
x++
```


      File "<ipython-input-19-77f4531f2da2>", line 1
        x++
           ^
    SyntaxError: invalid syntax
    


## Booleans:


```python
T = True
F = False
print(type(T))
```

    <class 'bool'>
    


```python
print(T and F)
```

    False
    


```python
print(T or F)
```

    True
    


```python
print(not T)
```

    False
    


```python
print(T != F)   # XOR
```

    True
    

## Strings:


```python
hello = 'hello'
world = 'world'
print(hello)
```

    hello
    


```python
print(len(hello))
```

    5
    


```python
hw = hello + ' ' + world
print(hw)
```

    hello world
    


```python
hw12 = '%s %s %d' % (hello, world, 12)
print(hw12)
```

    hello world 12
    


```python
s = "hello"
print(s.capitalize())
```

    Hello
    


```python
print(s.upper())
```

    HELLO
    


```python
print(s.lower())
```

    hello
    

## Lists


```python
mylist = [3, 1, 2]  # Note: this is a list
print(list)
```

    [3, 1, 'foo']
    


```python
print(mylist[0])
```

    3
    


```python
print(mylist[-1]) # Negative indices count from the end of the list
```

    2
    


```python
mylist[2] = 'foo'     # Lists can contain elements of different types
print(mylist)
```

    [3, 1, 'foo']
    


```python
mylist.append('CNN')
print(mylist)
```

    [3, 1, 'foo', 'CNN']
    


```python
print(mylist.pop())
```

    CNN
    


```python
print(mylist)
```

    [3, 1, 'foo']
    

## Slicing


```python
seq = list(range(5))
print(seq)
```

    [0, 1, 2, 3, 4]
    


```python
print(seq[2:4])   # Get a slice from index 2 to 4 (exclusive)
```

    [2, 3]
    


```python
print(seq[2:])
```

    [2, 3, 4]
    


```python
print(seq[:2])
```

    [0, 1]
    


```python
print(seq[:])
```

    [0, 1, 2, 3, 4]
    


```python
print(seq[:-1])
```

    [0, 1, 2, 3]
    


```python
seq[2:4] = [8, 9]
print(seq)
```

    [0, 1, 8, 9, 4]
    

## Loops


```python
foods = ['liempo', 'adobo', 'kare-kare']
for food in foods:
    print(food)
```

    liempo
    adobo
    kare-kare
    


```python
foods = ['liempo', 'adobo', 'kare-kare']
for i, food in enumerate(foods):
    print('%d. %s' % (i + 1, food))
```

    1. liempo
    2. adobo
    3. kare-kare
    

## List Comprehension


```python
nums = list(range(10))
squares = []
for x in nums:
    squares.append(x ** 2)
print(squares)
```

    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    


```python
cubes = [y ** 3 for y in nums]
print(cubes)
```

    [0, 1, 8, 27, 64, 125, 216, 343, 512, 729]
    


```python
nums = list(range(15))
even_squares = [x ** 2 for x in nums if x % 2 == 0]
print(even_squares)
```

    [0, 4, 16, 36, 64, 100, 144, 196]
    

## Dictionaries


```python
d = {'cat': 'cute', 'dog': 'friend'}
print(d['cat'])
```

    cute
    


```python
print(d['dog'])
```

    friend
    


```python
print('dog' in d)  # Check if a dictionary has a given key
```

    True
    


```python
d['fish'] = 'wet'
print(d)
```

    {'dog': 'friend', 'fish': 'wet', 'cat': 'cute'}
    


```python
print(d.get('monkey', 'N/A'))
```

    N/A
    


```python
print(d)
```

    {'dog': 'friend', 'fish': 'wet', 'cat': 'cute'}
    


```python
print(d.get('fish', 'N/A'))
```

    wet
    


```python
del d['fish']         # Remove an element from a dictionary
print(d)
```

    {'dog': 'friend', 'cat': 'cute'}
    


```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal in d:
    legs = d[animal]
    print('A %s has %d legs' % (animal, legs))
```

    A spider has 8 legs
    A person has 2 legs
    A cat has 4 legs
    


```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.items():
    print('A %s has %d legs' % (animal, legs))
```

    A spider has 8 legs
    A person has 2 legs
    A cat has 4 legs
    

## Sets


```python
animals = {'cat', 'dog'}
print('cat' in animals)
```

    True
    


```python
print('fish' in animals)
```

    False
    


```python
animals.add('fish')
print(animals)
```

    {'dog', 'fish', 'cat'}
    


```python
print(len(animals))
```

    3
    


```python
animals.remove('cat')
print(animals)
```

    {'dog', 'fish'}
    


```python
animals = {'cat', 'dog', 'fish'}
for i, animal in enumerate(animals):
    print('%d. %s' % (i + 1, animal))
```

    1. dog
    2. fish
    3. cat
    


```python
from math import sqrt
nums = {sqrt(x) for x in range(30)}
print(nums)
```

    {0.0, 1.0, 2.0, 2.23606797749979, 1.7320508075688772, 1.4142135623730951, 2.449489742783178, 2.6457513110645907, 2.8284271247461903, 3.0, 3.1622776601683795, 3.3166247903554, 3.4641016151377544, 4.0, 5.0, 4.47213595499958, 4.795831523312719, 4.898979485566356, 5.0990195135927845, 5.196152422706632, 5.291502622129181, 3.7416573867739413, 3.872983346207417, 4.123105625617661, 4.242640687119285, 4.358898943540674, 4.58257569495584, 4.69041575982343, 5.385164807134504, 3.605551275463989}
    

## Tuples


```python
d = {(x, x + 1): x for x in range(10)}
print(d)
```

    {(0, 1): 0, (1, 2): 1, (5, 6): 5, (2, 3): 2, (4, 5): 4, (6, 7): 6, (8, 9): 8, (9, 10): 9, (3, 4): 3, (7, 8): 7}
    


```python
t = (5, 6)
print(type(t))
```

    <class 'tuple'>
    


```python
print(d[t])
```

    5
    


```python
print(d[(1, 2)])
```

    1
    

## Sample Function


```python
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'
```


```python
print(sign(-1))
```

    negative
    


```python
for x in [-1, 0, 1]:
    print(sign(x))
```

    negative
    zero
    positive
    


```python
def hello(name, loud=False):
    if loud:
        print('HELLO, %s!' % name.upper())
    else:
        print('Hello, %s!' % name)
```


```python
hello('Juan')
```

    Hello, Juan!
    


```python
hello('Juan', loud=True)
```

    HELLO, JUAN!
    


```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)
```


```python
print(quicksort([3,6,8,10,1,2,4,7,9,5]))
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

## Classes


```python
class Greeter(object):
    
    # Constructor
    def __init__(self, name):
        self.name = name
        
    # Instance method
    def greet(self, loud=False):
        if loud:
            print('HELLO, %s!' % self.name.upper())
        else:
            print('Hello, %s!' % self.name)            
```


```python
g = Greeter('Melvin')
g.greet()
```

    Hello, Melvin!
    


```python
g.greet(loud=True)
```

    HELLO, MELVIN!
    

## NumPy


```python
import numpy as np
a = np.array([1, 2, 3])   # Create a rank 1 array
print(type(a))
```

    <class 'numpy.ndarray'>
    


```python
print(a.shape)
```

    (3,)
    


```python
print(a)
```

    [1 2 3]
    


```python
b = np.array([[1,2,3],[4,5,6]])
print(b)
```

    [[1 2 3]
     [4 5 6]]
    


```python
b[1,2] = 60
print(b)
```

    [[ 1  2  3]
     [ 4  5 60]]
    


```python
a = np.zeros((3,3))
print(a)
```

    [[ 0.  0.  0.]
     [ 0.  0.  0.]
     [ 0.  0.  0.]]
    


```python
b = np.ones((1,3))
print(b)
```

    [[ 1.  1.  1.]]
    


```python
c = np.full((10,10),8)
print(c)
```

    [[8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]
     [8 8 8 8 8 8 8 8 8 8]]
    


```python
d = np.eye(5)
print(d)
```

    [[ 1.  0.  0.  0.  0.]
     [ 0.  1.  0.  0.  0.]
     [ 0.  0.  1.  0.  0.]
     [ 0.  0.  0.  1.  0.]
     [ 0.  0.  0.  0.  1.]]
    


```python
e = np.random.random((3,3))
print(e)
```

    [[ 0.08245566  0.23666261  0.1054603 ]
     [ 0.71728521  0.81500598  0.65669296]
     [ 0.68904601  0.16632103  0.90048958]]
    

## Indexing


```python
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
print(a)
```

    [[ 1  2  3  4]
     [ 5  6  7  8]
     [ 9 10 11 12]]
    


```python
b = a[:2,1:3]
print(b)
```

    [[2 3]
     [6 7]]
    


```python
row_r1 = a[1, :]
print(row_r1)
```

    [5 6 7 8]
    


```python
row_r2 = a[1:2, :]
print(row_r2)
```

    [[5 6 7 8]]
    


```python
print(row_r1, row_r1.shape)
```

    [5 6 7 8] (4,)
    


```python
print(row_r2, row_r2.shape)
```

    [[5 6 7 8]] (1, 4)
    


```python
col_r1 = a[:, 1]
print(col_r1)
```

    [ 2  6 10]
    


```python
col_r2 = a[:, 1:2]
print(col_r2, col_r2.shape)
```

    [[ 2]
     [ 6]
     [10]] (3, 1)
    


```python
a = np.array([[1,2], [3, 4], [5, 6]])
print(a)
```

    [[1 2]
     [3 4]
     [5 6]]
    


```python
print(a[[0, 1, 2], [0, 1, 1]])
```

    [1 4 6]
    


```python
print(np.array([a[0, 0], a[1, 1], a[2, 1]]))
```

    [1 4 6]
    


```python
print(a[[0, 0], [1, 1]])
```

    [2 2]
    


```python
print(np.array([a[0, 1], a[0, 1]]))
```

    [2 2]
    


```python
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])

print(a)
```

    [[ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]
     [10 11 12]]
    


```python
b = np.array([0, 2, 0, 1])
print(b)
```

    [0 2 0 1]
    


```python
print(a[np.arange(4), b])
```

    [ 1  6  7 11]
    


```python
a[np.arange(4), b] += 10
print(a)
```

    [[11  2  3]
     [ 4  5 16]
     [17  8  9]
     [10 21 12]]
    

## Boolean Indexing


```python
a = np.array([[1,2], [3, 4], [5, 6]])
```


```python
bool_idx = (a > 2)
print(bool_idx)
```

    [[False False]
     [ True  True]
     [ True  True]]
    


```python
print(a[bool_idx])
```

    [3 4 5 6]
    


```python
print(a[a > 2])
```

    [3 4 5 6]
    

## Datatypes


```python
x = np.array([1, 2])
print(x.dtype)
```

    int32
    


```python
x = np.array([1.0, 2.0])
print(x.dtype)
```

    float64
    


```python
x = np.array([1, 2], dtype=np.int64)
print(x.dtype)
```

    int64
    

## Array math


```python
x = np.array([[1,2],[3,4]], dtype=np.float64)
print(x)
```

    [[ 1.  2.]
     [ 3.  4.]]
    


```python
y = np.array([[5,6],[7,8]], dtype=np.float64)
print(y)
```

    [[ 5.  6.]
     [ 7.  8.]]
    


```python
print(x + y)
```

    [[  6.   8.]
     [ 10.  12.]]
    


```python
print(np.add(x,y))
```

    [[  6.   8.]
     [ 10.  12.]]
    


```python
print(x - y)
```

    [[-4. -4.]
     [-4. -4.]]
    


```python
print(np.subtract(x,y))
```

    [[-4. -4.]
     [-4. -4.]]
    

Element-wise multiplication


```python
print(x * y)
```

    [[  5.  12.]
     [ 21.  32.]]
    


```python
print(np.multiply(x, y))
```

    [[  5.  12.]
     [ 21.  32.]]
    


```python
print(x / y)
```

    [[ 0.2         0.33333333]
     [ 0.42857143  0.5       ]]
    


```python
print(np.divide(x, y))
```

    [[ 0.2         0.33333333]
     [ 0.42857143  0.5       ]]
    


```python
print(np.sqrt(x))
```

    [[ 1.          1.41421356]
     [ 1.73205081  2.        ]]
    


```python
x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])

v = np.array([9,10])
w = np.array([11, 12])
```


```python
print(v.dot(w))
```

    219
    


```python
print(np.dot(v, w))
```

    219
    


```python
print(x.dot(v))
```

    [29 67]
    


```python
print(np.dot(x, v))
```

    [29 67]
    


```python
print(x.dot(y))
```

    [[19 22]
     [43 50]]
    


```python
print(np.dot(x, y))
```

    [[19 22]
     [43 50]]
    


```python
x = np.array([[1,2], [3,4]])
print(x)
```

    [[1 2]
     [3 4]]
    


```python
print(x.T)
```

    [[1 3]
     [2 4]]
    


```python
v = np.array([1,2,3])
print(v)
```

    [1 2 3]
    


```python
print(v.T)
```

    [1 2 3]
    

## Broadcasting


```python
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
print(x)
```

    [[ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]
     [10 11 12]]
    


```python
v = np.array([1, 0, 1])
y = np.empty_like(x)   # Create an empty matrix with the same shape as x
```


```python
for i in range(4):
    y[i, :] = x[i, :] + v
print(y)
```

    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
vv = np.tile(v, (4, 1))
print(vv)
```

    [[1 0 1]
     [1 0 1]
     [1 0 1]
     [1 0 1]]
    


```python
y = x + vv
print(y)
```

    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = x + v
print(y)
```

    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
v = np.array([1,2,3])  # v has shape (3,)
w = np.array([4,5])    # w has shape (2,)

print(np.reshape(v, (3, 1)) * w)
```

    [[ 4  5]
     [ 8 10]
     [12 15]]
    


```python
x = np.array([[1,2,3], [4,5,6]])

print(x + v)
```

    [[2 4 6]
     [5 7 9]]
    


```python
print((x.T + w).T)
```

    [[ 5  6  7]
     [ 9 10 11]]
    


```python
print(x + np.reshape(w, (2, 1)))
```

    [[ 5  6  7]
     [ 9 10 11]]
    


```python
print(x * 2)
```

    [[ 2  4  6]
     [ 8 10 12]]
    
-mkc