## oracle

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

