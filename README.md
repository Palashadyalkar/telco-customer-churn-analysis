# Telco Customer Churn Analysis

An exploratory analysis of 7,043 telecom customers to find out who is leaving, at what stage, and which account and service characteristics are linked to it.

**Tools:** Python, pandas, NumPy, matplotlib, seaborn, Jupyter Notebook

---

## Summary

26.54% of the customers in this dataset churned. The churn is not spread evenly. It is concentrated in a few clearly identifiable groups:

| Segment | Churn rate |
|---|---|
| Month-to-month contract | 42.71% |
| First year of tenure (0-12 months) | 47.44% |
| Fiber optic internet | 41.89% |
| No Online Security (internet customers) | 41.77% |
| No Tech Support (internet customers) | 41.64% |

By comparison, customers on a two-year contract churn at 2.83%, and customers who have been with the company for more than 36 months churn at 11.93%.

---

## Business Problem

Retaining an existing customer is generally cheaper than acquiring a new one, so a telecom business needs to know where it is losing customers before it decides where to spend on retention. This project looks at two questions:

1. Which customer segments have the highest churn rate?
2. Which factors in the data are most closely associated with customers leaving?

---

## Dataset

- **Source:** Publicly available Telco Customer Churn dataset (`WA_Fn-UseC_-Telco-Customer-Churn.csv`)
- **Size:** 7,043 customers, 21 columns
- **Target column:** `Churn` (Yes/No), which records whether the customer left
- **Columns used in the analysis:** `Contract`, `tenure`, `InternetService`, `OnlineSecurity`, `TechSupport`, `MonthlyCharges`, `TotalCharges`, `Churn`

This is a public practice dataset, not data from a company I worked with. The findings describe this dataset only.

---

## Approach

### 1. Data inspection (Steps 2-3)
Before changing anything I checked the dimensions, column types and the first few records, to understand the structure and spot obvious problems.

### 2. Data cleaning (Step 4)
- **`TotalCharges` was stored as text.** A few entries were blank spaces, which stopped pandas from reading the column as numbers. I converted it to numeric, which turned the blanks into missing values.
- **There were 11 missing values, and all 11 belong to customers with a tenure of 0.** These are customers who had just signed up and had not been billed yet, so I filled them with 0. Dropping the rows or filling them with an average would have misrepresented what these customers had actually paid.
- **`Churn` was converted from Yes/No to 1/0.** With a 1/0 column, the mean of the column is directly the churn rate, which is what the rest of the analysis relies on.
- I confirmed afterwards that no missing values remained.

### 3. Visual exploration (Steps 5-6)
I plotted the data before calculating anything, to see where patterns might be:
- Count of churned vs retained customers
- Churn count by contract type
- Box plots of tenure and monthly charges, split by churn

The box plots show that customers who churned generally have a shorter tenure and higher monthly charges than customers who stayed.

### 4. Dropping `customerID` (Step 7)
It is a unique label for each row and says nothing about why a customer leaves.

### 5. Baseline churn rate (Step 8)
I calculated the overall churn rate (26.54%) so that every segment can be judged against the average. A segment churn rate only means something relative to that baseline.

### 6. Segment-level churn rates (Steps 9-10)
Using `groupby`, I calculated the churn rate for each contract type, internet service type and tenure group. I grouped tenure into three bands (0-12 months, 13-36 months, more than 36 months) because three bands are easier to act on than a raw count of months. The cut-offs are my own choice.

I also compared customers with and without Online Security and Tech Support. For this comparison I excluded customers who have no internet service, because they cannot have these add-ons and would distort the result.

---

## Findings

**Overall:** 26.54% of customers churned.

### Contract type

| Contract | Churn rate |
|---|---|
| Month-to-month | 42.71% |
| One year | 11.27% |
| Two year | 2.83% |

Contract length is the strongest pattern in the data. Month-to-month customers leave at more than fifteen times the rate of two-year customers.

### Customer tenure

| Tenure group | Churn rate |
|---|---|
| New (0-12 months) | 47.44% |
| Mid (13-36 months) | 25.54% |
| Loyal (more than 36 months) | 11.93% |

Nearly half of customers in their first year leave, and the risk falls steadily as tenure increases.

### Internet service

| Internet service | Churn rate |
|---|---|
| Fiber optic | 41.89% |
| DSL | 18.96% |
| No internet service | 7.40% |

Fiber optic customers churn at more than twice the rate of DSL customers.

### Add-on services (customers with internet service only)

| Service | Without | With |
|---|---|---|
| Online Security | 41.77% | 14.61% |
| Tech Support | 41.64% | 15.17% |

Customers who have either service churn at roughly a third of the rate of those who do not.

---

## Recommendations

These follow from the patterns above. They are suggestions to test, not guaranteed fixes.

1. **Encourage month-to-month customers to move to longer contracts.** A discount or a free add-on in exchange for a 1-year or 2-year plan is worth testing, since this is where most churn sits.
2. **Put more effort into the first year.** Churn is highest in the first 12 months, so onboarding follow-ups or a first-year loyalty offer could reduce early exits.
3. **Investigate the Fiber optic plans.** Churn is more than double that of DSL. The data cannot say why, so the next step would be to look at pricing, service quality and competitor offers, and to ask churned customers directly.
4. **Promote Online Security and Tech Support.** Customers who have them churn far less, so bundling them into plans or offering a trial period is worth trying.

---

## Limitations

- **Association, not causation.** The analysis shows which groups churn more. It does not prove that a factor causes churn. For example, customers on long contracts may simply be the ones who were already satisfied, so offering contracts may reduce churn by less than the numbers suggest. The same applies to add-on services.
- **One factor at a time.** The segments overlap. A new customer on a month-to-month fiber plan sits in several high-risk groups at once, and I have not analysed those combinations here.
- **No predictive model.** The project explains churn in the existing data. It does not forecast which customers will leave next.
- **Tenure bands are my own cut-offs.** Different boundaries would shift the percentages slightly.

---

## How to Run

1. Download or clone this repository.
2. Install the libraries: `pip install pandas numpy matplotlib seaborn jupyter`
3. Keep the notebook and the CSV file in the same folder.
4. Open `Customer_Churn_Analysis.ipynb` in Jupyter and run all cells.

---

## Repository Contents

```
Customer_Churn_Analysis.ipynb          Full analysis with code, comments and outputs
WA_Fn-UseC_-Telco-Customer-Churn.csv   Dataset
README.md                              Project documentation
```

---

## Author

**Palash** | Data Analyst

LinkedIn: [linkedin.com/in/palashadyalkar](https://www.linkedin.com/in/palashadyalkar)
