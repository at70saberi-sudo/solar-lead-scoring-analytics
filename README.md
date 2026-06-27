\# Lead Scoring and Customer Analytics for Rural Solar Energy Adoption



\## Project Overview



This project is an end-to-end data science portfolio project focused on customer analytics and lead scoring for rural solar energy adoption near Darab, Iran.



The business case is based on Mehr Energy Company in Darab, a small solar energy installation company that provides off-grid solar systems for farms, garden houses, and rural properties. The goal is to help the sales team identify which customer leads are more likely to purchase a solar system and which leads should be prioritized for follow-up.



\## Business Problem



Solar installation companies often receive leads from different customer types, but not all leads have the same purchase potential. Some customers have urgent energy needs, while others may only need small seasonal systems.



The main business question is:



\*\*Which customer leads are most likely to purchase a solar system, and how can the sales team prioritize them more effectively?\*\*



\## Dataset



The project uses two structured datasets:



\### Lead Intake Data



Customer and property information:



\* `lead\_id`

\* `village`

\* `distance\_from\_darab\_km`

\* `property\_type`

\* `usage\_purpose`

\* `electricity\_access`

\* `lead\_source`



\### Assessment and Sales Data



Technical assessment and sales outcome information:



\* `lead\_id`

\* `required\_appliances`

\* `estimated\_daily\_energy\_need\_kwh`

\* `quoted\_price\_m\_toman`

\* `purchased`



The datasets were merged using `lead\_id` and used for exploratory analysis, customer segmentation, purchase prediction, and lead scoring.



\## Methodology



The project follows a full data science workflow:



1\. Data understanding and preparation

2\. Exploratory data analysis

3\. Customer segmentation

4\. Purchase prediction

5\. Model interpretation

6\. Lead scoring

7\. Dashboard preparation



\##



\## Customer Segmentation



Customer segmentation was performed using K-Prototypes because the dataset contains both numerical and categorical variables.



The `purchased` column was not used during clustering. It was only used afterward to interpret purchase rates across segments.



Four customer segments were identified:



\* Core Agricultural Household-Energy Leads

\* Nearby Seasonal Basic-Energy Leads

\* Remote Seasonal Pump-Light Leads

\* Remote High-Need Pump System Leads



The segmentation showed that high-need farm and agriculture-related customers are more valuable sales groups, even when they are located farther from Darab.



\## Purchase Prediction



Purchase prediction was treated as a supervised binary classification problem.



Target variable:



\* `purchased`

\* `0` = did not purchase

\* `1` = purchased



Three models were tested:



| Model               | Test Accuracy |

| ------------------- | ------------- |

| Logistic Regression | 71.1%         |

| Decision Tree       | 55.6%         |

| Random Forest       | 63.3%         |



Logistic Regression was selected as the final model because it achieved the best test accuracy and provided interpretable coefficients for business analysis.



\## Lead Scoring



The final Logistic Regression model was used to generate purchase probabilities for each lead.



These probabilities were converted into lead scores:



```text

lead\_score = purchase\_probability × 100

```



Each lead was assigned a priority level:



| Lead Score Range | Priority Level |

| ---------------- | -------------- |

| 0 to 40          | Low            |

| 40 to 70         | Medium         |

| 70 to 100        | High           |



The final scored dataset helps the sales team identify which leads should be contacted first.



\## Key Insights



\* Farm and agriculture-related leads showed stronger purchase potential.

\* Customers with no electricity access and pump-related appliance needs were more likely to purchase.

\* Referral and returning customer leads had stronger purchase signals.

\* Seasonal garden and light-only leads were generally lower priority.

\* Distance from Darab should not be used as the main filter for sales priority.

\* Remote leads can still be valuable when they have strong practical energy needs.



\## Dashboard Preparation



The final scored dataset was prepared for a Power BI dashboard. The dashboard is designed to show:



\* Total leads

\* Purchase rate

\* Average quoted price

\* High-priority leads

\* Leads by priority level

\* Purchase rate by property type

\* Purchase rate by required appliances

\* Top leads for sales follow-up



\## Tools and Libraries



\* Python

\* pandas

\* NumPy

\* matplotlib

\* seaborn

\* scikit-learn

\* kmodes

\* Jupyter Notebook

\* Power BI



\## Repository Structure



solar-lead-scoring-darab/

│

├── data/

│   ├── lead\_intake.csv

│   └── assessment\_sales.csv

│

├── notebooks/

│

│   ├── 01\_exploratory\_data\_analysis.ipynb

│   ├── 02\_customer\_segmentation.ipynb

│   └── 03\_purchase\_prediction\_lead\_scoring.ipynb

│

├── outputs/

│   ├── solar\_leads\_merged.csv

│   ├── solar\_leads\_segmented.csv

│   └── solar\_leads\_scored.csv

│

├── dashboard/

│   └── powerbi\_dashboard\_screenshot.png

│

├── requirements.txt

└── README.md



\## How to Run



1\. Clone the repository:



```bash

git clone https://github.com/your-username/solar-lead-scoring-darab.git

```



2\. Install the required libraries:



```bash

pip install -r requirements.txt

```



3\. Open Jupyter Notebook:



```bash

jupyter notebook

```



4\. Run the notebooks in order:



```text

1\_exploratory\_data\_analysis.ipynb

2\_customer\_segmentation.ipynb

3\_purchase\_prediction\_lead\_scoring.ipynb

```



\## Project Outcome



This project demonstrates how data science can support sales decision-making in a rural renewable energy context. By combining customer segmentation, purchase prediction, model interpretation, and lead scoring, the project translates customer data into practical business actions.



