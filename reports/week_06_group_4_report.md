# World Bank Project Report: Global Development Inequalities

## 0. Authors of the report
* **Name:** Group 4 Data Analysis Team
*  Paata: Statistical Analysis, Visualisation, Reporting
*
*

## Statistical Diagnostics of GDP and Agricultural Land: Normality Tests, Data Transformations, and Correlation Analysis

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
