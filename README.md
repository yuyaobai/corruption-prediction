### About this Project

This project is part of the LSE's [DS202W Data Science for Social Scientists](https://lse-dsi.github.io/DS202/2024/winter-term/) course. Using a data set on economic indicators and press freedom, the aim of this project is to classify countries according to their level of corruption. To do this, I ran several classification models, tuned their hyperparameters, and evaluated and interpreted their performance metrics in the context of the dataset. 

The sections below are taken from the project's prompt.

### About the Data

We will use a data set, which is a mix of indicators extracted from World Bank Databases, [Reporters Without Borders (RSF) data](https://rsf.org/en/index?year=2022) (relating to the World Press Freedom Index^[to read more about the various World Freedom Press Index components, see [here](https://rsf.org/en/index-methodologie-2022?year=2022&data_type=general)]), [data from the Harvard Growth Lab about the Economic Complexity Index](https://atlas.hks.harvard.edu/rankings), [data from Transparency International](https://www.transparency.org/en/cpi/2022) (for the 2022 Corruption Perception Index or CPI score 2022), data from the United Nations University World Institute for Development Economics Research (UNU-WIDER)'s [World Income Inequality Database (WIID)](https://www.wider.unu.edu/database/world-income-inequality-database-wiid) and finally, data from the [World Population Review (adult literacy rates)](https://worldpopulationreview.com/country-rankings/literacy-rate-by-country).

Here is an overview of the indicators in our data and their meaning:

| **Indicator**                          | **Definition**                                                                                                                                                        | **Source**                                                                                                      |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| **Inflation, consumer prices (annual %)** | Measures the annual percentage change in the average price level of a basket of goods and services. Indicates the rate at which the general price level is rising.     | [World Bank](https://data.worldbank.org/indicator/FP.CPI.TOTL.ZG)                                            |
| **Urbanization (%)**                   | Represents the percentage of a country’s population living in urban areas. Reflects the extent of urbanization in a country.                                          | [World Bank](https://data.worldbank.org/indicator/SP.URB.TOTL.IN.ZS)                                          |
| **FDI (% of GDP)**                     | Foreign Direct Investment as a percentage of GDP. Reflects foreign investment in the economy, showing a country’s level of economic openness.                         | [World Bank](https://data.worldbank.org/indicator/BX.KLT.DINV.WD.GD.ZS)                                      |
| **GDP per capita, PPP (current international $)** | GDP per capita adjusted for purchasing power parity (PPP). Provides a comparative measure of living standards across countries.                                       | [World Bank](https://data.worldbank.org/indicator/NY.GDP.PCAP.PP.CD)                                          |
| **Unemployment, total (% of total labor force)** | Percentage of the labor force that is unemployed, based on the International Labour Organization's model.                                                         | [World Bank](https://data.worldbank.org/indicator/SL.UEM.TOTL.ZS)                                            |
| **Tax revenue (% of GDP)**             | The total revenue from taxes collected by the government, expressed as a percentage of GDP. Indicates the government’s capacity to generate tax income.                | [World Bank](https://data.worldbank.org/indicator/GC.TAX.TOTL.GD.ZS)                                         |
| **Individuals using the Internet (% of population)** | The percentage of the total population that has access to and uses the internet. Reflects digital connectivity within a country.                                      | [World Bank](https://data.worldbank.org/indicator/IT.NET.USER.ZS)                                            |
| **Trade Openness (% of GDP)**          | The sum of exports and imports of goods and services as a percentage of GDP. Shows how integrated a country is with the global economy.                               | [World Bank](https://data.worldbank.org/indicator/NE.TRD.GNFS.ZS)                                            |
| **Rule of Law Score**                  | Measures the extent to which individuals can rely on the legal system to protect their rights. Includes factors like law enforcement and judicial independence.       | [World Bank](https://data.worldbank.org/indicator/RL.EST) |
| **Government Effectiveness**           | Reflects the quality of public services, civil service quality, and the government's ability to implement policies.                                                    | [World Bank](https://data.worldbank.org/indicator/GE.EST)  |
| **Regulatory Quality**                 | Measures the government’s ability to design and implement policies that promote the private sector’s development.                                                      | [World Bank](https://data.worldbank.org/indicator/RQ.EST)  |
| **Real GDP Growth (% Change)**         | The annual percentage change in real GDP. Reflects the economic growth or contraction within a country.                                                               | [World Bank](https://data.worldbank.org/indicator/NY.GDP.MKTP.KD.ZG)                                          |
| **Voice and Accountability: Estimate** | Measures the extent to which a country’s citizens can participate in selecting their government, freedom of expression, and access to information.                    | [World Bank](https://data.worldbank.org/indicator/VA.EST) |
| **Political Stability and Absence of Violence/Terrorism: Estimate** | An estimate of the political stability of a country, including the absence of violence, terrorism, and civil unrest.                                                  | [World Bank](https://data.worldbank.org/indicator/PV.EST) |
| **Central government debt, total (% of GDP)** | Total government debt expressed as a percentage of GDP. Reflects how much a government borrows relative to its economic output.                                        | [World Bank](https://data.worldbank.org/indicator/GC.DOD.TOTL.GD.ZS)                                          |
| **External debt stocks (% of GNI)**    | Total external debt as a percentage of a country’s Gross National Income (GNI). Indicates a country’s financial obligations to foreign creditors.                     | [World Bank](https://data.worldbank.org/indicator/DT.DOD.DECT.GN.ZS)                                          |
| **Public and publicly guaranteed debt service (% of exports)** | The ratio of a country’s debt service payments (interest and principal) to its export earnings. Shows the financial strain from external debt.                         | [World Bank](https://data.worldbank.org/indicator/DT.TDS.DECT.EX.ZS)                                          |
| **Domestic credit to private sector (% of GDP)** | The total financial credit extended to the private sector expressed as a percentage of GDP. Indicates the financial sector’s contribution to economic activity.        | [World Bank](https://data.worldbank.org/indicator/FS.AST.PRVT.GD.ZS)                                          |
| **RSF World Freedom of Press Score**   | A score measuring the degree of press freedom in a country. Higher scores indicate greater freedom of the press and less governmental interference.                   | [RSF](https://rsf.org/en/index?year=2022)                                                                  |
| **ECI (Economic Complexity Index)**    | Measures the diversity and sophistication of a country's productive capabilities. It evaluates how complex and interconnected a country's exports are.                 | [Atlas of Economic Complexity](https://atlas.hks.harvard.edu/rankings)                                        |
| **Corruption Perception Index (CPI)**  | A score indicating the perceived levels of corruption in the public sector. A lower score suggests higher levels of perceived corruption.                             | [Transparency International](https://www.transparency.org/en/cpi/2022/index/nzl)                            |
| **Gini Index**                         | A measure of income inequality within a country. A Gini index of 0 represents perfect equality, while an index of 100 represents perfect inequality.                   | [WID.World](https://www.wider.unu.edu/database/world-income-inequality-database-wiid)                          |
| **Total Population Literacy Rate**     | The percentage of people aged 15 and above who can read and write. Reflects the education level and access to literacy programs within a country.                     | [World Population Review](https://worldpopulationreview.com/country-rankings/literacy-rate-by-country)         |

<br/>

All data, except Gini index and Total Population Literacy Rate, were collected for the year 2022. For both Gini Index and Total Population Literacy Rate, the data for the most recent year on record (up to 2022) was retrieved (the data goes back to 2016 for Gini and to 2011 for literacy rates).  

<br/>

Our aim through this dataset is to try and classify countries according to their level of corruption (corruption perception index 2022).

### Question 1

Load the data and explore the characteristics of the data (e.g missingness patterns, interesting variable distributions, etc.)

### Question 2

Create the outcome variable `corruption` for our classification models i.e: 

- if the corruption perception index 2022 is higher or equal to 50, the country is classified `NotCorrupt` and assigned to class `0`
- otherwise, the country is classified `Corrupt` and assigned to class `1`

### Question 3

Create a baseline model (using the model of your choice) using a simple training/test split to predict `corruption`.
Explain your model and feature selection and any other modeling choices you made. Explain the performance of your model in the context of your dataset/application.

### Question 4

Select two/three models (one being a tree-based model). Tune their hyperparameters and/or evaluate their performance using cross-validation.
How well do they perform compared to your baseline model?
