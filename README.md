# Pandas1.0



Pandas1.0新特性  

1.新增pd.NA
- pd.NA可以表示整型、布尔型和1.0中新加入的字符型的“空”
- pd.NA支持逻辑与比较运算:
  
~~~python
In [7]: pd.NA > 1
Out[7]: <NA>
In [8]: pd.NA | True
Out[8]: True
~~~
- np.nan在浮点型与object类型中表示“空”
- None只能在object类型中表示“空”
- pd.NaT在时间类型中表示“空”

2.新增String类型

~~~python
In [10]: s = pd.Series(['abc', None, 'def'], dtype="string")

In [11]: s
Out[11]: 
0     abc
1    <NA>
2     def
Length: 3, dtype: string
~~~

3.convert_dtypes()方法
- 可以将Series或者DataFrame中传统的object型、浮点型和bool转化为String、整型和boolean型，在read_csv()和read_excel()读取数据后，可以根据需要将类型一致且是String、整型和布尔型的进行快速转换

4.ingore_index
- sort_values()、sort_index()新增ingore_index参数,当传入值为True时，可以解决下面index值因排序导致的不是从上到下依次递增的问题
- drop_duplicates()也添加了这个参数，解决了因为删除导致的index值顺序的问题
- 不过在我们的业务代码中，通常都是以次要维度、细分维度等作为index的，我们不需要让他强制再按照从小到大的顺序重新排列，所以基本不会影响到


~~~ python
In [17]: df = pd.DataFrame({'A': [1, 2, 3]})                                                                                                                                        

In [18]: df                                                                                                                                                                         
Out[18]: 
   A
0  1
1  2
2  3

In [19]: df.sort_values('A', ascending=False)                                                                                                                                       
Out[19]: 
   A
2  3
1  2
0  1
~~~

5.DataFrame.rename()
- 不再接受df.rename({0: 1}, {0: 2})这种模棱两可的传参方式，但可以只传第一个位置参数，按照默认axis=0进行更改index的名称，或者可以指定axis=1
- 建议只要是调用三方库的，都把位置参数的参数名写上，这样便于后期维护

6.DataFrame.info()
- 更加详细的展示每一行的信息


~~~python
In [34]: df = pd.DataFrame({"int_col": [1, 2, 3],
   ....:                    "text_col": ["a", "b", "c"],
   ....:                    "float_col": [0.0, 0.1, 0.2]})
   ....: 

In [35]: df.info(verbose=True)
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 3 columns):
 #   Column     Non-Null Count  Dtype  
---  ------     --------------  -----  
 0   int_col    3 non-null      int64  
 1   text_col   3 non-null      object 
 2   float_col  3 non-null      float64
dtypes: float64(1), int64(1), object(1)
memory usage: 200.0+ bytes
~~~

7.剔除sort_index()中的by参数
- 如果继续向使用by参数，官方更加推荐使用sort_values()
- sort_index()的level参数替代了原先的by的功能

8.DataFrame.groupby()
- 如果传入的是一个元组，则视为一个键,比如("a",)和"a"就是两种不同的键
- 报表中的group参数，就需要更改

8.read_excel()
- usecols参数不再允许传入一个整型值

9.DataFrame.apply()
- 剔除了reduce和broadcast参数
- 新增了result_type参数，其中包含了被剔除的功能

10.concat()
- 将sort参数默认值从None改为False

11.replace()
- 支持df.replace([0, 1, 2, 3], [4, 3, 2, 1])这种映射关系以两个列表表示

12.groupby()
- 当列和索引名称相同，进行 groupby，旧版本默认对 columns 进行 groupby，升级之后进行 groupby需要指定：axis{0 or ‘index’, 1 or ‘columns’}, default 0
- 前提条件 a 索引里面无重复的值。
~~~ python
In [6]: df1 = df.set_index("a", drop=False)

In [7]: df1
Out[7]:
   a  b  c
a
1  1  5  1
2  2  6  1
3  3  7  1
4  4  8  1

In [8]: df1.groupby(["a"], axis=1)

~~~
13.DataFrame()
- 低版本不支持
~~~ python
In [3]: a = {"a":1, "b":2, "c":3}                                                                                                                                                                                                                                                                                                                                          

In [4]: pd.DataFrame(a.items(), columns=['key', "value"])
~~~

想了解更多？点我
