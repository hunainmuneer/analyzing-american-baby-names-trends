# American Baby Names Analysis

![](baby_names.jpeg)

Photo by Travis Wise on [Wikimedia](https://commons.wikimedia.org/wiki/File:Hello_My_Name_Is_(15283079263).jpg)

# Introduction

How have American baby name tastes changed since 1920? Which names have remained popular for over 100 years, and how do those names compare to more recent top baby names? These are considerations for many new parents, but the skills we'll practice while answering these queries are broadly applicable. After all, understanding trends and popularity is important for many businesses, too!

## Description

- What makes a name timeless or trendy? In this project, we will use data published by the U.S. Social Security Administration spanning over a hundred years to understand American baby name tastes.
- The dataset used in this analysis is provided by the United States Social Security Administration. It includes first names along with the number and sex of babies they were given to each year.
- The dataset spans 101 years, from 1920 through 2020, and has been limited to first names given to over 5,000 American babies in a given year for processing speed purposes.
- The csv file that contains the data can be found [here](usa_baby_names.csv)

## Methodology

- SQL is utilized for this project to perform Data Manipulation and Exploratory Data Analysis.
- Following are the techniques used in this project: window functions, common table expression (cte), sub queries, ranking, grouping, joining, ordering, and pattern matching skills 

## Requirements

To run the SQL queries, you will need access to a PostgreSQL database containing the dataset provided.

## Data

### `baby_names` table

| Column | Definition | Data Type |
|-|-|-|  
|year| year |`int`|
|first_name| first name |`varchar`|
|sex| sex of babies given first_name | `varchar` |
|num| number of babies of sex given first_name in that year | `int` |

## Case Study Questions & Solutions

**1. Classic American names:**
```sql
SELECT first_name, SUM(num)
FROM baby_names
GROUP BY first_name
HAVING COUNT(year) = 101
ORDER BY 2 DESC 
```
Answer:
  
| first_name | sum |
|-|-|
|James|	4748138 |
|John|	4510721 |
|William|	3614424 |
|David| 3571498 |
|Joseph| 2361382 |
|Thomas| 2166802 |
|Charles | 2112352 |
|Elizabeth | 1436286 |

- The SQL query retrieves the `first_name` and calculates the sum of that names.
- This query orders the result with the most popular names at top.
- Then it filter for those names that appear in all 101 years.

**2. Timeless or trendy?**
```sql
SELECT first_name, sum(num), 
    CASE WHEN count(first_name) > 80 THEN 'Classic'
         WHEN count(first_name) BETWEEN 50 AND 80 THEN 'Semi-classic'
         WHEN count(first_name) BETWEEN 20 AND 50 THEN 'Semi-trendy'
         WHEN count(first_name) BETWEEN 0 AND 20 THEN 'Trendy'
    END AS popularity_type
FROM baby_names
GROUP BY first_name
ORDER BY first_name
```
Answer:

| first_name | sum | popularity_type |
|-|-|-|
|Aaliyah|	15870 | Trendy |
|Aaron|	530592 | Semi-classic |
|Abigail|	338485 | Semi-trendy |
|Adam|	497293 | Semi-trendy |
|Addison|	107433 | Trendy |

- The SQL query performs Classification for each name's popularity.
- The names are classified into the four categories: 'Classic', 'Semi-classic', 'Semi-trendy', or 'Trendy'
- If the name appears in the dataset for more than 80 years -> 'Classic'  
- If the name appears in the dataset for between 50 and 80 years -> 'Semi-classic'
- If the name appears in the dataset for between 20 and 50 years -> 'Semi-trendy'
- If the name appears in the dataset for less than 20 years -> 'Trendy'
- The above SQL result is a preview. You can view the complete result at [here](notebook.ipynb)

**3. Top-ranked female names since 1920**  
```sql
SELECT first_name,
SUM(num),
RANK() OVER(ORDER BY SUM(num) DESC) AS name_rank
FROM baby_names
WHERE sex = 'F'
GROUP BY first_name
LIMIT 10
```
Answer:

| first_name | sum | name_rank |
|-|-|-|
|Mary| 3215850 | 1 |
|Patricia| 1479802 | 2 |
|Elizabeth|	1436286 |	3 |
|Jennifer| 1404743 |	4 |
|Linda|	1361021 |	5 |
|Barbara|	1343901 |	6 |
|Susan|	1025728 |	7 |
|Jessica|	994210 | 8 |
|Lisa| 920119 |	9 |
|Betty|	893396 | 10 |


- The SQL query utilizes the window function (rank) to rank the top female baby names.
- "Mary" is the most popular name among the female babies eventhough it did not appear in our first query. ( The first query was only for the names that were given in all of the 101 years in our dataset)
- The query is limiting the results to only top 10 names.

**4. Picking a baby name**
```sql
SELECT first_name
FROM baby_names
WHERE sex = 'F' AND year > 2015 AND first_name LIKE '%a'
GROUP BY first_name
ORDER BY sum(num) DESC
```
Answer:

| first_name |
|-|
| Olivia |
| Emma |
| Ava |
| Sophia |
| Isabella |
| Mia |
| Amelia |
| Ella |
| Sofia |
  
- The SQL query retrives the most popular female baby names that ends with a vowel 'a' in their name.
- The query filters the result for the recent popular names that were given after the year 2015.
- The result shows the names in descending order, with the most popular name at the top.
- "Olivia" was the top name amongst female baby names since 2015.
- The above SQL result is a preview. You can view the complete result at [here](notebook.ipynb)

**5. The Olivia expansion**
```sql
SELECT year, first_name, num,
SUM(num) OVER(ORDER BY year) AS cumulative_olivias
FROM baby_names
WHERE first_name = 'Olivia'
ORDER BY year
```
Answer:

| year | first_name | num	| cumulative_olivias |
|-|-|-|-|
|1991|	Olivia | 5601 | 5601 |
|1992|	Olivia | 5809	| 11410 |
|1993|	Olivia | 6340	| 17750 |
|1994|	Olivia | 6434 | 24184 |
|1995|	Olivia | 7624 | 31808 |
  
- The SQL query dives into more depth, analyzing the rise of the name "Olivia". 
- The result shows that the name was first given in 1991 and to 5601 babies.
- The window function is utilized to see the trend of the name "Olivia" over the years by using cummulative sum. 
- The above SQL result is a preview. You can view the complete result at [here](notebook.ipynb)

**6. Many males with the same name**
```sql
SELECT year, MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year
```
Answer: 

| year | max_num |
|-|-|
|1970| 85291 |
|2000| 34483 |
|1947| 94764 |
|1962| 85041 |
|1975| 68451 |

-  The SQL query retrives the maximum number for male baby names each year.
-  This SQL query will be used as a subquery in the next question where we will utilize it to view the most popular male baby name each year.
- The above SQL result is a preview. You can view the complete result at [here](notebook.ipynb)

**7. Top male names over the years**
```sql
SELECT b.year, b.first_name, b.num
FROM baby_names AS b
INNER JOIN (
SELECT year, MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year
) AS subquery
ON b.year = subquery.year AND b.num = subquery.max_num
ORDER BY year DESC
```
Answer: 

| year | first_name |	num |
|-|-|-|
|2020| Liam | 19659 |
|2019| Liam |	20555 |
|2018| Liam |	19924 |
|2017| Liam | 18824 |
|2016| Noah	| 19154 |
|2015| Noah	| 19650 |
|2014| Noah	| 19319 |
|2013| Noah | 18266 |
|2012| Jacob | 19088 |

- The SQL query uses the subquery from the pervious step to perform a join.
- The SQL query shows the results for the most popular male baby name each year.
- The results also shows the number of babies who were named that name each year in descending order.
- The above SQL result is a preview. You can view the complete result at [here](notebook.ipynb)

**8. The most years at number one**
```sql
WITH cte AS(
SELECT b.year, b.first_name, b.num
FROM baby_names AS b
INNER JOIN (
SELECT year, MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year
) AS subquery
ON b.year = subquery.year AND b.num = subquery.max_num
ORDER BY year DESC)

SELECT first_name, COUNT(year) AS count_top_name
FROM cte
GROUP BY first_name
ORDER BY COUNT(year) DESC
```
Answer: 

| first_name | count_top_name |
|-|-|
|Michael| 44|
|Robert| 17 |
|Jacob| 14 |
|James| 13 |
|Noah| 4 |
|John| 4 |
|Liam| 4 |
|David|	1 |

- The SQL query builds on the previous query where we retrieved the top names in each year, using the pervious query as a Common Table Expression (CTE).
- This SQL query retrieves the number of times a male baby name has been the top name each year.
- The result shows that "Micheal" has been the most popuplar name amongst male baby names, taking the first position in a year, 44 times.

## Key Findinga:

- There are 8 names that were given each year since 1920 till 2020. Following are the names starting with the most popular name.
  1. James	
  2. John	
  3. William	
  4. David	
  5. Joseph	
  6. Thomas	
  7. Charles	
  8. Elizabeth
- Classification of the names based on how many years were they given to at least one baby.
- "Mary" was the most popular name given to a female baby since 1920. Approximately 3.2 million female babies were given the name, Mary.
- "Olivia" was the most popular female baby name that ends with a vowel 'a'.
- The name "Olivia" was first given in year 1991 and had a meteoric rise!
- "Micheal" was the most popular name amongst male baby, which topped the list 44 times. 
  

  ## The End 
