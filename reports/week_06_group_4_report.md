# World Bank Project Report: Global Development Inequalities

## 0. Authors of the report
* **Name:** Group 4 
*  Paata, Julian, ..: Statistical Analysis, Visualisation, Reporting
*

## Statistical Diagnostics of GDP and Agricultural Land: Normality Tests, Data Transformations, and Correlation Analysis (Paata)

The chosen variables GDP (current USD) and agricultural land (% of total land area) for broader exploration relate to the dataset  consisting of several different countries as well as different years. Due to the data being spread out across several years and different countries, the aim is not to perform cause and effect or temporal interpretation. Rather, the aim is to investigate of what the variables tend to do in general as data is collated from different parts of the world. The goal of the distribution analysis is to find some level of non-normally distributed economic data that is common in samples gathered from multiple countries in the world. Such preliminary analysis is essential, especially for mixed global data, as other statistical techniques can expect the data set to have been transformed, or cleaned of outliers, in order to be a fit for analysis.
__

![dgp & agr. land hists](https://github.com/Avonik/ASDA_2025_newGroup_4_Portfolio/blob/main/additional_material/images/gdp%20%26%20agr.%20land%20histogram.png)

* Inspection of the histograms indicates that the data are not normally distributed. The distributions show noticeable skewness and deviations from the bell-shaped curve, suggesting that normality assumptions are not fully met. At this time we don't need to make normality test before we transform data.


### Improving Variable Distributions Through Log and Sqrt Transformations

![Agricultural Land Distribution](https://github.com/Avonik/ASDA_2025_newGroup_4_Portfolio/blob/main/additional_material/images/KDE%20for%20arg.%20land.png)

* The plots above show the distribution of the variable Agricultural Land in three forms: the original data, square-root transformed, and natural log transformed. After applying transformations, the distributions of the data become closer to normal compared to the original skewed data. Both the square-root and natural log transformations reduce skewness and make the data more symmetric. Among them, the squre root transformation results in a distribution that most closely approximates normality.


![GDP Distribution](https://github.com/Avonik/ASDA_2025_newGroup_4_Portfolio/blob/main/additional_material/images/KDE%20for%20gdp.png)

* The plots above show the distribution of the variable GDP in three forms: the original data, square-root transformed, and natural log transformed. After applying transformations, the distributions of the data become closer to normal compared to the original skewed data. However, in this case log transformation results in a distribution that most closely approximates normality.
  
### Normality Test for Transformed Variables

| Variable                | W Statistic | p-value           | Normal (p > 0.05) |
|-------------------------|------------:|-------------------:|--------------------|
| gdp_current_us_log      | 0.995135    | 1.055676e-12       | False              |
| agricultural_land_sqrt  | 0.955367    | 3.739916e-38       | False              |
* A Shapiro–Wilk normality test was conducted on the log-transformed GDP values and the square-root-transformed agricultural land data. While both transformations are expected to improve the shape of the distribution, the tests show that neither variable follows a normal distribution. In both cases, the p-values were much less than the {0.05} significance level, and thus, the normality of the variable was assumed to be rejected.


### Distribution After Outlier Removal

![Distribution after outlier removal](https://github.com/Avonik/ASDA_2025_newGroup_4_Portfolio/blob/main/additional_material/images/gdp%20%26%20agr%2C%20land%20after%20transformation.png)
* After removing outliers, the overall distribution of the transformed variables remained largely unchanged, indicating that extreme values had only a minimal impact on the shape of the distribution.

### Normality test after outlier removal

| Variable                | W Statistic | p-value           | Normal (p > 0.05) |
|-------------------------|------------:|-------------------:|--------------------|
| gdp_current_us_log      | 0.995121    | 1.006764e-12       | False              |
| agricultural_land_sqrt  | 0.955322    | 3.659627e-38       | False              |

* Also Normality test shows the same result. The distribution of the variables still deviates from normality.

### Correlation Analysis Between Variables

![scatterplot](https://github.com/Avonik/ASDA_2025_newGroup_4_Portfolio/blob/main/additional_material/images/agr.%20land%20vs%20gdp%20scatterplot.png)

* The initial scatter plot indicates that there is a non-linear and skewed pattern from Agricultural Land and GDP. Most data points are situated at low GDP levels, with a few extreme outliers at the highest levels which dominate the graph scale. The initial data showcase a lack of key linearity and homoscedasticity assumptions, violations a model of linear regression should not encounter. The scope of the second scatter plot is the transformed data, utilizing sqrt (Agricultural Land) and log (GDP) where this systematic alteration addresses the unequally distributed, extreme outlier, skewness. In the context of the graph, the transformed variables are not linearly correlated; rather, data points are situated in a scattered cloud. The overall meaning is that while the statistical properties of the data were improved by transformation, Agricultural Land is not a strong, simple linear explanatory variable for GDP, even after accounting for distribution issues.

#### Spearman's correlation test

| Transformation          | n    | r        | 95% CI       | p-value          | Power |
|-------------------------|-----:|---------:|-------------|----------------:|-------|
| Before Transformation   | 5538 | 0.132989 | [0.11, 0.16] | 2.825014e-23    | 1.0   |
| After Transformation    | 5536 | 0.132875 | [0.11, 0.16] | 3.135243e-23    | 1.0   |

* Spearman’s correlation coefficient was calculated to assess the relationship between agricultural land and GDP, as the normality test indicated that the variables were not normally distributed. Both before and after applying square-root and log transformations, the correlation remained weakly positive (r ≈ 0.133, p < 0.001), indicating a statistically significant but small association. Transformations and outlier removal slightly improved data properties but did not substantially change the underlying monotonic relationship, suggesting that countries with more agricultural land tend to have slightly higher GDP.






  


## Correlation: Health Expenditure and Life Expectancy (Julian)

### 1. Research Question (RQ)
Does a higher percentage of GDP spent on health expenditure correlate with a higher life expectancy at birth across different nations?

### 2. Data Inspection and Handling
We analyzed the relationship between **Current Health Expenditure (% of GDP)** and **Life Expectancy at Birth** for the period 1990–2019.

Initial inspection of the data distribution revealed significant challenges:
* **Skewness:** The health expenditure data was highly right-skewed. The majority of countries spend a relatively small percentage of their GDP on health, while a few wealthy nations (outliers such as the United States) spend a disproportionately high amount.
* **Non-Linearity:** Preliminary scatter plots indicated a non-linear relationship. The data followed a logarithmic curve, suggesting "diminishing returns"—initial increases in spending yield massive gains in life expectancy, but this effect plateaus for developed nations.

<img width="1478" height="501" alt="image" src="https://github.com/user-attachments/assets/f0ff9767-030e-4a42-855d-cd80ca1965de" />
<img width="1055" height="608" alt="image" src="https://github.com/user-attachments/assets/981d2379-2c00-4ff1-8ec1-58802b08f91b" />

**Transformations & Method:**
To address these issues without removing valid data points we applied two techniques:
1.  **Log-Transformation:** We applied a natural logarithm to the Health Expenditure variable. This normalized the distribution and linearized the relationship for better visualization.
2.  **Robust Statistics:** We opted for **Spearman’s Rank Correlation** rather than Pearson. Spearman is non-parametric and evaluates rank order rather than raw values, making it robust to the extreme outliers observed in the expenditure data.

### 3. Visualization
The scatter plot below visualizes the relationship after applying the log-transformation. The data points are color-coded by **Income Group** to highlight economic disparities.

The plot reveals a clear positive trend: as countries move from low spending (left) to high spending (right), life expectancy generally increases. However, the data also shows distinct clustering, with High-Income countries clustered at the top-right (high spending, high life expectancy) and Low-Income countries at the bottom-left.

<img width="1131" height="616" alt="image" src="https://github.com/user-attachments/assets/fe505f56-50d3-41e8-868d-6e051f10beb4" />

### 4. Statistical Test
We performed a Spearman Rank Correlation test to quantify the strength of the association.

* **Method:** Spearman Rank Correlation ($r_s$)
* **Correlation Coefficient:** **0.6642**
* **P-value:** **< 0.001** (0.0000e+00)

**Result:** The p-value is well below the significance level of 0.05, leading us to reject the null hypothesis. There is a **statistically significant, strong positive correlation** between health expenditure and life expectancy.

**Correlation by Income Group**
* Low income               : r = 0.1344 * (n=430)
* Upper middle income      : r = 0.2673 * (n=1018)
* Lower middle income      : r = 0.3495 * (n=907)
* High income              : r = 0.5242 * (n=1181)

### 5. Interpretation
The analysis confirms that nations investing a larger share of their GDP into health generally achieve longer life expectancies ($r_s = 0.66$). However, the "diminishing returns" pattern observed in the visualization is crucial for interpretation.

* **Group Differences:** The correlation is likely driven heavily by Low and Middle-Income nations, where basic health funding (vaccines, sanitation, maternal care) drastically reduces mortality. In High-Income nations, the relationship weakens; simply spending more money (as seen in the outlier case of the USA) does not guarantee the highest life expectancy, suggesting that lifestyle factors (diet, exercise) and healthcare efficiency play a larger role at that stage.
* **Causation vs. Correlation:** While the link is strong, expenditure is not the sole cause of longevity. High spending is often a proxy for overall economic development, better education, and infrastructure—all of which contribute to living longer. Therefore, while funding is necessary for long life, it is not sufficient on its own.
