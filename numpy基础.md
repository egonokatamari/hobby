# numpy基础

## ndarray：多维数组对象

### 创建ndarray

```python
arr = np.array([[1., 2., 3.], [4., 5., 6.]]) 
# 会自动转换合适的数据类型
arr.ndim # 维度
arr.shape # 形状
arr.dtype # 元素的数据类型
```

```python
np.zeros((3, 6)) # 创建3×6元素全为0的二维数组
np.empty((2, 3, 2)) # 创建一个没有任何具体值的三维数组
#empty返回的是未初始化的内存值，可能包含非0的垃圾值
#当你要在创建后填充数据是，才应该使用
```

```python
In[]:np.arange(15)
Out[]:array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
```

### ndarray的数据类型

```py
arr = np.array([1, 2, 3, 4, 5])
float_arr = arr.astype(np.float64) # int64转为float64类型
# numpy会将python类型映射到等价的dtype上
#astype总会创建一个新的数组，即使与旧的数组数据类型相同
```

- 注意：使用numpy.string_类型时，因为numpy的字符串数据是大小固定的，发生截断时不会发出警告。建议用pandas

### 数组运算

```python
arr = np.array([[1., 2., 3.], [4., 5., 6.]])
arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])
arr2 > arr
# 大小相同的数组之间的比较会生成布尔值数组
```

```
array([[False,  True, False],
       [ True, False,  True]])
```

### 索引和切片

```python
arr = np.arange(10)
print(arr)
arr[5:8] = 12
print(arr)
# 用一个标量赋值给一个切片时，该值会自动广播到整个切片
```

```python
[0  1  2  3  4 5  6  7   8  9]
[0  1  2  3  4 12 12 12  8  9]
```

- 与列表区别：切片时原始数组的视图。这意味着数据不会被复制，仕途上的任何修改都会直接反应到源数组上
- 若要得到副本而非视图，要使用显式赋值np.copy()
- numpy的多维索引语法不适用与python的常规对象。比如用arr[0,1]访问第一行第二列的元素

### 布尔型索引

```python
names = np.array(["Bob", "Joe", "Will", "Bob", "Will", "Joe", "Joe"])
names == "Bob"
# 想选取多个名字可以使用布尔算术运算符&(与)和|(或)
```

```python
array([ True, False, False,  True, False, False, False])
```

```python
# 布尔数组可以和切片、整数（或整数序列）混合使用
names = np.array(["Bob", "Joe", "Will", "Bob", "Will", "Joe", "Joe"])
data = np.array([[4, 7], [0, 2], [-5, 6], [0, 0], [1, 2],
                 [-12, -4], [3, 4]])
data[names == "Bob", 1:]

```

```python
array([[7],
       [0]])
```

```python
In[]:data[names == "Bob", 1]
Out[]:array([7, 0])
```

---

~可用来反转布尔型数组

```python
arr = np.array([ True, False, False,  True, False, False, False])
print(~arr)
```

```python
[False  True  True False  True  True  True]
```

---

```python
# 可通过布尔型数组来设置值
# 元素级
data[data < 0] = 0
data
```

```python
array([[4, 7],
       [0, 2],
       [0, 6],
       [0, 0],
       [1, 2],
       [0, 0],
       [3, 4]])
```

---

```python
# 轴级
data[names != "Joe"] = 7
data
```

```python
array([[7, 7],
       [0, 2],
       [7, 7],
       [7, 7],
       [7, 7],
       [0, 0],
       [3, 4]])
```

### 花式索引

```python
arr = np.arange(32).reshape((8, 4))
arr
# reshape:元素个数不变，改变形状
#花式索引：使用整数数组进行索引
```

```python
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23],
       [24, 25, 26, 27],
       [28, 29, 30, 31]])
```

```python
In[]:arr[[1, 5, 7, 2], [0, 3, 1, 2]] #第一个数组是行索引，第二个数组是列索引
Out[]:array([ 4, 23, 29, 10])
```

- 对结果赋值时，将数据复制到新数组中，不会改变原数组
- 若用原数组进行花式索引赋值，会修改索引的值

### 数组转置和轴对换

```python
#数组有个T属性，可以得到矩阵的转置
arr.T 
# 轴变换，swapaxes方法
arr.swapaxes(0,1)# 参数代表维度，0代表行，1代表列。意思是将行与第列进行标记以重排数组
# swapaxes方法返回的也是视图，修改转置数组里的元素，原数组对应的元素也会修改。
# 原数组的行列仍然不交换

# 两个矩阵的乘法运算（不是元素对影响相乘）
np.dot(arr1,arr2) # dot方法
arr1 @ arr2 # 中缀运算符
```

## 生成伪随机数

***numpy的random模块可快速生成大量样本值***

![numpy的随机数生成器方法](https://gitee.com/egonokatamari/img/raw/master/numpy的随机数生成器方法.png)

```python
# 配置生成器
In [147]: rng = np.random.default_rng(seed=12345)

In [148]: data = rng.standard_normal((2, 3))
```

## 通用函数（ufunc）：快速的元素级数组函数
