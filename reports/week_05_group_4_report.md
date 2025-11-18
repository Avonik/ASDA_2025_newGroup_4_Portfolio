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

<img width="946" height="550" alt="image" src="https://github.com/user-attachments/assets/7f46ad03-34e2-46a0-808f-4bc0c45f6ba0" />


### II. The Carbon Cost of Wealth
Prosperity comes with a price. Our analysis shows a strong positive correlation between **Income/GDP** and **CO2 Emissions**.
* **The Trend:** As countries climb the income ladder, their carbon footprint expands. High-income countries are the largest emitters per capita.
* **The Twist:** Interestingly, `renewable_energy_consumption` shows a *negative* correlation with income in many cases. This reveals that many low-income nations rely on traditional renewable biomass (wood/waste) for fuel, whereas developing middle-income nations switch to fossil fuels to power rapid industrialization. Only at the highest income levels do we see a technological return to modern renewables, creating a U-shaped sustainability curve.

<img width="905" height="567" alt="image" src="https://github.com/user-attachments/assets/1bbee65e-838f-4ad9-9808-17ee1609488b" />

**Average Carbon Intensity (CO2 / GDP) by Income Group**

Scaled Average Carbon Intensity (CO2 / GDP) by Income Group:
-----------------------------------------------------
income_group
Upper middle income    1.695194
Lower middle income    0.835624
High income            0.650598
Low income             0.359056
Name: carbon_intensity, dtype: float64
-----------------------------------------------------
Interpretation: A lower intensity suggests more economically efficient (cleaner) production.

*   The analysis of average Carbon Intensity ($\text{CO}_2$/$\text{GDP}$) shows a non-monotonic trend where the Upper middle income group is the most intensive (1.675 tons $\text{CO}_2$ per Million USD GDP), while the High income group is significantly cleaner (0.647 tons), reflecting improved efficiency post-industrialization.

![Histogram](../additional_material/images/1histogram_co2_intensity.png)

*   Data is left skewed. We try square root transformation.

![Histogram & Boxplot](../additional_material/images/2sqrt_c02_intensity.png)

*   Data is right skewed even after square root transformation. We will try to clean outliers by using Interquartile Range (IQR) method.

![Histogram & Boxplot](../additional_material/images/22sqrt_c02_intensity.png)

*   The distribution is more normalized after we clean outliers. We can run Anova test now.

 
  
    

    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }


  
    
      
      sum_sq
      df
      F
      PR(>F)
    
  
  
    
      C(income_group)
      0.000034
      3.0
      186.506945
      1.233504e-114
    
    
      Residual
      0.000305
      4995.0
      NaN
      NaN
    
  


    

  
    

  
    
  
    

  
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  

    
      const buttonEl =
        document.querySelector('#df-24947956-cfaa-42c7-9806-a8b210d1b190 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-24947956-cfaa-42c7-9806-a8b210d1b190');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    
  


    
      


    
        
    

      


  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }


      
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-f3527b35-d663-4837-85ca-2a9353f3c942 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      
    

  
    
      .colab-df-generate {
        background-color: #E8F0FE;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        display: none;
        fill: #1967D2;
        height: 32px;
        padding: 0 0 0 0;
        width: 32px;
      }

      .colab-df-generate:hover {
        background-color: #E2EBFA;
        box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
        fill: #174EA6;
      }

      [theme=dark] .colab-df-generate {
        background-color: #3B4455;
        fill: #D2E3FC;
      }

      [theme=dark] .colab-df-generate:hover {
        background-color: #434B5C;
        box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
        filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
        fill: #FFFFFF;
      }
    
    

  
    
  
    
    
      (() => {
      const buttonEl =
        document.querySelector('#id_56975e16-10df-49f0-a337-87ba26d2bb82 button.colab-df-generate');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      buttonEl.onclick = () => {
        google.colab.notebook.generateWithVariable('aov_table2');
      }
      })();
    
  

    
  


### III. The Digital and Physical Divide
Infrastructure metrics like `access_to_electricity` acts as gatekeepers to the modern economy.
* **High Income:** Near 100% saturation.
* **Low Income:** High variance, with many nations having less than 50% access. This infrastructure gap makes it exponentially harder for these economies to catch up, creating a feedback loop of inequality.

<img width="1063" height="644" alt="image" src="https://github.com/user-attachments/assets/b79c583d-866e-432d-a49a-3264624781ee" />





### IV. The Demographic Transition: Life Expectancy vs. Birth Rate
A powerful indicator of a nation's development phase is the relationship between **Life Expectancy** and **Birth Rate**.
* **The Negative Correlation:** As shown in our scatter plots, there is a clear, strong negative correlation between these two variables. As nations develop, they trade high birth rates for longer lives.
  
* **The Clustering:** 
  * **Low Income / Developing:** These nations (often in Sub-Saharan Africa) appear in the top-left of the graph, characterized by high birth rates (30-45 births per 1000) and lower life expectancy (< 60 years).
  * **High Income / Developed:** These nations cluster in the bottom-right, with low birth rates (< 15 per 1000) and high life expectancy (> 75 years).
* **Statistical Significance:** Our ANOVA and Tukey post-hoc tests on `log_birth_rate` confirmed that these differences between income groups are statistically significant, verifying that the demographic shift is a fundamental structural change that accompanies economic growth.

<img width="856" height="600" alt="image" src="https://github.com/user-attachments/assets/0b4fa649-be02-40c6-96bf-a4d0cfdbaa02" />


---
### IV. Conclusion
The data suggests that economic development is the primary engine for improving human life span and basic access to services. However, the current model of development is resource-intensive. The challenge for the next century, as shown by the data, is to help Low and Middle-income countries achieve High-income health and infrastructure standards without replicating the high-carbon trajectory of the current wealthy nations.

---






