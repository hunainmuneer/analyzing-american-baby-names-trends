# American Baby Names Analysis

[](baby_names.jpeg)

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

### `categories` table

| Column | Definition | Data Type |
|-|-|-|  
|category_code| Code for the category of the business |`varchar`|
|category| Description of the business category |`varchar`|


