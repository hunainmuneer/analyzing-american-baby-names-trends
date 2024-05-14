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
- Following are the techniques used in this project: ranking, grouping, joining, ordering, and pattern matching skills 

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
- The above SQL result is a preview. You can view the complete result at [here]()
    
  
  



