# World Bank Project Report: Global Development Inequalities

## 0. Authors of the report
* **Name:** Group 4 Data Analysis Team

---

## 1. Dataset Overview

| Item | Description |
| :--- | :--- |
| **Dataset name** | World Bank Development Indicators |
| **Number of rows** | 17,272 (Initial Raw Data) |
| **Number of columns** | 52 |
| **Format file** | .csv |
| **Authors of the dataset** | The World Bank Group |
| **Source (name)** | World Bank Open Data |
| **Source (link)** | [data.worldbank.org](https://data.worldbank.org/) |

---

## 2. Dataset Structure

We are reducing the dataset to the most relevant columns to make this overview more usefull and structured.

The dataset contains a mix of categorical and numerical variables representing development metrics across countries and years.

| Feature/variable | Data type | Description | Example |
| :--- | :--- | :--- | :--- |
| `country` | Object | Name of the country or region | "Afghanistan" |
| `income_group` | Object | Economic classification of the country | "Low income" |
| `life_expectancy_at_birth` | Float | Average years a newborn is expected to live | 64.84 |
| `gdp_current_us` | Float | Gross Domestic Product in current US dollars | 2.57e+10 |
| `co2_emissions` | Float | Carbon dioxide emissions (metric tons per capita) | 4.5 |
| `access_to_electricity` | Float | Percentage of population with access to electricity | 80.97 |

---

## 3. Data Cleaning

The raw data required several processing steps to ensure quality and consistency for analysis.

| Issue | Affected Columns | Description of the Action Taken |
| :--- | :--- | :--- |
| **Inconsistent column labeling** | All Columns | Standardized names: converted to lowercase, stripped whitespace, and replaced spaces with underscores (e.g., `Birth Rate` $\to$ `birth_rate`). |
| **Missing values** | `income_group` | Dropped rows where `income_group` was missing to ensure valid comparative analysis. |
| **Missing values** | `life_expectancy_at_birth` | Dropped approximately 1,000 rows with null values to allow for accurate health trend analysis. |
| **Duplicates** | All Columns | Verified the dataset for duplicate entries (0 duplicates found). |
| **Data Integration** | `Region`, `Income` | Merged the main dataset with `income.xlsx` metadata to add `Region` and `Income group` classifiers to each country. |

---

## 4. Descriptive Statistics

Summary of key numeric indicators after cleaning:

| Column | Count | Mean | Std | Min | 25% | 50% | 75% | Max |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Agricultural Land (%)** | 10,872 | 36.86 | 22.43 | 0.26 | 17.77 | 37.65 | 54.71 | 93.44 |
| **Access to Electricity (%)** | 5,676 | 80.97 | 29.64 | 0.53 | 70.46 | 99.09 | 100.0 | 100.0 |
| **Renewable Energy (%)** | 6,147 | 30.65 | 30.38 | 0.00 | 3.73 | 19.78 | 53.02 | 98.34 |
| **Life Expectancy (Years)** | 12,337 | 64.84 | 11.26 | 11.99 | 57.95 | 67.66 | 73.08 | 85.50 |
| **CO2 Emissions** | 7,408 | - | - | - | - | - | - | - |

---

## 5. The Data Story: Competing for a Better Future

Our analysis of the World Bank data tells a story of a world divided by economic fortune but connected by shared challenges. By segmenting countries into **High**, **Upper-Middle**, **Lower-Middle**, and **Low** income groups, distinct patterns emerge that define the global development landscape.

### I. The Privilege of Long Life
The most striking inequality is visible in **Life Expectancy**. 
* **The Gap:** There is a dramatic staircase effect. High-income nations sit at the top with life expectancies averaging over 70 years, while Low-income nations struggle in the low 50s.
* **The Driver:** This disparity is strongly correlated with `gdp_current_us` and `access_to_electricity` (Correlation: 0.83), indicating that economic infrastructure is a prerequisite for public health.

![img_9.png](img_9.png)

### II. The Carbon Cost of Wealth
Prosperity comes with a price. Our analysis shows a strong positive correlation between **Income/GDP** and **CO2 Emissions**.
* **The Trend:** As countries climb the income ladder, their carbon footprint expands. High-income countries are the largest emitters per capita.
* **The Twist:** Interestingly, `renewable_energy_consumption` shows a *negative* correlation with income in many cases. This reveals that many low-income nations rely on traditional renewable biomass (wood/waste) for fuel, whereas developing middle-income nations switch to fossil fuels to power rapid industrialization. Only at the highest income levels do we see a technological return to modern renewables, creating a U-shaped sustainability curve.

![img_8.png](img_8.png)

### III. The Digital and Physical Divide
Infrastructure metrics like `access_to_electricity` acts as gatekeepers to the modern economy.
* **High Income:** Near 100% saturation.
* **Low Income:** High variance, with many nations having less than 50% access. This infrastructure gap makes it exponentially harder for these economies to catch up, creating a feedback loop of inequality.

![img_7.png](img_7.png)



### IV. The Demographic Transition: Life Expectancy vs. Birth Rate
A powerful indicator of a nation's development phase is the relationship between **Life Expectancy** and **Birth Rate**.
* **The Negative Correlation:** As shown in our scatter plots, there is a clear, strong negative correlation between these two variables. As nations develop, they trade high birth rates for longer lives.
  
* **The Clustering:** 
  * **Low Income / Developing:** These nations (often in Sub-Saharan Africa) appear in the top-left of the graph, characterized by high birth rates (30-45 births per 1000) and lower life expectancy (< 60 years).
  * **High Income / Developed:** These nations cluster in the bottom-right, with low birth rates (< 15 per 1000) and high life expectancy (> 75 years).
* **Statistical Significance:** Our ANOVA and Tukey post-hoc tests on `log_birth_rate` confirmed that these differences between income groups are statistically significant, verifying that the demographic shift is a fundamental structural change that accompanies economic growth.

![img_5.png](img_5.png)

### IV. Conclusion
The data suggests that economic development is the primary engine for improving human life span and basic access to services. However, the current model of development is resource-intensive. The challenge for the next century, as shown by the data, is to help Low and Middle-income countries achieve High-income health and infrastructure standards without replicating the high-carbon trajectory of the current wealthy nations.

---

