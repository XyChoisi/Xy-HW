# 用 python 做计算器，做数学题

## 高数
求导
```
>>> from sympy import *
>>> x = symbols('x')

>>> diff (x*x + sin(x)*cos(x)) 
2*x - sin(x)**2 + cos(x)**2

```

极限
```
>>> from sympy import *
>>> x = symbol('x') 

>>> limit(1/x**2, x, 0) 
oo 
```


## 线代
矩阵求逆
```
>>>from numpy import * 
>>>A1 = mat(eye(2,2)*0.5)
>>> A1
matrix([[ 0.5,  0. ],
        [ 0. ,  0.5]])
>>>A2=A1.I  
>>> A2
matrix([[ 2.,  0.],
        [ 0.,  2.]])
```

矩阵转置
```
>>>from numpy import * 
>>> a1=mat([[1,1],[0,0]])
>>> a1
matrix([[1, 1],
        [0, 0]])
>>> a2=a1.T
>>> a2
matrix([[1, 0],
        [1, 0]])
```
