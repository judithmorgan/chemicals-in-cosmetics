# Chemicals in Cosmetics

![chemicals in cosmetics image](../assets/chemicalsincosmetics.png)

# Table of Contents 
- [Task](#task)
- [Tools](#tools)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Question and Solution](#question-and-solution)

Please note that the database used for this case study can be found on [Data.gov] (https://catalog.data.gov/dataset/chemicals-in-cosmetics-2a971).

If you have any questions, reach out to me on [LinkedIn](https://www.linkedin.com/in/judithpmorgan/).

## Task
This project explores harmful ingredients found in cosmetic products using a dataset extracted from the California Safe Cosmetics Program (CSCP) in the California Department of Public Health.

The primary purpose of the CSCP is to collect information on hazardous and potentially hazardous ingredients in cosmetic products sold in California and to make this information available to the public.

My goal is to identify any insights found from the data that might be helpful to a makeup user who wants to be more proactive about using products with cleaner ingredients and to support companies with that goal.


## Process

Kicking off the analysis, I downloaded the relevant dataset from Data.Gov. After the download, I cleaned and organized the data using SQL. To isolate specific patterns and trends, I created separate sheets, meticulously breaking down the information. Finally, each prepared sheet was loaded into Tableau for visualization.

## Table Diagram
As the dataset was large and filled with many columns. I divided up the dataset into three tables: product, company, and chemical. I added a unique numerical number for every row in the dataset, known as id. This was used as the primary key.

![chemicals in cosmetics image](../assets/tablediagram.png)


## Question and Solution

Database was imported into PostgreSQL and the following SQL queries were executed to answer each relevant question.

The analysis includes the following questions:

1) How many times did each unique chemical appear in the dataset?
   
````sql
SELECT chemicalname, COUNT(*) AS chemicalcount
FROM chemical
GROUP BY chemicalname
ORDER BY count (*) DESC 
````

2) In which makeup subcategory is titanium dioxide found most frequently?
   
````sql
SELECT subcategory, chemicalname, COUNT(*) AS count
FROM public.chemical
JOIN public.product
ON chemical.id = product.id
GROUP by product.subcategory, chemical.chemicalname
HAVING chemicalname in ('Titanium dioxide')
ORDER BY count DESC
```` 


3) How many potentially hazardous/hazardous chemicals were used by each company in the dateset?

````sql
SELECT companyname, COUNT(DISTINCT chemicalname) AS chemicalcount
FROM chemical
JOIN company
ON chemical.id = company.id
GROUP BY companyname
ORDER BY chemicalcount DESC
````


4) How many products did each company have in the dataset? 

````sql
SELECT companyname , COUNT(productname) as product 
FROM public.product
JOIN public.company
ON product.id = company.id
GROUP by companyname
ORDER BY product desc
````


5) Which chemical category has the highest amount of hazadous chemicals found?

````sql
SELECT subcategory AS productcategory, COUNT(*) AS chemicalcount
FROM product
GROUP BY subcategory
ORDER BY count (*) DESC
```` 

6) Which company took the most effort to remove hazardous/potentially hazardous chemicals in their products? 

````sql
SELECT companyname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY companyname
ORDER BY countofchemicalsremoved DESC 
````

7) Which year saw the most number of products discontinued and/or chemicals removed by all companies in the dataset?
   
````sql
SELECT chemicalname, discontinueddate, chemicaldateremoved 
FROM chemical
WHERE chemicaldateremoved IS NOT NULL
OR discontinueddate IS NOT NULL
````

8) Which chemical was removed the most in the dataset?
   
````sql
SELECT chemicalname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY chemicalname
ORDER BY countofchemicalsremoved DESC
````

9) List out all chemicals by product and company.
   
````sql
SELECT chemicalname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY chemicalname
ORDER BY countofchemicalsremoved DESC
````


