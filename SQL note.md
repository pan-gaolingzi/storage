## oracle

### not in null

理由は NOT IN は、リストやサブクエリーで指定した全ての値との StudentID <> [値] が TRUE となるレコードのみを返すのですが、StudentID <> NULL は常に FALSE になるからです。

### select case

```
SELECT
    CASE
    WHEN A + B <= C OR A + C <= B OR B + C <= A THEN 'Not A Triangle'
    WHEN A = B AND B = C THEN 'Equilateral'
    WHEN A = B OR A = C OR B = C THEN 'Isosceles'
    ELSE 'Scalene'
    END AS triangle_type
FROM triangles;
```

return binary tree type:

You are given a table, *BST*, containing two columns: *N* and *P,* where *N* represents the value of a node in *Binary Tree*, and *P* is the parent of *N*.

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443818507-5095ab9853-1.png)

Write a query to find the node type of *Binary Tree* ordered by the value of the node. Output one of the following for each node:

- *Root*: If node is root node.
- *Leaf*: If node is leaf node.
- *Inner*: If node is neither root nor leaf node.

**Sample Input**

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443818467-30644673f6-2.png)

**Sample Output**

```
1 Leaf
2 Inner
3 Leaf
5 Root
6 Leaf
8 Inner
9 Leaf
```


**Explanation**

The *Binary Tree* below illustrates the sample:

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443773633-f9e6fd314e-simply_sql_bst.png)

```
SELECT N,
    CASE
    WHEN P IS Null THEN 'Root'
    WHEN N IN (SELECT B2.P FROM BST B2) THEN 'Inner'
    ELSE 'Leaf'
    END
FROM BST
ORDER BY N;
```



### concat

```
SELECT CONCAT(name, '('||SUBSTR(occupation, 1, 1)||')')
FROM occupations
ORDER BY name;

SELECT CONCAT('There are a total of ', COUNT(name)||' '||LOWER(occupation)||'s.')
FROM occupations
GROUP BY occupation
ORDER BY COUNT(name), occupation;
```

output

```
Aamina(D) 
Ashley(P) 
Belvet(P) 
Britney(P) 
Christeen(S) 
Eve(A) 
Jane(S) 
Jennifer(A) 
Jenny(S) 
Julia(D) 
Ketty(A) 
Kristeen(S) 
Maria(P) 
Meera(P) 
Naomi(P) 
Priya(D) 
Priyanka(P) 
Samantha(A) 
There are a total of 3 doctors. 
There are a total of 4 actors. 
There are a total of 4 singers. 
There are a total of 7 professors.
```

### partition



```
select name, category ,price,
       rank() over(partition by category order by price desc) ord
from goods
order by category;
```



### pivot

oracle:

### 向下取整

```
SELECT FLOOR(AVG(population))
FROM city;
```

### 数字 字符串转换 replace

原始数据和0键坏了的情况，salary平均值差异

```
SELECT CEIL(AVG(salary)-AVG(TO_NUMBER(REPLACE(TO_CHAR(salary), '0', ''))))
FROM employees;
```

### 最大值个数

We define an employee's *total earnings* to be their monthly worked, and the *maximum total earnings* to be the maximum total earnings for any employee in the **Employee** table. Write a query to find the *maximum total earnings* for all employees as well as the total number of employees who have maximum total earnings. Then print these values as space-separated integers.

**Input Format**

The **Employee** table containing employee data for a company is described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/19629/1458557872-4396838885-ScreenShot2016-03-21at4.27.13PM.png)

where *employee_id* is an employee's ID number, *name* is their name, *months* is the total number of months they've been working for the company, and *salary* is the their monthly salary.

**Sample Input**

![img](https://s3.amazonaws.com/hr-challenge-images/19631/1458559098-23bf583125-ScreenShot2016-03-21at4.32.59PM.png)

**Sample Output**

```
69952 1
```

```
SELECT earning AS max_earnings, COUNT(*)
FROM (SELECT (salary * months) AS earning
      FROM employee
      ) earnings
WHERE earning IN (SELECT MAX(earning)
                  FROM (SELECT (salary * months) AS earning
                        FROM employee)
                  )
GROUP BY earning;
```

### 保留指定位数小数

```
SELECT ROUND(SUM(lat_n), 2), ROUND(SUM(long_w), 2)
From station;
```



## MySQL

### partition by

```
FROM 
    (SELECT 
            CASE
            WHEN occupation = 'Doctor' THEN name
            END AS Doctor,
            CASE
            WHEN occupation = 'Professor' THEN name
            END AS Professor,
            CASE
            WHEN occupation = 'Singer' THEN name
            END AS Singer,
            CASE
            WHEN occupation = 'Actor' THEN name
            END AS Actor,
            ROW_NUMBER() OVER(PARTITION BY occupation ORDER BY name) rn 
     FROM occupations
     ORDER BY name) t
GROUP BY rn
ORDER BY rn;
```

