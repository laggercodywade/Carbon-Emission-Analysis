# CarbonEmission-Analysis
### 1.Introduction
![image](https://github.com/laggercodywade/Carbon-Emission-Analysis/blob/main/cover.jpg)
Photo by Chris LeBoutillier (unsplash.com)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

### Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

### Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![image](https://github.com/laggercodywade/Carbon-Emission-Analysis/blob/main/Database%20diagram.png)

### Tables' columns description
#### Table <mark>product_emissions</mark>
id: Identifier for each product emission record.
1. **company_id**: Identifier for the company associated with the product.

2. **country_id**: Identifier for the country where the product is being produced.

3. **industry_group_id**: Identifier for the industry group to which the product belongs.

4. **year**: The year in which the emissions data was recorded.

5. **product_name**: The name of the product associated with the emissions data.

6. **weight_kg**: The weight of the product in kilograms.

7. **carbon_footprint_pcf**: The carbon footprint of the product, measured in CO2 equivalent.

8. **upstream_percent_total_pcf**: The percentage of the total carbon footprint attributed to upstream activities.

9. **operations_percent_total_pcf**: The percentage of the total carbon footprint attributed to operations.

10. **downstream_percent_total_pcf**: The percentage of the total carbon footprint attributed to downstream activities.

#### Table <mark>'industry_groups'<mark>
1. **id**: Unique identifier for each industry group.
2. **industry_group**: The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.
 

#### Table <mark>'companies'<mark>
1. **id**: Unique identifier for each company.
2. **company_name**: The name of the company, identifying the specific organization within the dataset.
 

#### Table <mark>'countries'<mark>
1. **id**: Unique identifier for each country.
2. **country_name**: The name of the country.

##### 1. Top 10 Product Most To Carbon Emission
```
SELECT product_name,   
       ROUND(AVG(carbon_footprint_pcf),2) AS average_carbon_footprint_pcf
FROM product_emissions
GROUP BY  product_name
ORDER BY average_carbon_footprint_pcf DESC
LIMIT 10;
```
| product_name                                                                                                                       | average_carbon_footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00                   | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00                   | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00                   | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00                   | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00                    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00                    | 
| TCDE                                                                                                                               | 99075.00                     | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00                     | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00                     | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00                     |

##### 2. Top 10's Product Have Most Carbon Emission
```
SELECT pe.product_name,ig.industry_group,average_carbon_footprint_pcf
FROM (
  SELECT   product_name,
  		   industry_group_id,
		   ROUND(AVG(carbon_footprint_pcf),2) AS average_carbon_footprint_pcf
  FROM 	 product_emissions
  GROUP BY product_name,industry_group_id
  ORDER BY average_carbon_footprint_pcf DESC
  LIMIT 10
) AS pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id;

```

| product_name                                                                                                                       | industry_group                     | average_carbon_footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------------------------: | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00                    | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00                     | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00                     | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00                     | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00                   | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00                   | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00                   | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00                   | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00                    | 
| TCDE                                                                                                                               | Materials                          | 99075.00                     | 
##### 3.Top 10's Industry Group Have Most Carbon Emission
```
 SELECT ig.industry_group,SUM(carbon_footprint_pcf) total_emission
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY total_emission DESC
LIMIT 10;

```
| industry_group                                   | total_emission | 
| -----------------------------------------------: | -------------: | 
| Electrical Equipment and Machinery               | 9801558        | 
| Automobiles & Components                         | 2582264        | 
| Materials                                        | 577595         | 
| Technology Hardware & Equipment                  | 363776         | 
| Capital Goods                                    | 258712         | 
| "Food, Beverage & Tobacco"                       | 111131         | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486          | 
| Chemicals                                        | 62369          | 
| Software & Services                              | 46544          | 
| Media                                            | 23017          | 
 
####  4.
