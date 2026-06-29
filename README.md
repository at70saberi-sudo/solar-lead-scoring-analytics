# Customer Analytics and Purchase Prediction for Rural Solar Energy Adoption in Darab, Iran

## Project Overview

This project analyzes historical customer and sales data from a small solar energy company in Darab, Iran. The company designs and installs customized off-grid solar systems for rural farms, garden houses, agricultural lands, and some rural houses around Darab.

The main goal of the project is to understand customer purchase behavior and identify which customer profiles are more likely to purchase a solar system. The project combines exploratory data analysis, customer segmentation, interpretable machine learning, lead scoring, and Power BI dashboard development to support better sales communication and decision-making.

This is not a production lead rejection system. In a small city like Darab, the company does not have so many leads that low-score customers should be ignored. Instead, the model and dashboard are used as decision-support tools to help the company understand customer needs, buying intent, and follow-up patterns.

## Business Context

The villages around Darab generally have electricity. However, many farms, garden houses, and agricultural lands around these villages are not connected to the electricity grid or have unstable electricity access.

For this reason, electricity access in this project is related to the specific property, not necessarily to the village itself. A village may have electricity, while a nearby farm or garden house may still need an independent off-grid solar system.

The company does not sell fixed solar packages. Each solar system is designed based on the customer’s actual needs, such as appliance requirements, daily electricity demand, battery needs, water pump use, site conditions, and budget considerations.

The company workflow can be summarized as:

```text
Customer inquiry -> Technical assessment -> Custom system design -> Price quotation -> Purchase or no purchase
```

## Business Questions

This project aims to answer the following business questions:

1. What customer characteristics are associated with higher purchase likelihood?
2. Do farms, garden houses, and rural houses behave differently?
3. How do electricity access, appliance needs, energy demand, and quoted price relate to purchase decisions?
4. Which customer groups show stronger buying intent?
5. How can the company use these insights to improve future sales conversations and follow-up strategies?

## Repository Structure

```text
solar-lead-scoring-analytics/
│
├── README.md
├── requirements.txt
│
├── data/
│   ├── lead_intake.csv
│   ├── assessment_sales.csv
│   └── solar_leads_merged.csv
│
├── outputs/
│   └── solar_leads_scored.csv
│
├── notebooks/
│   ├── Exploratory Data Analysis.ipynb
│   ├── segmentation.ipynb
│   └── Purchase Prediction.ipynb
│
└── dashboard/
    ├── solar_customer_analytics_dashboard_final.pbix
    ├── dashboard_executive_overview.png
    └── dashboard_customer_analysis.png
```

## Dataset Description

The project uses two related tables that are merged using `lead_id`.

### Table 1: `data/lead_intake.csv`

This table represents the first customer inquiry.

| Column                   | Description                                                     |
| ------------------------ | --------------------------------------------------------------- |
| `lead_id`                | Unique customer lead ID                                         |
| `village`                | Village or nearby area related to the property                  |
| `distance_from_darab_km` | Distance from Darab in kilometers                               |
| `property_type`          | Type of property                                                |
| `usage_purpose`          | Main reason for needing electricity                             |
| `electricity_access`     | Whether the property has no electricity or unstable electricity |
| `lead_source`            | How the customer contacted or found the company                 |

### Table 2: `data/assessment_sales.csv`

This table represents the technical assessment and sales outcome.

| Column                            | Description                                                           |
| --------------------------------- | --------------------------------------------------------------------- |
| `lead_id`                         | Unique customer lead ID                                               |
| `required_appliances`             | Appliances requested by the customer                                  |
| `estimated_daily_energy_need_kwh` | Estimated daily electricity demand                                    |
| `quoted_price_m_toman`            | Quoted price in million toman                                         |
| `purchased`                       | Target variable, where 1 means purchased and 0 means did not purchase |

The merged dataset is saved as:

```text
data/solar_leads_merged.csv
```

The final scored output is saved as:

```text
outputs/solar_leads_scored.csv
```

## Main Variables

| Variable                          | Meaning                                                       |
| --------------------------------- | ------------------------------------------------------------- |
| `property_type`                   | Farm, garden house, or rural house                            |
| `usage_purpose`                   | Agriculture, seasonal garden, mixed use, or primary residence |
| `electricity_access`              | None or unstable                                              |
| `required_appliances`             | Appliances such as lights, TV, fridge, pump, and AC           |
| `estimated_daily_energy_need_kwh` | Estimated daily electricity need                              |
| `quoted_price_m_toman`            | Quoted price in million toman                                 |
| `purchased`                       | Binary purchase outcome                                       |
| `lead_score`                      | Model-based purchase likelihood score from 0 to 100           |
| `priority_level`                  | Follow-up level based on lead score: Low, Medium, or High     |

## Project Workflow

The project was developed in eight main phases:

1. Business understanding and data preparation
2. Exploratory data analysis
3. Feature engineering
4. Customer segmentation
5. Predictive modeling
6. Model interpretation
7. Purchase probability and lead scoring
8. Power BI dashboard development

## 1. Business Understanding and Data Preparation

The first step was to understand the company workflow and the business meaning of each variable. Since the company designs customized systems instead of selling fixed packages, the data had to be interpreted based on real customer needs and the sales process.

The two datasets were merged using `lead_id`.

The data preparation process included:

* Loading customer intake and sales assessment data
* Merging the two tables
* Checking missing values
* Correcting data types
* Preparing categorical and numerical variables
* Creating a clean dataset for analysis, segmentation, modeling, and dashboard development

This step was important because the project is not only a technical prediction task. The data needed to be understood in relation to the company’s real business process.

## 2. Exploratory Data Analysis

Exploratory data analysis was used to understand purchase patterns and customer behavior.

The analysis focused on:

* Purchase distribution
* Property type
* Usage purpose
* Electricity access
* Lead source
* Required appliances
* Estimated daily energy demand
* Quoted price
* Distance from Darab
* Village-level patterns

The dataset contains 300 leads. Out of these, 180 customers purchased a solar system and 120 did not purchase. This gives an overall purchase rate of around 60%.

The EDA showed that purchase behavior differs across property types. Farm customers showed stronger purchase behavior compared with garden house customers. This is reasonable because farms often have more practical and urgent electricity needs, especially when agricultural activity or water pump use is involved.

Customers with no electricity access also showed stronger purchase behavior than customers with unstable electricity. This suggests that complete lack of electricity creates a stronger need for an independent solar solution.

The relationship between estimated daily energy need and quoted price was positive. This matches the business context because each system is customized. Higher energy demand usually requires a larger system, more battery capacity, and therefore a higher quoted price.

The EDA notebook is available here:

```text
notebooks/Exploratory Data Analysis.ipynb
```

## 3. Feature Engineering

The appliance information was transformed into machine-learning features so that the models could better understand customer needs.

The `required_appliances` column includes appliance needs such as lights, TV, fridge, pump, and AC. These appliance requirements were converted into encoded features that could be used in machine learning models.

This step is important because appliance needs are directly connected to system size, technical complexity, price, and purchase urgency. For example, a customer who needs a pump for agricultural use may have a stronger practical need than a customer who only needs basic lighting.

## 4. Customer Segmentation

Customer segmentation was used to explore whether customers naturally form meaningful business groups.

The aim of segmentation was not to predict purchase directly. Instead, segmentation was used to understand customer profiles and compare purchase behavior after the segments were created.

Since the dataset contains both numerical and categorical variables, K-Prototypes was selected as the final segmentation method. The target variable `purchased` was excluded during clustering to avoid target leakage.

Four customer segments were identified.

| Segment   | Main Profile                                | Business Interpretation                                          |
| --------- | ------------------------------------------- | ---------------------------------------------------------------- |
| Segment 0 | Agriculture-focused, high-demand customers  | Strong core market with high purchase potential                  |
| Segment 1 | Nearby seasonal garden customers            | Moderate purchase potential, may need more explanation           |
| Segment 2 | Remote seasonal customers with lower demand | Lower purchase rate, but still useful for targeted communication |
| Segment 3 | Remote but high-need customers              | Strong purchase potential despite longer distance                |

A key insight from segmentation is that distance from Darab should not be used alone as a negative signal. Some remote customers still showed strong purchase potential when they had practical electricity needs, pump-related appliance needs, or higher energy demand.

This means that the company should evaluate customers based on the combination of distance, property type, usage purpose, electricity access, appliance needs, and estimated energy demand.

The segmentation notebook is available here:

```text
notebooks/segmentation.ipynb
```

## 5. Predictive Modeling

The predictive modeling phase aimed to estimate the likelihood that a customer would purchase a solar system.

The target variable was:

```text
purchased
```

Three classification models were tested:

* Logistic Regression
* Decision Tree
* Random Forest

The dataset was split into training and testing sets using a 70/30 split. The split was stratified to keep the proportion of purchased and non-purchased customers similar in both sets.

Logistic Regression was selected as the final model because it provided a good balance between predictive performance and interpretability. The model was trained using `solver="liblinear"`, which is suitable for binary classification problems and small datasets.

The model achieved around 72.2% test accuracy and showed good recall for purchased customers. This is useful for the business because the company wants to identify customers with stronger buying intent and improve follow-up decisions.

The purpose of the model is not to automatically decide which customers should be accepted or rejected. Instead, it provides a purchase probability score that can support sales communication and decision-making.

The purchase prediction notebook is available here:

```text
notebooks/Purchase Prediction.ipynb
```

## 6. Model Interpretation

The Logistic Regression model was interpreted using its coefficients.

The features positively associated with purchase likelihood included:

* Farm properties
* Agriculture-related usage
* Pump-related appliance needs
* No electricity access
* Referral leads
* Returning customers

These factors suggest that customers with stronger practical electricity needs, higher urgency, and greater trust in the company are more likely to purchase.

The features negatively associated with purchase likelihood included:

* Seasonal garden use
* Light-only appliance needs
* Garden house properties
* Instagram leads
* Local advertisement leads

These customers are not unimportant. However, they may be more uncertain, more price-sensitive, or still exploring options. They may need a different sales approach, more explanation, or a simpler customized system design.

## 7. Purchase Probability and Lead Scoring

The final Logistic Regression model was used to estimate purchase probability for each customer. These probabilities were converted into lead scores from 0 to 100.

The scores were grouped into three follow-up levels.

| Follow-up Level | Business Meaning                                        |
| --------------- | ------------------------------------------------------- |
| High            | Strong need and high purchase likelihood                |
| Medium          | Some purchase potential, may need more explanation      |
| Low             | More uncertain, possibly price-sensitive or exploratory |

The actual purchase rate increased clearly from Low to Medium to High follow-up groups. This shows that the model can meaningfully separate customers by purchase likelihood.

However, a low score does not mean that the customer should be ignored. Instead, it suggests that the customer may need a different type of communication.

For example:

* High-score customers may need fast and detailed technical follow-up.
* Medium-score customers may need clarification about system size, price, and benefits.
* Low-score customers may need education, affordability discussion, or simpler customized system options.

The scored dataset is saved here:

```text
outputs/solar_leads_scored.csv
```

## 8. Power BI Dashboard Development

A Power BI dashboard was developed to translate the analytical and modeling results into a clear business-facing tool.

The dashboard helps communicate the main findings in a visual and accessible way. It allows the company to explore customer purchase behavior, compare customer groups, review follow-up levels, and understand the relationship between energy demand and quoted price.

The final dashboard contains two pages:

### Page 1: Executive Overview

The first page provides a high-level summary of the project results.

Main KPI cards:

* Total Leads
* Purchase Rate
* Average Lead Score
* Average Quoted Price

Main visuals:

* Total Leads by Follow-up Level
* Purchase Rate by Follow-up Level
* Purchase Rate by Property Type
* Purchase Rate by Required Appliances

Interactive slicers:

* Follow-up Level
* Property Type
* Electricity Access

This page gives a quick overview of customer volume, overall purchase behavior, lead score distribution, and key purchase patterns.

### Page 2: Customer Analysis

The second page focuses on customer behavior and sales-related patterns.

Main visuals:

* Purchase Rate by Usage Purpose
* Purchase Rate by Lead Source
* Average Energy Need by Property Type
* Energy Need vs Quoted Price by Property Type

This page helps explain how customer purpose, lead source, property type, energy demand, and quoted price relate to purchase decisions.

The scatter plot compares average energy need and average quoted price by property type. This supports the business interpretation that quoted price should be understood together with energy demand, since larger energy needs usually require larger and more expensive customized solar systems.

The Power BI dashboard file is available here:

```text
dashboard/solar_customer_analytics_dashboard_final.pbix
```

## Dashboard Preview

The Power BI dashboard presents the main analytical findings in a clear and business-oriented format. It includes two pages: Executive Overview and Customer Analysis.

The Executive Overview page summarizes key indicators such as total leads, purchase rate, average lead score, and average quoted price. It also shows purchase patterns by follow-up level, property type, and required appliances.

![Executive Overview](dashboard/dashboard_executive_overview.png)

The Customer Analysis page focuses on customer behavior and sales-related patterns, including purchase rate by usage purpose and lead source, average energy need by property type, and the relationship between energy need and quoted price.

![Customer Analysis](dashboard/dashboard_customer_analysis.png)

## Key Findings

The main findings of the project are:

1. Farm customers are more likely to purchase than garden house customers.
2. Agriculture-related usage is strongly connected to higher purchase likelihood.
3. Customers with no electricity access show stronger buying intent.
4. Pump-related appliance needs are important purchase drivers.
5. Referral leads and returning customers have higher purchase likelihood.
6. Higher quoted prices should be interpreted together with energy demand and appliance needs.
7. Distance from Darab does not automatically mean low purchase potential.
8. Customer scoring is useful for follow-up strategy, but not for rejecting leads.
9. The Power BI dashboard helps translate analytical findings into practical business insights.

## Business Recommendations

Based on the analysis, the company can improve its sales process in several ways.

### 1. Prioritize practical-need customers for faster follow-up

Customers with farm properties, agriculture use, pump needs, or no electricity access often show stronger buying intent. These customers may benefit from faster technical assessment and clearer quotation.

### 2. Use different communication for seasonal garden customers

Seasonal garden customers may be less urgent or more price-sensitive. They may need simpler explanations, smaller customized system options, and more focus on comfort, reliability, and long-term value.

### 3. Do not reject remote customers only because of distance

Some remote customers have strong purchase potential, especially when they have high energy demand or pump-related needs. Distance should be evaluated together with customer need.

### 4. Use referral and returning customers as trust signals

Referral leads and returning customers are valuable because they already have some level of trust in the company. These customers may need a more direct and confident follow-up approach.

### 5. Use the model and dashboard as decision support, not automation

The lead score and dashboard should help the company adapt its sales communication. They should not replace human judgment or automatically filter out customers.

## Tools and Technologies

The project was developed using Python and Power BI.

Main Python libraries:

* pandas
* numpy
* matplotlib
* seaborn
* scikit-learn
* kmodes
* scipy

Dashboard tool:

* Power BI

Main techniques used:

* Data cleaning
* Data merging
* Exploratory data analysis
* Categorical encoding
* Feature engineering
* Customer segmentation
* Classification modeling
* Logistic Regression interpretation
* Purchase probability scoring
* Dashboard design
* Business insight communication

## Project Files

| File                                                      | Description                                                        |
| --------------------------------------------------------- | ------------------------------------------------------------------ |
| `data/lead_intake.csv`                                    | Customer inquiry data                                              |
| `data/assessment_sales.csv`                               | Technical assessment and sales outcome data                        |
| `data/solar_leads_merged.csv`                             | Final merged dataset used for analysis and modeling                |
| `outputs/solar_leads_scored.csv`                          | Dataset with purchase probability, lead score, and follow-up level |
| `notebooks/Exploratory Data Analysis.ipynb`               | EDA and business pattern analysis                                  |
| `notebooks/segmentation.ipynb`                            | Customer segmentation analysis                                     |
| `notebooks/Purchase Prediction.ipynb`                     | Machine learning models and purchase probability scoring           |
| `dashboard/solar_customer_analytics_dashboard_final.pbix` | Final Power BI dashboard                                           |
| `dashboard/dashboard_executive_overview.png`              | Screenshot of the Executive Overview dashboard page                |
| `dashboard/dashboard_customer_analysis.png`               | Screenshot of the Customer Analysis dashboard page                 |
| `requirements.txt`                                        | Python dependencies used in the project                            |

## Model Performance Summary

| Model               | Result                                                                     |
| ------------------- | -------------------------------------------------------------------------- |
| Logistic Regression | Best balance between accuracy and interpretability                         |
| Decision Tree       | Lower performance and less stable                                          |
| Random Forest       | Better than Decision Tree, but less interpretable than Logistic Regression |

The final selected model was Logistic Regression because the business goal required both prediction and interpretation.

## Conclusion

This project shows how customer analytics, interpretable machine learning, and dashboard design can help a small rural solar energy company understand customer behavior and improve sales decision-making.

The analysis showed that purchase likelihood is connected to practical electricity need, property type, usage purpose, appliance requirements, electricity access, and customer trust signals such as referrals and returning customers.

The final Logistic Regression model estimates purchase probability and supports follow-up planning. The Power BI dashboard translates these findings into a clear visual format for business use.

Overall, the project demonstrates how data science can support practical decision-making without replacing human judgment.
