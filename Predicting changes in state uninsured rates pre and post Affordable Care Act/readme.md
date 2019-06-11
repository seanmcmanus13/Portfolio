# **Predicting state change in insurance rate post Affordable Care Act - A supervised learning approach**

## Background:
The Affordable Care Act (ACA) lead to 20 million people gaining insurance, but the effect of this piece of legislation varied greatly by state:
**![](https://lh4.googleusercontent.com/RN_Qbfi2PileNosi_tm3moyeDUs5Bm-QmAHfGtFEVmSwPi-M3giEM12GWbTn3U4yMa4TGM3UpJYM5VMS1Mf07z_6q9DFtMaB32bC3WIH1qVJzqF5HckLdxVuL7koGOIeEH7RsCZgG_A)**
This project aimed to answer two questions:
1.  Can we accurately predict the effect size of the ACA , as measured by change in % individuals uninsured, using feature variables that describe that state?
2.  Can we establish the metrics that have the biggest effect on the change in % individuals uninsured?

## **The data:**
The data contained in this folder includes personally collated data from the Department of Health and Human Services and The Henry J Kaiser Family Foundation

## **Approach (and disclaimer):**
A number of supervised learning model were utilized in order to establish the best model to answer the questions posed above. This project by en large served a pedagogical purpose as practice applying a diverse range of models and some of these models, are not suited to the size of this dataset (notably Decision Trees, Random Forest and Gradient Boost Regression). That being said the results presented in the multivariable linear regression model are consistent and seem to present a generalizable model.

## Results:
Multivariable Linear Regression resulted in a **Training R2 of 0.758  and a Test R2  of 0.702**.
The variables that were used to generate this model were:
-   State Health Expense Per Capita
-  State Ratio of individuals over 200% federal poverty line (FPL) to individuals under 200% of FPL
-   State Uninsured Rate 2010
-   State Political Lean
-   State Medicaid Expansion Status

**![](https://lh5.googleusercontent.com/n6VHVT2wF6hru8nz8KhwXJMCVO-5xtbo6Yt3B93Ko5qNCII0_Z5zMPONpqYK2xIj54xTGZrmGyTpZ8MwWduf5G1nKRa9pBLzHKYphy3sBuj_UvKySMX4vKmFihPd6uGEZrrjje3nXwM)**
**![](https://lh6.googleusercontent.com/HQrn1Gy3nDyiK5eHwVQpAS8hX4NYMqmiLUc94n3oJ-xw-x1l9YsOKxU39AgoUh1au7WWA9pAaWHqLVd32EA-XKJWmX_u_OJc6LTLdvtFwWbBXh0lQdWg8PrpTzpmDtzlmuo2JLaT5EY)**

The relative effect sizes of each of these features and discussion on their inclusion in the model are discussed further [here](https://docs.google.com/presentation/d/1ZHhH-dG7iSeXoW0GF0U0a1F3fH-6yshIHBsN9MjwDCs/edit?usp=sharing).


For permission to use any of the analyses and/or images contained herein, please contact me via linkedin:
https://www.linkedin.com/in/sean-mcmanus-9a092a17
