## 1. Dataset Overview (Clean Version)

| Item | Description |
| :--- | :--- |
| **Dataset name** | Lego Inventory (Cleaned) |
| **Authors** | Group 4 |
| **Number of entries** | 204 |
| **Number of features/variables** | 12 |
| **Format file (.csv, .txt, etc)** | .xlsx |

---

## 2. Dataset Structure (Clean Version)

| Feature/variable | Data type | Number of Unique values | Description                                                         | Example values |
| :--- | :--- | :--- |:--------------------------------------------------------------------| :--- |
| **id** | int64 | 204 | Unique identifier for each piece entry                              | 1, 2, 3... |
| **color** | object | 58 | Color of the piece                                                  | yellow, red, black |
| **is duplo?** | bool | 2 | Whether the piece is a Duplo block                                  | False, True |
| **size type** | object | 3 | Type of piece (e.g., brick, plate)                                  | plate, brick |
| **base shape** | object | 5 | Base shape of the piece                                             | rectangle, square |
| **base dimensions**| object | 12 | Standardized dimensions (e.g., WxL)                                 | 2x2, 2x4, 1x1 |
| **number of studs**| int64 | 10 | Count of studs on the piece                                         | 8, 4, 0 |
| **has slope?** | bool | 2 | Whether the piece has a sloped surface                              | False, True |
| **slope degree** | int64 | 4 | The angle of the slope, if present (0 if no slope)                  | 0, 45, 30 |
| **in stock** | int64 | 1 | Number of this exact piece                                          | 1 |
| **group number** | int64 | 5 | The original data entry group                                       | 1, 2, 3, 4, 5 |
| **is transparent?**| float64 | 2 | Whether the piece is transparent (1.0=True, 0.0=False, NaN=Unknown) | 1.0, 0.0, NaN |

---

## 3. Descriptive Statistics (Clean Version)

### Numeric columns

This table shows the descriptive statistics for the numeric columns identified in the final, cleaned dataset.

| | **id** | **number of studs** | **slope degree** | **in stock** | **group number** | **is transparent?** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Count** | 204.00 | 204.00 | 204.00 | 204.0 | 204.00 | 45.00 |
| **Mean** | 102.50 | 4.91 | 5.07 | 1.0 | 3.00 | 0.20 |
| **Standard deviation** | 59.03 | 5.00 | 14.11 | 0.0 | 1.42 | 0.40 |
| **Min** | 1.00 | 0.00 | 0.00 | 1.0 | 1.00 | 0.00 |
| **25%** | 51.75 | 2.00 | 0.00 | 1.0 | 2.00 | 0.00 |
| **50%** | 102.50 | 4.00 | 0.00 | 1.0 | 3.00 | 0.00 |
| **75%** | 153.25 | 6.00 | 0.00 | 1.0 | 4.00 | 0.00 |
| **Max** | 204.00 | 24.00 | 45.00 | 1.0 | 5.00 | 1.00 |

### Categorical/object columns

This table shows the descriptive statistics for the categorical and boolean columns identified in the final, cleaned dataset.

| | **color** | **is duplo?** | **size type** | **base shape** | **base dimensions** | **has slope?** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Count** | 203 | 204 | 204 | 204 | 203 | 204 |
| **Number of unique values** | 58 | 2 | 3 | 5 | 12 | 2 |
| **Most frequent value** | yellow | False | plate | rectangle | 2x2 | False |
| **Most frequent value (frequency)** | 18 | 171 | 104 | 109 | 47 | 180 |

---

## 4. Data Cleaning Procedure

### 4.1 Major data inconsistencies

| Issue | Names of Columns affected | Description of the Issue | Action Taken |
| :--- | :--- | :--- | :--- |
| **Inconsistent column labeling** | `sheet_name`, `transparent` | Column names were not standardized or optimal for analysis. | Created `group number` by extracting the number from `sheet_name`. Created `is transparent?` from `transparent`. Deleted original columns (`sheet_name`, `transparent`). |
| **Wrong data types** | `is duplo?`, `has slope?`, `in stock`, `is transparent?` | Boolean values were stored as "yes"/"no" strings. Numeric stock info was stored as "yes". Transparency column was float but intended as boolean. | Converted "yes"/"no" to 1/0 and then to `bool` type. Converted `in stock` "yes" to 1 and cast column to `int`. Cast `is transparent?` to `boolean` (which reverted to `float64` on Excel export/re-import to handle NaNs). |
| **Missing values** | `slope degree`, `base dimensions`, `color`, `is transparent?` | `slope degree` was NaN when `has slope?` was False. One `base dimensions` entry was invalid ("+x2x2x2x4"). One `color` entry was NaN. `is transparent?` was mostly NaN. | Filled `slope degree` NaN with 0 where `has slope?` was False. Replaced invalid `base dimensions` entry with NaN. `color` and `is transparent?` NaNs were left as is. |
| **Duplicates** | All feature columns | Multiple rows represented the same unique piece due to data entry from different groups. | Data was cleaned first. Duplicates were then identified by grouping on all key features. The 204 entries were found to represent 183 unique pieces. The final Excel file contains all 204 entries. |
| **Inconsistent categories** | `color`, `size type`, `base shape`, `base dimensions` | Casing was inconsistent ("Brick", "brick"). Spellings varied ("blck", "black"; "trapezium", "trapezoid"). Formats for dimensions were inconsistent ("2x4", "4x2", "2*4"). | Converted `color`, `size type`, and `base shape` to lowercase. Corrected misspellings (e.g., "blck" to "black"). Standardized shape names (e.g., "round" to "circle"). Removed spacers/operators from `base dimensions`, sorted dimensions, and joined with "x" (e.g., "4 X 2" -> "2x4"). |

---

## 5. Recommendations for good practices regarding data collection

The goal of this exercise was to experience the challenges of manual data entry and understand how inconsistencies can occur even with a predefined structure. In future work, using automated input forms or validation rules could help reduce such errors.

---

## 6. AI Disclaimer

I used AI-generated code for tasks such as standardizing color names, handling transparency values, unifying base dimension formats, and extracting group numbers, as shown below:

```python
# ai, adds blank after light/dark
df["color"] = df["color"].str.replace(r'^(light|dark)(?!\s)', r'\1 ', regex=True) 
# if color contains "transparent", make transparent column 1
df.loc[df['color'].str.contains('transparent', case=False, na=False), 'transparent'] = 1 
# delete transparent text
df['color'] = df['color'].str.replace('transparent', '', case=False, regex=False).str.strip()