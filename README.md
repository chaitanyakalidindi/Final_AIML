
# Housing Price Index – Microeconomic Factors  

**Authors:** Sathwika Varma Kalidindi, Chaitanya  

---

## Executive Summary  

Many millennials and members of Gen Z want to buy homes, but they can’t afford one. The number of first-time homebuyers in 2004 was nearly 3.2 million; by 2024, that number had fallen by almost two-thirds to just 1.14 million, according to the National Association of Realtors. Only one-quarter of the homes purchased last year were bought by first-time buyers – a historic low. Yet at the same time, there’s no shortage of builders eager to break ground to boost the housing supply. So what’s the holdup? Understanding how macroeconomic conditions (interest rates, mortgage rates, material costs) and demographic dynamics shape housing prices is crucial to answering this question  

The findings can help:  
-  **Younger generations** plan long-term financial stability in housing markets.  
-  **Builders and developers** adjust supply strategies.  
-  **Financial institutions** manage lending risk more effectively.  

---

##  Research Question  

Our study aims to answer:  
1. What are the most influential factors driving house price fluctuations?  
2. How do mortgage and interest rates directly affect affordability and demand?  
3. What are the regional and demographic differences in housing price sensitivity?  

---

##  Data Sources  

The dataset was prepared using **real U.S. statistical data** from **17 different sources**.  
We combined data by **state, month, and year** (2008–2019) to capture key microeconomic and demographic signals.  

A unique aspect of this project is that we **researched and built our own dataset** tailored to the problem — reflecting real-world ML challenges.  

**Data sources include:**  
- [Zillow Housing Prices](https://www.zillow.com/research/data/)  
- [Mortgage Rates (FRED)](https://fred.stlouisfed.org/series/MORTGAGE30US)  
- [Lumber Prices (FRED)](https://fred.stlouisfed.org/series/WPU081)  
- [Population Distribution by Age (KFF)](https://www.kff.org/state-health-policy-data/state-indicator/distribution-by-age/?currentTimeframe=0)  
- [Exports of Goods by State (FRED)](https://fred.stlouisfed.org/release?rid=449&t=exports)  
- [Imports of Goods by State (FRED)](https://fred.stlouisfed.org/release?rid=449&t=imports)  
- [Producer Price Index (FRED)](https://fred.stlouisfed.org/series/PPIACO)  
- [Consumer Price Index (FRED)](https://fred.stlouisfed.org/series/CPIAUCSL)  
- [State Demographics (Kaggle)](https://www.kaggle.com/datasets/jamesbailey0/state-demographics-1962-2024/data)  
- [Per Capita Income (FRED)](https://fred.stlouisfed.org/release?rid=110)  
- [Homeownership Rate (FRED)](https://fred.stlouisfed.org/release?rid=144&t=homeownership)  
- [NASDAQ Index (FRED)](https://fred.stlouisfed.org/series/NASDAQCOM)  

---

##  Methodology  

###  Data Cleaning  
- All datasets were stored in `/content/data/`.  
- Each dataset was cleaned and merged by **state, month, and year**.  
- Target variable stored in: `State_Zhvi_AllHomes.csv` (Zillow Home Value Index).  
- A **relative increase dataset** was created by divinfing base **01/2008 as the base date** with the rest of the values to get relative increse in when compared to 01/2008.  
  - This removes bias from starting price levels.  
  - Allows clean, comparable growth signals across states.  

---

###  Data Exploration  
We began with **Exploratory Data Analysis (EDA)** before modeling.  

The time-series plots show clear trends between 2008 and 2018. House prices rose steadily after the 2008 dip, supported by growth in GDP, income, and population. Interest and mortgage rates fell, fueling affordability, while ownership rates declined. Demographic shifts—especially growth in adults aged 26–34 and 65+—highlight changing housing demand, alongside broader market factors like imports, exports, and inflation.
<img width="1490" height="3490" alt="image" src="https://github.com/user-attachments/assets/7be49ed3-db02-4dfb-a6dd-018b6dcfbdbc" />

To see how features relate to house prices, we created scatter plots:  

-  **Positive Relationship**: Both the X axis and y axis increase together  
-  **Negative Relationship**: One increases while the other decreases  
-  **No Clear Relationship**: X axis little or no visible effect on y axis
<img width="1489" height="1990" alt="image" src="https://github.com/user-attachments/assets/70b8c389-ea5d-4da4-a19f-bfaad0d14597" />

From these plots, we see that some features show a strong influence. To make if ineaser to interpret the plot below, we have ranked all the features by their correlation with average house price, from positive to negative.
**Ranking Feature Impact**  
- **Strong**: Homeownership rate, Real Per-Capita Income, Adults 26–34, GDP of goods.  
- **Weaker**: Imports, Exports, some economic indices.  
<img width="559" height="549" alt="image" src="https://github.com/user-attachments/assets/3c459ee7-5726-410d-b915-8d4e9f3789a6" />

---

##  Modeling Approach  
 
Our goal was to understand the key drivers of **average house prices**. We chose Root Mean Squared Error (RMSE) as our evaluation metric. RMSE is well-suited for regression tasks as it measures the average magnitude of errors between predicted and actual prices, expressed in the same units as the target variable (dollars). 
To do this, we trained several machine learning models, including:  

- Linear Regression  
- Ridge  
- Lasso  
- Lasso with Feature Selection  
- Support Vector Machine (SVM)  
- Decision Tree  

We built two versions of the model:  
1. **Absolute Model** – compared raw feature values directly to house prices.  
2. **Relative Model** – compared the growth of features and house prices since a baseline of **January 2008**.  

---

##  Results  

### Absolute value dataset Model (Raw Values)  
<img width="989" height="790" alt="image" src="https://github.com/user-attachments/assets/97cf4a8d-8345-4f04-9447-a6d9d4d81cd4" />

**Demographics**  
- Adults **35–54** and **26–34**: strong positive impact — prime home-buying age groups.  
- Adults **19–25**: negative effect on prices, likely due to limited buying power.  
- High homeownership rates: linked to weaker demand for new homes, reducing price growth.  

**Macroeconomic Factors**  
- **Mortgage and interest rates**: influence affordability and borrowing capacity.  
- **GDP and imports of goods**: positively linked to stronger housing markets.  
- **Exports of goods**: negatively correlated, possibly reflecting capital outflows. 

**Model Reults**
Linear Regression, Ridge, and Lasso showed similar performance, with test RMSE values ranging from  5,300 to 5,700. The Ridge model with feature engineering achieved a slightly lower RMSE, suggesting it may generalize marginally better. Decision Tree and SVM, however, had relatively higher RMSE values, making them unsuitable for absolute value dataset.

<img width="1189" height="790" alt="image" src="https://github.com/user-attachments/assets/3bb540ca-4dfa-48a9-b3ab-147efaa9b881" />


---

### Relative Growth Since 2008 Dateset Model 
<img width="989" height="790" alt="image" src="https://github.com/user-attachments/assets/bd1e6185-9713-41c8-a699-dadb7b2d4ceb" />

- **Income growth (Real_PerCap_rel)** and **goods-sector growth (GDP of goods)** are the strongest drivers of long-term price momentum.  
- **Demographic shifts**:  
  - **Children (0–18)**: linked to faster price growth — families with newborns are more likely to settle and buy homes.  
  - **Adults 65+**: positive effect, consistent with retiree in-migration.  
  - **Adults 55–64**: weaker growth, likely due to pre-retirement downsizing.  
- **Inflation (CPI of goods)** and **construction-related activity** (lumber, CPI goods) support healthy housing demand.  
- **Cost pressures (PPI_ALL)** and **fast import growth** dampen local housing demand.  

**Key Insight**  
While children (0–18) negatively affected prices in the absolute model, their **relative growth** strongly drives housing demand.  
This suggests that **family formation (new parents buying homes)** is a critical long-term driver of house-price increases. 

**Model Reults**
Linear Regression, Ridge, and Lasso again showed similar performance, with test RMSE values around 0.03 to 0.04 (percent relative increase). SVM performed poorly, with a very high RMSE, making it unsuitable for comparison. In contrast, the Decision Tree model achieved a lower RMSE than all other models, making it the best fit for this dataset.
<img width="1189" height="790" alt="image" src="https://github.com/user-attachments/assets/1e1c936b-ede0-4192-9e24-2599facbb382" />


---

##  Key Takeaways  

### Demographics  
1. States with rising **newborn and child populations** see faster house-price growth, indicating families with new borns drive growth  
2. States attracting **65+ populations** experience higher prices — retiree in-migration is a major factor.  
3. Large **working-age populations (26–54)** are associated with high prices overall, though relative growth has less impact than families or retirees.  

### Economic Factors  
1. **Income growth** is the strongest driver of house prices.  
2. **Inflation (CPI of goods)** is closely tied to rising housing costs.  
3. States with **high imports** tend to have lower house prices, reflecting weaker local demand.  

---

##  Next Steps  

###  Add More Features  
- Building permits, rental prices, unemployment rates, migration flows, new-home supply, property-tax rates.  

###  Improve Models  
- Compare/tune Ridge, Lasso, ElasticNet, Gradient Boosting, Random Forest, XGBoost.  
- Use **cross-validation** for better accuracy and generalization.  

###  Adjust for Inflation  
- Correct features for **inflation adjustment** before modeling.  
- Ensures insights are based on **real values** rather than nominal growth.  

---

##  Project Outline  

- [Notebook 1 – Data Preparation]()  
- [Notebook 2 – EDA & Visualization]()  
- [Notebook 3 – Modeling & Results]()  

---

##  Contact & Further Information  

For questions or collaboration, please reach out to the project authors.  
