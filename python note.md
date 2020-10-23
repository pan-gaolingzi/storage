### eg 1 eval

input:

The first line contains an integer, , denoting the number of commands.
Each line of the subsequent lines contains one of the commands described above.

```
12
insert 0 5
insert 1 10
insert 0 6
print
remove 6
append 9
append 1
sort
print
pop
reverse
print
```

output:

```
[6, 5, 10]
[1, 5, 9, 10]
[9, 5, 1]
```



```python
if __name__ == '__main__':
    N = int(input())
    li = []
    for i in range(N):
        cmds = input().split()
        if len(cmds) == 3:
            eval('li.'+ cmds[0]+ '('+ cmds[1]+ ','+ cmds[2]+')')
        if len(cmds) == 2:
            eval('li.'+ cmds[0]+ '('+ cmds[1]+ ')')
        elif len(cmds) == 1:
            if cmds[0] == 'print':
                eval(cmds[0]+'(li)')
            else:
                eval('li.'+ cmds[0]+ '('+ ')')
```

**Input Format**

The first line contains the space separated values of and .
The second line contains the polynomial .

**Output Format**

Print `True` if . Otherwise, print `False`.

**Sample Input**

```
1 4
x**3 + x**2 + x + 1
```

**Sample Output**

```
True
```

```
x, y = input().split()
x = int(x)
# y = int(y)
express = input()

print(eval(express + '==' + y))
```

 `eval()` can also be used to work with Python keywords or defined functions and variables. These would normally be stored as strings.

For example:

```
>>> type(eval("len"))
<type 'builtin_function_or_method'>
```

Without eval()

```
>>> type("len")
<type 'str'>
```



### e.g 2 rangoli

```
--------e--------
------e-d-e------
----e-d-c-d-e----
--e-d-c-b-c-d-e--
e-d-c-b-a-b-c-d-e
--e-d-c-b-c-d-e--
----e-d-c-d-e----
------e-d-e------
--------e--------
```

注意：空列表不能直接用下標賦值，會出index out of range錯誤.

```python
def print_rangoli(n):
    # your code goes here
    if n == 0:
        return
    max_chr = 96 + n
    l = []
    for i in range(0, n):
        l1 = []
        for j in range(n):          
            if max_chr - i + j > max_chr:
                l1.insert(2 * j, '-')
            else:
                l1.insert(2 * j, chr(max_chr - i + j))
            if 2 * j + 1 < 2 * n -2:
                l1.insert(2 * j + 1, '-')
        l2 = l1[1:].copy()
        l2.reverse()
        l2.extend(l1)
        line = ''.join(l2)
        l.insert(i, line)
        print(line)
    for k in range(n-2, -1, -1):
        print(l[k])


if __name__ == '__main__':
    n = int(input())
    print_rangoli(n)
```

### e.g. 3 minion game

Kevin and Stuart want to play the '**The Minion Game**'.

**Game Rules**

Both players are given the same string, .
Both players have to make substrings using the letters of the string .
Stuart has to make words starting with *consonants*.
Kevin has to make words starting with *vowels*.
The game ends when both players have made all possible substrings.

**Scoring**
A player gets `+1` point for each occurrence of the substring in the string .

**For Example**:
String = *BANANA*
Kevin's vowel beginning word = *ANA*
Here, *ANA* occurs twice in *BANANA*. Hence, Kevin will get `2` Points.

For better understanding, see the image below:

![banana.png](https://s3.amazonaws.com/hr-challenge-images/9693/1450330231-04db904008-banana.png)

Your task is to determine the winner of the game and their score.

**Input Format**

A single line of input containing the string .
**Note**: The string will contain only uppercase letters: .

**Constraints**



**Output Format**

Print one line: the name of the winner and their score separated by a space.

If the game is a draw, print *Draw*.

**Sample Input**

```
BANANA
```

**Sample Output**

```
Stuart 12
```

```
# 錯誤示例，直接把緩存跑崩了
def minion_game(string):
    # your code goes here
    len_str = len(string)
    l_stuart = []
    l_kevin = []
    for i in range(1, len_str+1):
        for j in range(len_str-i+1):
            sub_str = string[j:j+i]
            if sub_str[0] in ('A', 'E', 'I', 'O', 'U'):
                l_kevin.append(sub_str)
            else:
                l_stuart.append(sub_str)
    if len(l_stuart) > len(l_kevin):
        print('Stuart', len(l_stuart))
    elif len(l_stuart) < len(l_kevin):
        print('Kevin', len(l_kevin))
    else:
        print('Draw')


if __name__ == '__main__':
    s = input()
    minion_game(s)
    
# 正確方法
s = raw_input()

vowels = 'AEIOU'

kevsc = 0
stusc = 0
for i in range(len(s)):
    if s[i] in vowels:
        kevsc += (len(s)-i)
    else:
        stusc += (len(s)-i)

if kevsc > stusc:
    print "Kevin", kevsc
elif kevsc < stusc:
    print "Stuart", stusc
else:
    print "Draw"
```

#### petite note

“修改”字符串：

1. 轉list
2. 目標位置前後slice，然後拼接 string[:position] + change + string[position+1:]

### e.g. 4 any

判斷是否含任意字母

```
any(c.isalpha() for c in string_1)      # 返回布爾值
```

### e.g 5 Text Alignment -- print logo

```python
thickness = int(input()) #This must be an odd number
c = 'H'

#Top Cone
for i in range(thickness):
    print((c*i).rjust(thickness-1)+c+(c*i).ljust(thickness-1))

#Top Pillars
for i in range(thickness+1):
    print((c*thickness).center(thickness*2)+(c*thickness).center(thickness*6))

#Middle Belt
for i in range((thickness+1)//2):
    print((c*thickness*5).center(thickness*6))    

#Bottom Pillars
for i in range(thickness+1):
    print((c*thickness).center(thickness*2)+(c*thickness).center(thickness*6))    

#Bottom Cone
for i in range(thickness):
    print(((c*(thickness-i-1)).rjust(thickness)+c+(c*(thickness-i-1)).ljust(thickness)).rjust(thickness*6))
```

example output:

```
    H    
   HHH   
  HHHHH  
 HHHHHHH 
HHHHHHHHH
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHHHHHHHHHHHHHHHHHHHHHH   
  HHHHHHHHHHHHHHHHHHHHHHHHH   
  HHHHHHHHHHHHHHHHHHHHHHHHH   
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
  HHHHH               HHHHH             
                    HHHHHHHHH 
                     HHHHHHH  
                      HHHHH   
                       HHH    
                        H 
```

### e.g. 6 text wrap

```python
import textwrap

def wrap(string, max_width):
    t = textwrap.fill(string, max_width)
    return t

if __name__ == '__main__':
    string, max_width = input(), int(input())
    result = wrap(string, max_width)
    print(result)
```

### e.g. 7 分組去重

Consider the following:

- A string, , of length where .
- An integer, , where is a factor of .

We can split into subsegments where each subsegment, , consists of a contiguous block of characters in . Then, use each to create string such that:

- The characters in are a subsequence of the characters in .
- Any repeat occurrence of a character is removed from the string such that each character in occurs exactly once. In other words, if the character at some index in occurs at a previous index in , then do not include the character in string .

Given and , print lines where each line denotes string .

**Input Format**

The first line contains a single string denoting .
The second line contains an integer, , denoting the length of each subsegment.

**Constraints**

- , where is the length of 
- 
- It is guaranteed that is a multiple of .

**Output Format**

Print lines where each line contains string .

**Sample Input**

```
AABCAAADA
3   
```

**Sample Output**

```
AB
CA
AD
```

**Explanation**

String is split into equal parts of length . We convert each to by removing any subsequent occurrences non-distinct characters in :

1. 
2. 
3. 

We then print each on a new line.

```python
def merge_the_tools(string, k):
    # your code goes here
    n = len(string)
    num_seg = n//k
    for i in range(num_seg):
        part_no_repeat = string[i*k:i*k + k]
        res_part = ''
        for c in list(part_no_repeat):
            if c not in res_part:
                res_part += c
        print(res_part)

if __name__ == '__main__':
    string, k = input(), int(input())
    merge_the_tools(string, k)
   
# advanced
S, N = input(), int(input()) 
for part in zip(*[iter(S)] * N):
    d = dict()
    print(''.join([ d.setdefault(c, c) for c in part if c not in d ]))
```

### e.g. 8 itertools.product()

```python
from itertools import product


str_a = input().split()
lst_a = []
for i in str_a:
    lst_a.append(int(i))
str_b = input().split()
lst_b = []
for i in str_b:
    lst_b.append(int(i))

for item in list(product(lst_a, lst_b)):
    print(item, end=" ")
```

### e.g. 9 排列 permutation

```python
# Enter your code here. Read input from STDIN. Print output to STDOUT
from itertools import permutations

string, n = input().split()
n = int(n)

l_per = list(permutations(string, n))
l_per.sort()

for serie in l_per:
    print(''.join(serie))


```



### e.g. 10 組合 combination

```python
from itertools import combinations


string, k = input().split()
k = int(k)
str_sorted = sorted(string, key=str.upper)

for i in range(1, k + 1):
    l_combi = list(combinations(str_sorted, i))
    for item in l_combi:
        print(''.join(item))
```

### e.g. 11 collections counter

**Task**

 is a shoe shop owner. His shop has number of shoes.
He has a list containing the size of each shoe he has in his shop.
There are number of customers who are willing to pay amount of money only if they get the shoe of their desired size.

Your task is to compute how much money earned.

**Input Format**

The first line contains , the number of shoes.
The second line contains the space separated list of all the shoe sizes in the shop.
The third line contains , the number of customers.
The next lines contain the space separated values of the desired by the customer and , the price of the shoe.

**Constraints**





**Output Format**

Print the amount of money earned by .

**Sample Input**

```
10
2 3 4 5 6 8 7 6 5 18
6
6 55
6 45
6 55
4 40
18 60
10 50
```

**Sample Output**

```
200
```

**Explanation**

**Customer 1**: Purchased size 6 shoe for **$55**.
**Customer 2**: Purchased size 6 shoe for **$45**.
**Customer 3**: Size 6 no longer available, so no purchase.
**Customer 4**: Purchased size 4 shoe for **$40**.
**Customer 5**: Purchased size 18 shoe for **$60**.
**Customer 6**: Size 10 not available, so no purchase.

Total money earned = 55 + 45 + 40 + 60 = 200

```
from collections import Counter


num_shoe = input()

l_size = input().split()
size_counter = Counter(l_size)

num_customer = int(input())
revenue = 0

for i in range(num_customer):
    # perchase.insert(tuple(map(int, input().split()))
    size, price = input().split()
    price = int(price)
    if size in l_size and size_counter[size] > 0:
        size_counter[size] -= 1
        revenue += price
print(revenue)
```

### e.g. 12 complex number

[**Polar coordinates**](https://en.wikipedia.org/wiki/Polar_coordinate_system) are an alternative way of representing Cartesian coordinates or [Complex Numbers](https://en.wikipedia.org/wiki/Complex_number).

A complex number ![Capture.PNG](http://i.picresize.com/images/2015/08/21/OUzGu.png)



is completely determined by its real part and imaginary part .
Here, is the [imaginary unit](https://en.wikipedia.org/wiki/Imaginary_unit).



A polar coordinate () ![Capture.PNG](https://s3.amazonaws.com/hr-challenge-images/9951/1440141121-5b051fd241-Capture.PNG)

is completely determined by modulus and phase angle .

If we convert complex number to its polar coordinate, we find:
: Distance from to origin, i.e., 
: Counter clockwise angle measured from the positive -axis to the line segment that joins to the origin.

Python's [cmath](https://docs.python.org/2/library/cmath.html) module provides access to the mathematical functions for complex numbers.


This tool returns the phase of complex number (also known as the argument of ).

```
>>> phase(complex(-1.0, 0.0))
3.1415926535897931
```


This tool returns the modulus (absolute value) of complex number .

```
>>> abs(complex(-1.0, 0.0))
1.0
```

**Constraints**

Given number is a valid complex number

**Output Format**

Output two lines:
The first line should contain the value of .
The second line should contain the value of .

**Sample Input**

```
  1+2j
```

**Sample Output**

```
 2.23606797749979 
 1.1071487177940904
```

```
from cmath import phase

z_str = input()
z = complex(z_str)
# part = z_str.split('+')
# x = int(part[0])
# y = int(part[1][:-1])

# print((x ** 2 + y ** 2)**0.5)
print(abs(z))
print(phase(z))
```

### e.g. 13 angle

```
import math

ab = int(input())
bc = int(input())

ac = math.sqrt(ab ** 2 + bc ** 2)
mb = ac / 2
bh = bc / 2

angle = round(math.degrees(math.acos(bh/mb)))
print('%s°' % angle)
```

### e.g.14

把字典转化为集合，只取其中的keys

```
print set({'Hacker' : 'DOSHI', 'Rank' : 616 })
set(['Hacker', 'Rank'])
```

### e.g. 15 defaultdict

The *defaultdict* tool is a container in the collections class of Python. It's similar to the usual dictionary (*dict*) container, but the only difference is that a defaultdict will have a *default* value if that key has not been set yet. If you didn't use a defaultdict you'd have to check to see if that key exists, and if it doesn't, set it to what you want.
**For example:**

```
from collections import defaultdict
d = defaultdict(list)
d['python'].append("awesome")
d['something-else'].append("not relevant")
d['python'].append("language")
for i in d.items():
    print i
```

This prints:

```
('python', ['awesome', 'language'])
('something-else', ['not relevant'])
```

In this challenge, you will be given integers, and . There are words, which might repeat, in word group . There are words belonging to word group . For each words, check whether the word has appeared in group or not. Print the indices of each occurrence of in group . If it does not appear, print .

**Constraints**



**Input Format**

The first line contains integers, and separated by a space.
The next lines contains the words belonging to group .
The next lines contains the words belonging to group .

**Output Format**

Output lines.
The line should contain the -indexed positions of the occurrences of the word separated by spaces.

**Sample Input**

```
5 2
a
a
b
a
b
a
b
```

**Sample Output**

```
1 2 4
3 5
```

**Explanation**

**'a'** appeared times in positions 1, 2 and 4.
**'b'** appeared times in positions 3 and 5.
In the sample problem, if **'c'** also appeared in word group , you would print -1.

```
from collections import defaultdict

n, m = input().split()
n = int(n)
m = int(m)
lst_n = []
lst_m = []

for i in range(n):
    lst_n.append(input())

for i in range(m):
    lst_m.append(input())

# def constant_factory(value):
#     return lambda: value

# d = defaultdict(constant_factory(-1))

# d = defaultdict(list)

# for i in range(m):
#     if lst_m[i] in lst_n:
#          for j in range(n):
            # if lst_n[j] == lst_m[i]:
#         d[lst_m[i]].append(i + 1)
#     else:
#         d[lst_m[i]] = -1


for i in range(m):
    if lst_m[i] in lst_n:
        for j in range(n):
            if lst_n[j] == lst_m[i]:
                print(j + 1, end=' ')
        print('')
    else:
        print(-1)


```

### e.g. 16 calendar

**[Calendar Module](https://docs.python.org/2/library/calendar.html#module-calendar)**

The calendar module allows you to output calendars and provides additional useful functions for them.

**[class calendar.TextCalendar([firstweekday\])](https://docs.python.org/2/library/calendar.html#calendar.TextCalendar)**

This class can be used to generate plain text calendars.

```
>>> import calendar
>>> 
>>> print calendar.TextCalendar(firstweekday=6).formatyear(2015)
                                  2015

      January                   February                   March
Su Mo Tu We Th Fr Sa      Su Mo Tu We Th Fr Sa      Su Mo Tu We Th Fr Sa
             1  2  3       1  2  3  4  5  6  7       1  2  3  4  5  6  7
 4  5  6  7  8  9 10       8  9 10 11 12 13 14       8  9 10 11 12 13 14
11 12 13 14 15 16 17      15 16 17 18 19 20 21      15 16 17 18 19 20 21
18 19 20 21 22 23 24      22 23 24 25 26 27 28      22 23 24 25 26 27 28
25 26 27 28 29 30 31                                29 30 31
...
```

###### weekday

**Sample Input**

```
08 05 2015
```

**Sample Output**

```
WEDNESDAY
```

```
import calendar

dic = {0: 'MONDAY', 1: 'TUESDAY', 2: 'WEDNESDAY', 3: 'THURSDAY', 4: 'FRIDAY', 5: 'SATURDAY', 6: 'SUNDAY'}

m, d, y = input().split()
m = int(m)
d = int(d)
y = int(y)

num_weekday = calendar.weekday(y, m, d)
print(dic.get(num_weekday))

```

### e.g. 17 re

判断一个表达式是否为正则表达式：

```
import re

n = int(input())

for i in range(n):
    rex = input()
    try:
        re.compile(rex)
        print('True')
    except Exception as e:
        print('False')
```

### e.g. 18 set intersection

**.intersection()**

The *.intersection()* operator returns the intersection of a set and the set of elements in an iterable.
Sometimes, the *&* operator is used in place of the *.intersection()* operator, but it only operates on the set of elements in *set*.
The set is immutable to the *.intersection()* operation (or *&* operation).

```
>>> s = set("Hacker")
>>> print s.intersection("Rank")
set(['a', 'k'])

>>> print s.intersection(set(['R', 'a', 'n', 'k']))
set(['a', 'k'])

>>> print s.intersection(['R', 'a', 'n', 'k'])
set(['a', 'k'])

>>> print s.intersection(enumerate(['R', 'a', 'n', 'k']))
set([])

>>> print s.intersection({"Rank":1})
set([])

>>> s & set("Rank")
set(['a', 'k'])
```

Output the total number of students who have subscriptions to **both** *English* and *French* newspapers.

```
num_en = int(input())

l_en = set(input().split(' '))

num_fr = int(input())

l_fr = list(input().split(' '))

both = l_en.intersection(l_fr)

print(len(both))
```

### e.g. 19 difference

![A-B.png](https://s3.amazonaws.com/hr-challenge-images/9420/1437904659-11e4bef847-A-B.png) **.difference()**

The tool *.difference()* returns a set with all the elements from the set that are not in an iterable.
Sometimes the `-` operator is used in place of the *.difference()* tool, but it only operates on the set of elements in *set*.
Set is immutable to the *.difference()* operation (or the `-` operation).

```
num_en = input()

set_en = set(input().split(' '))

num_fr = input()

l_fr = input().split(' ')

only_en = set_en.difference(l_fr)

print(len(only_en))
```

### e.g. 20 namedtuple

**Example**

**Code 01**

```
>>> from collections import namedtuple
>>> Point = namedtuple('Point','x,y')
>>> pt1 = Point(1,2)
>>> pt2 = Point(3,4)
>>> dot_product = ( pt1.x * pt2.x ) +( pt1.y * pt2.y )
>>> print dot_product
11
```

**Code 02**

```
>>> from collections import namedtuple
>>> Car = namedtuple('Car','Price Mileage Colour Class')
>>> xyz = Car(Price = 100000, Mileage = 30, Colour = 'Cyan', Class = 'Y')
>>> print xyz
Car(Price=100000, Mileage=30, Colour='Cyan', Class='Y')
>>> print xyz.Class
Y
```

------

**Task**

Dr. John Wesley has a spreadsheet containing a list of student's , , and .

Your task is to help Dr. Wesley calculate the average marks of the students.







**Note:
\1. Columns can be in any order. IDs, marks, class and name can be written in any order in the spreadsheet.
\2. Column names are `ID`, `MARKS`, `CLASS` and `NAME`. (The spelling and case type of these names won't change.)**

**Input Format**

The first line contains an integer , the total number of students.
The second line contains the names of the columns in any order.
The next lines contains the , , and , under their respective column names.

**Constraints**



**Output Format**

Print the average marks of the list corrected to 2 decimal places.

**Sample Input**

**TESTCASE 01**

```
5
ID         MARKS      NAME       CLASS     
1          97         Raymond    7         
2          50         Steven     4         
3          91         Adrian     9         
4          72         Stewart    5         
5          80         Peter      6   
```

**TESTCASE 02**

```
5
MARKS      CLASS      NAME       ID        
92         2          Calum      1         
82         5          Scott      2         
94         2          Jason      3         
55         8          Glenn      4         
82         2          Fergus     5
```

**Sample Output**

**TESTCASE 01**

```
78.00
```

**TESTCASE 02**

```
81.00
```

**Explanation**

**TESTCASE 01**

Average = 

*Can you solve this challenge in `4 lines of code or less`?*
**NOTE**: There is `no penalty` for solutions that are correct but have more than 4 lines.

```
# 不用namedtuple
# from collections import namedtuple

n_stu = int(input())
string = input()
col = string.split()
i_mark = col.index('MARKS')
sum_score = 0
# Student = namedtuple('Student', string)


for i in range(n_stu):
    info = input().split()
    mark = int(info[i_mark])
    sum_score += mark

print(sum_score/n_stu)

# namedtuple
import collections

n = int(input())
scol = ','.join(input().split())
Student = collections.namedtuple('Student',scol)

sum = 0
for i in range(n):
    row = input().split()
    student = Student(*row)
    sum += int(student.MARKS)

print(sum/n)
```

### e.g. 21 set.add()

Find the total number of distinct country stamps.

**Input Format**

The first line contains an integer , the total number of country stamps.
The next lines contains the name of the country where the stamp is from.

**Constraints**



**Output Format**

Output the total number of distinct country stamps on a single line.

**Sample Input**

```
7
UK
China
USA
France
New Zealand
UK
France 
```

**Sample Output**

```
5
```

```
total = int(input())
s_stamp = set()

for i in range(total):
    s_stamp.add(input())

print(len(s_stamp))
```

### e.g. 22 collections.OrderedDict()

An *OrderedDict* is a dictionary that remembers the order of the keys that were inserted first. If a new entry overwrites an existing entry, the original insertion position is left unchanged.

```
>>> from collections import OrderedDict
>>> 
>>> ordinary_dictionary = {}
>>> ordinary_dictionary['a'] = 1
>>> ordinary_dictionary['b'] = 2
>>> ordinary_dictionary['c'] = 3
>>> ordinary_dictionary['d'] = 4
>>> ordinary_dictionary['e'] = 5
>>> 
>>> print ordinary_dictionary
{'a': 1, 'c': 3, 'b': 2, 'e': 5, 'd': 4}
>>> 
>>> ordered_dictionary = OrderedDict()
>>> ordered_dictionary['a'] = 1
>>> ordered_dictionary['b'] = 2
>>> ordered_dictionary['c'] = 3
>>> ordered_dictionary['d'] = 4
>>> ordered_dictionary['e'] = 5
>>> 
>>> print ordered_dictionary
OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)])
```

------

**Task**

You are the manager of a supermarket.
You have a list of items together with their prices that consumers bought on a particular day.
Your task is to print each `item_name` and `net_price` in order of its first occurrence.

`item_name` = Name of the item.
`net_price` = Quantity of the item sold multiplied by the price of each item.

**Input Format**

The first line contains the number of items, .
The next lines contains the item's name and price, separated by a space.

**Constraints**



**Output Format**

Print the `item_name` and `net_price` in order of its first occurrence.

**Sample Input**

```
9
BANANA FRIES 12
POTATO CHIPS 30
APPLE JUICE 10
CANDY 5
APPLE JUICE 10
CANDY 5
CANDY 5
CANDY 5
POTATO CHIPS 30
```

**Sample Output**

```
BANANA FRIES 12
POTATO CHIPS 60
APPLE JUICE 20
CANDY 20
```

```
import re
from collections import OrderedDict


n_item = int(input())
regex = r'(\D+)(\s+)(\d+)'
pattern = re.compile(regex)
items = OrderedDict()

for i in range(n_item):
    item = re.match(pattern, input())
    goods = item.group(1)
    price = int(item.group(3))
    if items.get(goods):
        items[goods] += price
    else:
        items[goods] = price

for key in items:
    print(key, items[key])

```

You are given words. Some words may repeat. For each word, output its number of occurrences. The output order should correspond with the input order of appearance of the word. See the sample input/output for clarification.

**Note:** Each input line ends with a **"\n"** character.

**Constraints:**

The sum of the lengths of all the words do not exceed 
All the words are composed of lowercase English letters only.

**Input Format**

The first line contains the integer, .
The next lines each contain a word.

**Output Format**

Output lines.
On the first line, output the number of distinct words from the input.
On the second line, output the number of occurrences for each distinct word according to their appearance in the input.

**Sample Input**

```
4
bcdef
abcdefg
bcde
bcdef
```

**Sample Output**

```
3
2 1 1
```

```
from collections import OrderedDict

total_word = int(input())
word_dic = OrderedDict()

for i in range(total_word):
    word = input()
    if word_dic.get(word):
        word_dic[word] += 1
    else:
        word_dic[word] = 1

print(len(word_dic))
for v in word_dic.values():
    print(v, end=' ')

```

### e.g. 23 Set .discard(), .remove() & .pop()

**.remove(x)**

This operation removes element from the set.
If element does not exist, it raises a **`KeyError`**.
The *.remove(x)* operation returns **`None`**.

**.discard(x)**

This operation also removes element from the set.
If element does not exist, it **does not** raise a `KeyError`.
The *.discard(x)* operation returns **`None`**.

**.pop()**

This operation removes and return an arbitrary 任意element from the set.
If there are no elements to remove, it raises a **`KeyError`**.

### e.g. 24 pile up

There is a horizontal row of cubes. The length of each cube is given. You need to create a new vertical pile of cubes. The new pile should follow these directions: if is on top of then .

When stacking the cubes, you can only pick up either the leftmost or the rightmost cube each time. Print *"Yes"* if it is possible to stack the cubes. Otherwise, print *"No"*. Do not print the quotation marks.

**Input Format**

The first line contains a single integer , the number of test cases.
For each test case, there are lines.
The first line of each test case contains , the number of cubes.
The second line contains space separated integers, denoting the *sideLengths* of each cube in that order.

**Output Format**

For each test case, output a single line containing either *"Yes"* or *"No"* without the quotes.

**Sample Input**

```
2
6
4 3 2 1 3 4
3
1 3 2
```

**Sample Output**

```
Yes
No
```

```python
def can_pile():
    n_case = int(input())
    for i in range(n_case):
        n_row = int(input())
        flag = 'Yes'
        l_cube = list(map(int, input().split()))

        for j in range(n_row//2):
            if (l_cube[j] < l_cube[j+1] and l_cube[-(j+1)] < l_cube[-(j+2)]):
                flag = 'No'
                break
        print(flag)

can_pile()
```

### e.g. 25 嵌套列表排序 -- operator.itemgetter

You are given a spreadsheet that contains a list of athletes and their details (such as age, height, weight and so on). You are required to sort the data based on the th attribute and print the final resulting table. Follow the example given below for better understanding.

![image](https://s3.amazonaws.com/hr-assets/0/1514874268-6fabad07aa-AthleteSort2.png)

Note that is indexed from to , where is the number of attributes.

**Note**: If two attributes are the same for different rows, for example, if two atheletes are of the same age, print the row that appeared first in the input.

**Input Format**

The first line contains and separated by a space.
The next lines each contain elements.
The last line contains .

**Constraints**



Each element 

**Output Format**

Print the lines of the sorted table. Each line should contain the space separated elements. Check the sample below for clarity.

**Sample Input 0**

```
5 3
10 2 5
7 1 0
9 9 9
1 23 12
6 5 9
1
```

**Sample Output 0**

```
7 1 0
10 2 5
6 5 9
9 9 9
1 23 12
```

```
import math
import os
import random
import re
import sys
import operator


if __name__ == '__main__':
    nm = input().split()

    n = int(nm[0])

    m = int(nm[1])

    arr = []

    for _ in range(n):
        arr.append(list(map(int, input().rstrip().split())))

    k = int(input())

    arr.sort(key=operator.itemgetter(k))

    for line in arr:
        for attribute in line:
            print(attribute, end=' ')
        print('')

```



## Django

##### ListView 

没有post方法,也没法添加post方法。换成TemplateView等。

##### PasswordChangeView 

默认修改的是当前登录的用户的密码，想实现管理其他用户的密码，可以重写以下方法

```
def get_form_kwargs(self):
    pk = self.kwargs.get('pk')
    kwargs = super().get_form_kwargs()
    kwargs['user'] = Staff.objects.filter(pk=pk).first()
    return kwargs
```

##### UpdateView

默认只更新表单META里设置的字段，重写form_valid方法不能追加更新字段。

错误案例：

```
    def form_valid(self, form):
        # form.last_update = timezone.now()
        # form.last_update_id = self.request.user.pk
        messages.success(self.request, f'スタッフの情報変更が完了しました')
        return super().form_valid(form)
```

改正方法：修改base_inital对象

```
    def get_initial(self):
        """initialize your's form values here"""

        base_initial = super().get_initial()
        # So here you're initiazing you're form's data
        base_initial['dataset_request'] = Staff.objects.filter(
            username=self.kwargs.get('pk')
        ).update(last_update=timezone.now(), last_update_id=self.request.user.pk)
        return base_initial
```

