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

## Entity Relationship Diagram
## Question and Solution

Database was imported into PostgreSQL and the following SQL queries were executed to answer each relevant question.

The analysis includes the following questions:

1) Count of each chemical in the dataset
   
````sql
SELECT chemicalname, COUNT(*) AS chemicalcount
FROM chemical
GROUP BY chemicalname
ORDER BY count (*) DESC 
````

2) Count of titanium dioxide in makeup subcategories
   
````sql
SELECT subcategory, chemicalname, COUNT(*) AS count
FROM public.chemical
JOIN public.product
ON chemical.id = product.id
GROUP by product.subcategory, chemical.chemicalname
HAVING chemicalname in ('Titanium dioxide')
ORDER BY count DESC
```` 


3) Number of hazardous chemicals used by each company

````sql
SELECT companyname, COUNT(DISTINCT chemicalname) AS chemicalcount
FROM chemical
JOIN company
ON chemical.id = company.id
GROUP BY companyname
ORDER BY chemicalcount DESC
````


4) Product Count By Company 

````sql
SELECT companyname , COUNT(productname) as product 
FROM public.product
JOIN public.company
ON product.id = company.id
GROUP by companyname
ORDER BY product desc
````




5) Chemicals by category

````sql
SELECT subcategory AS productcategory, COUNT(*) AS chemicalcount
FROM product
GROUP BY subcategory
ORDER BY count (*) DESC
```` 

6) Number of chemicals removed by company

````sql
SELECT companyname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY companyname
ORDER BY countofchemicalsremoved DESC 
````

7) Number of Products Discontinued/Chemicals Removed By Year
   
````sql
SELECT chemicalname, discontinueddate, chemicaldateremoved 
FROM chemical
WHERE chemicaldateremoved IS NOT NULL
OR discontinueddate IS NOT NULL
````

8) Number of Times a Chemical Was Removed
   
````sql
SELECT chemicalname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY chemicalname
ORDER BY countofchemicalsremoved DESC
````

9) Chemical By Product and Company
   
````sql
SELECT chemicalname, COUNT(chemicaldateremoved) AS countofchemicalsremoved
FROM chemical
JOIN company
ON chemical.id = company.id
WHERE chemicaldateremoved IS NOT NULL
GROUP BY chemicalname
ORDER BY countofchemicalsremoved DESC
````


