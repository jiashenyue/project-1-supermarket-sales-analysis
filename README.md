# Project 1 Supermarket Sales Analysis

Shenyue Jia

## Business Problem:
- We have a dataset showing item sales and a number of related metrics of the outlets where the items are sold. 
- We want to use these metrics to investigate which factors are the most important determinants of item sales. 
- Eventually, we want to construct a model to estimate the item sales based on the metrics available from our dataset.
## Data:
The salary data for data scientist is collected and prepared by [Analytics Vidhya](https://datahack.analyticsvidhya.com/contest/practice-problem-big-mart-sales-iii/).

### Data Dictionary:
Variable Name  | Description
-------------------|------------------
Item_Identifier	| Unique product ID
Item_Weight	| Weight of product
Item_Fat_Content	| Whether the product is low fat or regular
Item_Visibility	| The percentage of total display area of all products in a store allocated to the particular product
Item_Type	| The category to which the product belongs
Item_MRP	| Maximum Retail Price (list price) of the product
Outlet_Identifier	| Unique store ID
Outlet_Establishment_Year	| The year in which store was established
Outlet_Size	| The size of the store in terms of ground area covered
Outlet_Location_Type	| The type of area in which the store is located
Outlet_Type	| Whether the outlet is a grocery store or some sort of supermarket
Item_Outlet_Sales	| Sales of the product in the particular store. This is the target variable to be predicted.

## EDA
![png](https://github.com/jiashenyue/project-1-supermarket-sales-analysis/blob/main/PNG/EDA-item-outlet-sales.png)

## Linear Regression Model
### Model Performance

Data  | $R^2$ | RMSE
------|--------|-----------
Training	| 0.56 | 1114.97
Testing | 0.56 | 1164.43

### Feature Importance
- We excluded the `Outlet_Identifier` columns as they are IDs of outlets and are less meaningful to describe the pattern in the big picture

![png](https://github.com/jiashenyue/project-1-supermarket-sales-analysis/blob/main/PNG/coeff-no-outlet-id.png)

**Top 3 most important features**

Feature  | Feature Importance 
------|--------------
`Outlet_Type_Supermarket Type3`	| 579.3975
`Outlet_Type_Supermarket Type1` | 375.2792
`Outlet_Size_Medium` | 361.1267

1. Outlet_Type_Supermarket Type3
 - Being in the Type 3 Supermarket group increases the item outlet sales by 579.3975.
2. Outlet_Type_Supermarket Type1
 - Being in the Type 1 Supermarket group increases the item outlet sales by 375.2792.
3. Outlet_Size_Medium
 - Being in the Medium Supermarket type increases the item outlet sales by 361.1267.

## Tree-based Model
### Model Performance

Data  | $R^2$ | RMSE
------|--------|-----------
Training	| 1.00 | 0.00
Testing | 0.21 | 1556.79

### Feature Importance

- We excluded the `Outlet_Identifier` columns as they are IDs of outlets and are less meaningful to describe the pattern in the big picture

![png](https://github.com/jiashenyue/project-1-supermarket-sales-analysis/blob/main/PNG/coeff-no-outlet-id-dec-tree.png)

**Top 5 most important features**

Feature  | Feature Importance 
------|--------------
`Item_MRP`	| 0.4420
`Outlet_Type_Grocery Store` | 0.1986
`Item_Visibility` | 0.1986
`Item_Weight` | 0.0624
`Outlet_Type_Supermarket Type3` | 0.0563

## Feature Importance by `SHAP`
### Results from `SHAP` and model

Rank | Model | SHAP
-----| -----| ----
1 | `Item_MRP`| `Item_MRP`
2 | `Outlet_Type_Grocery Store` | `Outlet_Type_Grocery Store`
3 | `Item_Visibility` | `Outlet_Type Supermarket Type3`
4 | `Item_Weight` | `Item_Visibility`
5 | `Outlet_Type_Supermarket Type3` | `Item_Weight`

- In previous analysis, we excluded features derived from `Outlet_Identifier` as they are the IDs of outlets
- The features displayed below do not include any features derived from `Outlet_Identifier`
- We can see the top 5 most important features derived from the model and by SHAP are slightly different (in orders)
    - `Item_Visibility` ranked #3 in the result from model, but ranked #4 in results by SHAP
    - `Item_Visibility` ranked #4 in the result from model, but ranked #5 in results by SHAP
    - `Outlet_Type_Supermarket Type3` ranked #5 in the result from model, but ranked #3 in results by SHAP

![png](https://github.com/jiashenyue/project-1-supermarket-sales-analysis/blob/main/PNG/Summary_Plot_Bar.png)

### Indication of Top 3 Most Important Features

![png](https://github.com/jiashenyue/project-1-supermarket-sales-analysis/blob/main/PNG/Summary_Plot_dot.png)

- The Top 3 most important features according to SHAP are `Item_MRP`, `Outlet_Type_Grocery Store`, and `Outlet_Type Supermarket Type3`
    - `Item_MRP`
        - Positive values (red dots on the right). The greater the maximum retail price of an item, the higher the model will predict as the item outlet sales.
        - Negative values (blue dots on the left). The smaller the maximum retail price of an item, the lower the model will predict as the item outlet sales.
    - `Outlet_Type_Grocery Store`
        - Positive values (red dots on the left). If items are more likely to be sold in Grocery Store, it tend to have a lower value in sales.
        - Negative values (blue dots on the right). If items are less likely to be sold in Grocery Store, it tend to have a higher value in sales.
    - `Outlet_Type Supermarket Type3`
        - Positive values (red dots on the right). If items are more likely to be sold in Type3 Supermarket, it tend to have a higher value in sales.
        - Negative values (blue dots on the left). If items are less likely to be sold in Type3 Supermarket, it tend to have a lower value.

 
