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

#### Table =='product_emissions'==

1. **id**: Identifier for each product emission record.
2. **company_id**: Identifier for the company associated with the product.
3. **country_id**: Identifier for the country where the product is being produced.
4. **industry_group_id**: Identifier for the industry group to which the product belongs.
5. **year**: The year in which the emissions data was recorded.
6. **product_name**: The name of the product associated with the emissions data.
7. **weight_kg**: The weight of the product in kilograms.
8. **carbon_footprint_pcf**: The carbon footprint of the product, measured in CO2 equivalent.
9. **upstream_percent_total_pcf**: The percentage of the total carbon footprint attributed to upstream activities.
10. **operations_percent_total_pcf**: The percentage of the total carbon footprint attributed to operations.
11. **downstream_percent_total_pcf**: The percentage of the total carbon footprint attributed to downstream activities.

#### Table =='industry_groups'==

1. **id**: Unique identifier for each industry group.
2. **industry_group**: The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.

#### Table =='companies'==

1. **id**: Unique identifier for each company.
2. **company_name**: The name of the company, identifying the specific organization within the dataset.

#### Table =='countries'==

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
  FROM   product_emissions
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

#### 4. The Most Company With Highest Contribution To Carbon Emissions.
```
SELECT c.company_name, SUM(pe.carbon_footprint_pcf) AS total_carbon
FROM product_emissions pe 
JOIN companies c ON pe.company_id  = c.id
GROUP BY c.company_name
ORDER BY total_carbon DESC
LIMIT 10;
```

|company_name|total_carbon|
|------------|------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464|
|Daimler AG|1594300|
|Volkswagen AG|655960|
|"Mitsubishi Gas Chemical Company, Inc."|212016|
|"Hino Motors, Ltd."|191687|
|Arcelor Mittal|167007|
|Weg S/A|160655|
|General Motors Company|137007|
|"Lexmark International, Inc."|132012|
|"Daikin Industries, Ltd."|105600|

#### 5. The Most Countries With Highest Contribution To Carbon Emissions.
```
SELECT c.country_name, SUM(carbon_footprint_pcf) total_pcf
FROM product_emissions pe 
JOIN countries c ON pe.country_id = c.id
GROUP BY c.country_name 
ORDER BY total_pcf DESC
LIMIT 10;
```
|country_name|total_pcf|
|------------|---------|
|Spain|9786130|
|Germany|2251225|
|Japan|653237|
|USA|518381|
|South Korea|186965|
|Brazil|169337|
|Luxembourg|167007|
|Netherlands|70417|
|Taiwan|62875|
|India|24574|

#### 6. Trends of Carbon Footprints (PCFs) over years.

```
SELECT year, SUM(carbon_footprint_pcf) total_carbon_footprint
FROM product_emissions pe 
GROUP BY year
ORDER BY year;
```
|year|total_carbon_footprint|
|----|----------------------|
|2013|503857|
|2014|624226|
|2015|10840415|
|2016|1640182|
|2017|340271|


#### 7. Compare the reduction of emissions by industry over the years.
```
SELECT industry_group, year,
	   SUM(pe.carbon_footprint_pcf) AS total_carbon_emission
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id  = ig.id 
GROUP BY ig.industry_group, pe.year
ORDER BY ig.industry_group, pe.year;
```

|industry_group|year|total_carbon_emission|
|--------------|----|---------------------|
|"Consumer Durables, Household and Personal Products"|2015|931|
|"Food, Beverage & Tobacco"|2013|4995|
|"Food, Beverage & Tobacco"|2014|2685|
|"Food, Beverage & Tobacco"|2015|0|
|"Food, Beverage & Tobacco"|2016|100289|
|"Food, Beverage & Tobacco"|2017|3162|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|2015|8909|
|"Mining - Iron, Aluminum, Other Metals"|2015|8181|
|"Pharmaceuticals, Biotechnology & Life Sciences"|2013|32271|
|"Pharmaceuticals, Biotechnology & Life Sciences"|2014|40215|
|"Textiles, Apparel, Footwear and Luxury Goods"|2015|387|
|Automobiles & Components|2013|130189|
|Automobiles & Components|2014|230015|
|Automobiles & Components|2015|817227|
|Automobiles & Components|2016|1404833|
|Capital Goods|2013|60190|
|Capital Goods|2014|93699|
|Capital Goods|2015|3505|
|Capital Goods|2016|6369|
|Capital Goods|2017|94949|
|Chemicals|2015|62369|
|Commercial & Professional Services|2013|1157|
|Commercial & Professional Services|2014|477|
|Commercial & Professional Services|2016|2890|
|Commercial & Professional Services|2017|741|
|Consumer Durables & Apparel|2013|2867|
|Consumer Durables & Apparel|2014|3280|
|Consumer Durables & Apparel|2016|1162|
|Containers & Packaging|2015|2988|
|Electrical Equipment and Machinery|2015|9801558|
|Energy|2013|750|
|Energy|2016|10024|
|Food & Beverage Processing|2015|141|
|Food & Staples Retailing|2014|773|
|Food & Staples Retailing|2015|706|
|Food & Staples Retailing|2016|2|
|Gas Utilities|2015|122|
|Household & Personal Products|2013|0|
|Materials|2013|200513|
|Materials|2014|75678|
|Materials|2016|88267|
|Materials|2017|213137|
|Media|2013|9645|
|Media|2014|9645|
|Media|2015|1919|
|Media|2016|1808|
|Retailing|2014|19|
|Retailing|2015|11|
|Semiconductors & Semiconductor Equipment|2014|50|
|Semiconductors & Semiconductor Equipment|2016|4|
|Semiconductors & Semiconductors Equipment|2015|3|
|Software & Services|2013|6|
|Software & Services|2014|146|
|Software & Services|2015|22856|
|Software & Services|2016|22846|
|Software & Services|2017|690|
|Technology Hardware & Equipment|2013|61100|
|Technology Hardware & Equipment|2014|167361|
|Technology Hardware & Equipment|2015|106157|
|Technology Hardware & Equipment|2016|1566|
|Technology Hardware & Equipment|2017|27592|
|Telecommunication Services|2013|52|
|Telecommunication Services|2014|183|
|Telecommunication Services|2015|183|
|Tires|2015|2022|
|Tobacco|2015|1|
|Trading Companies & Distributors and Commercial Services & Supplies|2015|239|
|Utilities|2013|122|
|Utilities|2016|122|
