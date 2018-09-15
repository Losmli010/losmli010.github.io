---
title: 运算符和if语句以及while和for循环
date: 2018-06-18 13:14:33
tags: [表达式,if语句,循环]
categories: python
---

#### 表达式

由常量、变量和运算符组成。

```python
x = 1024			#赋值表达式		
x > 18               #比较表达式
```



#### 运算符

##### 算术运算符

```python
+[加]	-[减]	*[乘]	/[除]	%[求余]	//[取整]	**[幂]

算术运算符的优先级和数学上的相同
```



##### 赋值运算符

```python
"""
简单赋值：=
复合赋值：+=		-=		*=		/=		%=		//=		**=

复合赋值等价于先进行算术运算再将结果进行赋值
"""

num = 16
num += 2       #等价于num = num + 2
```



##### 比较运算符

```python
>[大于]	<[小于]	>=[大于等于]	<=[小于等于]	==[等于]	!=[不等于]

比较表达式判断的结果为布尔值：True或False
```



##### 逻辑运算符

```python
and[逻辑与]：全真才真，有假则假
or[逻辑或]：有真则真，全假才假
not[逻辑非]：真则假，假则真

逻辑表达式判断的结果为布尔值：True或False
```



##### 成员运算符

```python
in：判断元素是否在序列中，在其中则返回True，否则返回False
not in：和in判断结果刚好相反
```



##### 身份运算符

```python
"""
is：判断两个变量是否引用自同一个对象，是则返回True，否则返回False
is not：和is判断的结果刚好相反

注意：==用来判断两个变量存储的值是否相同，is用来判断两个变量引用的对象是否相同
"""

a = 16
b = 32
print(a == b)
print(a is b)			#等价于print(id(a) == id(b))
```



#### if语句

Python中表示判断为假的值：0、0.0、False、None、""、[]、{}。



##### if语句

要么执行，要么不执行：if后面的表达式为真，则执行，否则跳过if语句。

```python
password = input("请输入接头暗号：")
if password != "芝麻开门":
    print("biubiubiu")
```



##### if-else语句

二选一：if后面的表达式为真，则执行if下的语句，否则执行else下的语句。

```python
age = int(input("请输入你的年龄："))
if age < 18:
    print("一定要健健康康的长大哦!")
else:
    print("成年呐，要学着爱自己!")
```



##### if-elif-else语句

多选一：从上往下判断，如果在某个判断上是True，把该判断对应的语句执行后，就忽略掉剩下的elif和else。

```python
age = 24
if age >= 12:
    print("青少年")
elif age >= 18:
    print("成年人")
else:
    print("少儿")
```



#### while循环

只要while后面的条件表达式为True，就不断循环，False时退出循环。 

```python
#计算1-100所有数的和

sum = 0
num = 0
while num <= 100:
    sum += num
    num += 1
print(sum)
```



#### for循环

for...in循环，依次把序列中的每个元素遍历出来 。

```python
#计算1-100所有数的和

sum = 0
for num in range(101):
    sum += num
print(sum)
```



#### break和continue

break：跳出当前的整个循环，继续执行循环后面的代码。

continue：跳过当前次的循环，继续执行下一次循环。

```python
for num in [1, 2, 3, 4, 5]:
    if num == 3:
        break
    print(num)
    
"""
1
2
"""

for num in [1, 2, 3, 4, 5]:
    if num == 3:
        continue
    print(num)
    
"""
1
2
4
5
"""
```