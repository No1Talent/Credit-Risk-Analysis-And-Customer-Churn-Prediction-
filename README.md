# Credit Risk Analysis And Customer Churm Prediction

## Business Understanding
In a non-bank financial services industry context, many individuals face difficulties obtaining loans, primarily due to inadequate or nonexistent credit histories. Unfortunately, this vulnerable segment often falls prey to untrustworthy lenders. As aspiring data scientists, we aim to enhance financial inclusion for the unbanked population and deter subprime lending to high-risk borrowers. Specifically, our data analysis will address the following key business questions:

- Factor Analysis: What are the primary factors that significantly influence an applicant's ability to repay a loan and, consequently, affect our loan approval decisions?

- Exploratory Data Analysis: What characteristics define our customer profiles in various dimensions?

- Predictive Modeling: How can we fine-tune our credit-granting strategies to optimize expected profits?

## Data Understanding
We have a Train/Test dataset containing 307,511 credit approval entries and 122 features related to clients’ demographics, socioeconomic profiles, and credit history. TARGET in the Train dataset is a binary variable indicating whether the loan was paid back. 

While the substance of features and observations benefits our default risk study, it also has several biases:
- First, fluctuation in interest rates significantly affects borrowers’ behavior and lenders’ profits. The higher the interest rate, the more a borrower has to repay. Consequently, the lender will face a higher default risk. On the other hand, the higher the interest rate, the higher the cost of capital, and the lower the discounted cash flow we received (profit). However, we don’t have interest rate and timestamp data and thus cannot model those impacts.
- Second, the data set was published in 2018, which is not an up-to-date reflection of the current customer data and our default detect capability technology. New objective variables and borrower information may have arisen to help predict the default pattern better. Also, 22.7 million Americans have a personal loan in the second quarter of 2023, while our dataset contains 307,511 observations. It’s unclear whether it is a representative sample of Home Credit’s customer group or will continue to be as time shifts.
- Third, the data provided is skewed toward individuals with a high credit score and a strong credit history since they already gained loan approval from Home Credit. This may lead to positive selection bias because it underrepresents those with weaker credit profiles more likely to default.

## Data Preparation
- Removed identification variables: SK_ID_CURR, not related to default status.
- Drop Outliers: After dropping the 2 CODE_GENDER = "XNA" cases, we recode CODE_GENDER as dummy variables with 2 levels.
- Recode abnormal observations: In both train and test sets, there are 55374 occurrences of 365243 days in DAYS_EMPLOYED, around 1000 years. We create a DAYS_EMPLOYED_ANOM flag to log this abnormality and recode those abnormal values to 0.
- Recode counter-intuitive values: for unspecified reasons, the DAYS_XX variables in our dataset are all coded as negative values, we code all of them into positive values for ease of interpretability. After that, we divide the DAYS_BIRTH by 365 to transform it into AGE(YEAR_BIRTH) and drop the original DAYS_BIRTH to avoid multicollinearity.
- Create New Feature via Domain Expertise: We create four new variables based on original ones, which we believe are important measures of repayment ability, which indeed rank high on subsequent feature importance tests. They’re
   - CREDIT_INCOME_PERCENT: the percentage of the credit amount relative to a client's income. It provides insights into whether the client's regular loan payments are affordable within the context of their income. High credit-to-income ratios could signify financial strain.
   - PAYMENT_CREDIT_PERCENT: the percentage of monthly payment relative to total credit.
   - EMPLOYED_PERCENT: the percentage of the days employed relative to the client's age. A low employed-to-age ratio might indicate employment instability and low wealth accumulation, affecting the client's ability to make loan payments.
- Factorize and Encode character variables: Within 120+ original features, 16 are of char types. 4 of the 16 chars are of 2 levels, and 12 are of multiple levels. We conduct label encoding on variables with 2 features to minimize storage and one-hot encoding with two features to avoid imposing orders on unordered levels.
- Handling missing values:
-    Visualize NA in the top 20 features: We first identify the five with significant missing values among the top features.
![image](https://github.com/No1Talent/Credit-Risk-Analysis-And-Customer-Churn-Prediction-/assets/91887485/7b8c168a-baad-4819-89aa-9d3ef8e8561b)
-    Square Test on MAR assumptions: Then we conduct Chi-Square tests to see whether those values are missing randomly (MAR). The p-values for all of them are highly significant (p-value < 2.2e-16), suggesting that the missingness is not independent of y.
-    Impute NA: We create NA indicators for all of them and recode NA to 0:
-    Remove obs with a few NA in other columns: till now, train becomes a complete sample with 264894*214 size.
- Split the train data: into data_train and data_valid in advance to find out whether our final model suffers overfitting.
- Normalize numerical features if necessary.


## Factor Analysis And Data Visualization
![image](https://github.com/No1Talent/Credit-Risk-Analysis-And-Customer-Churn-Prediction-/assets/91887485/699fda05-b0ec-4ad4-a435-f288f01f6885)

![image](https://github.com/No1Talent/Credit-Risk-Analysis-And-Customer-Churn-Prediction-/assets/91887485/17c96930-0df8-4b69-b486-d397f882986f)

## Customer Churn Prediction




## Credit Amount Discussion
