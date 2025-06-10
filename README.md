# WORLD DB SQL Database Queries

## Overview

This repository contains a collection of SQL queries designed for the **WORLD** database. The WORLD database models geographical information, including data about countries and cities, and is commonly used for analytical and reporting tasks. The queries here demonstrate how to extract valuable insights from this dataset.

---

## Database Schema

The queries assume a standard WORLD database schema with these key tables:

- **country**: Stores information about countries (name, life expectancy, population, surface area, GNP, continent, capital city ID).
- **city**: Contains details about cities (name, population, and a foreign key linking to their country via `CountryCode`).

---

## SQL Queries by Category

### Demographic Analysis

**Count Cities in USA**  
_Counts the total number of cities in the United States._
```
SELECT count(ci.Name)
FROM city AS ci
INNER JOIN country AS co ON ci.CountryCode = co.Code
WHERE co.name = "united states";
```

**Cities with Population Larger than 2,000,000**  
_Lists cities with populations over 2 million, ordered by population._
```
SELECT *
FROM city
WHERE population > 2000000
ORDER BY population DESC;
```

**Cities with Population Between 500,000-1,000,000**  
_Lists cities with populations between 500,000 and 1,000,000._
```
SELECT *
FROM city
WHERE population BETWEEN 500000 AND 1000000
ORDER BY population DESC;
```

**Most Populated City**  
_Retrieves details for the most populated city._
```
SELECT *
FROM city
ORDER BY population DESC
LIMIT 1;
```

**City with the Lowest Population**  
_Identifies the city with the smallest population._
```
SELECT *
FROM city
ORDER BY population ASC
LIMIT 1;
```

**Country with Largest Population**  
_Finds the country with the highest population._
```
SELECT *
FROM country
ORDER BY population DESC
LIMIT 1;
```

---

### Geographical & Location-Based Queries

**Country with Highest Life Expectancy**  
_Identifies the country with the highest life expectancy._
```
SELECT *
FROM country
ORDER BY LifeExpectancy DESC
LIMIT 1;
```

**Cities with 'New' in Their Name**  
_Lists all cities whose names contain "New"._
```
SELECT *
FROM city
WHERE name LIKE "%new%";
```

**Cities Beginning with 'Be' Prefix**  
_Lists cities starting with "Be"._
```
SELECT *
FROM city
WHERE name LIKE "be%";
```

**Capital of Spain**  
_Identifies the capital city of Spain._
```
SELECT co.name AS country_name, ci.name AS capital_city_name
FROM country AS co
INNER JOIN city AS ci ON ci.id = co.capital
WHERE co.name = "spain";
```

**Cities in Europe**  
_Lists all cities in Europe with their country and continent._
```
SELECT co.name AS country_name, ci.name AS city_name, co.continent
FROM city AS ci
INNER JOIN country AS co ON ci.countrycode = co.code
WHERE co.continent = "europe";
```

---

### Data Overview & Utility

**Display Columns with Limit (First 10 Rows)**  
_Top 10 most populous cities._
```
SELECT *
FROM city
ORDER BY population DESC
LIMIT 10;
```

**Display Columns with Limit (Rows 31-40)**  
_Cities ranked 31st to 40th by population._
```
SELECT *
FROM city
ORDER BY population DESC
LIMIT 10 OFFSET 30;
```

**Display Cities Sorted by Name in Ascending Order**  
_Alphabetically sorted list of all city names._
```
SELECT *
FROM city
ORDER BY name ASC;
```

**City Name Frequency Analysis**  
_List of unique city names, sorted alphabetically._
```
SELECT DISTINCT name
FROM city
ORDER BY name ASC;
```

---

### More Complex Query Examples

**Capital Cities Population Comparison**  
_Compare populations of capital cities, ordered by population._
```
SELECT co.name AS "country", ci.name AS "Capital City", ci.population
FROM country AS co
INNER JOIN city AS ci ON ci.id = co.capital
ORDER BY ci.population DESC;
```

**Average Population by Country**  
_Average city population for each country._
```
SELECT
    country.Name AS Country,
    ROUND(AVG(city.Population), 0) AS AverageCityPopulation
FROM city
JOIN country ON city.CountryCode = country.Code
GROUP BY country.Name
ORDER BY AverageCityPopulation DESC;
```

**Countries with Low Population Density**  
_Countries with sparse populations (surface area/population)._
```
SELECT name AS Country, (surfacearea / population) AS "Population Density"
FROM country
ORDER BY (surfacearea / population) DESC;
```

**Cities with High GDP per Capita**  
_Cities with above-average GDP per capita (country GNP / city population)._
```
SELECT ci.name AS City, (co.GNP / ci.population) AS GDP_Per_Capita
FROM country AS co
INNER JOIN city AS ci ON ci.countrycode = co.code
ORDER BY GDP_Per_Capita DESC;
```
