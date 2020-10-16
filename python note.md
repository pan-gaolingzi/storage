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

