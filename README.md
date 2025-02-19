# AI-MACHINE-LEARNING-FOUNDATIONS
Assignments for ML course


# Assignment 1 Notes on my process and thoughts:

had issues with the first version of this development and realised I got confused by following the insturctions and intorduced data leakage. I created a new branch called restructuring and will remodel all my code to address these issues. Here are my updated steps. All these notes here are an attempt to structure my work and account for all possible issues that might arise before I attempt to code this model again. 

## Step 1: Data Loading and EDA

- Familiarised myself with the kaggle API https://www.kaggle.com/docs/api#authentication which could be useful in future 
assigments. Was a very simple install. Another resource I used: https://dev.to/taw/how-to-access-kaggle-data-from-command-line-4hgd

- all rudimentary EDA was already completed in my first attempt

## Step 2: Managing Missing Values

- Before Splitting:
    - Dropping columns that introduce data leakage or bias like boat(indicates survival) or body(indicates non survival)
    - Drop columns with non predictive features name (not useful for survival prediction) or ticket (no clear relationship with survival)
    - Drop Columns with excessive missing values e.g cabin 
    - Fill categorical variables with a placeholder (embarked with unkown for example)

- After splitting (DATA LEAKAGE PROBLEM I HAD)
    - Numerical impuation
    - Possibly attempt KNN impuation if I have time for correlated values
    - After splitting -> categorical values like embarked we can choose to keep as unknown and have it as another category in the encoding. Or replace unkown with mode impuation which I did in the first iteration. The choice depends on how many unkowns there are. Unknown could carry predicitve power or could be random noise and my choice will depend on this.


- Why this approach works?
    - Columns that are dropped before splitting to remove or fill values in a way that doenst introduce bias or rely on the realationships the model is supposed to learn. 
    - Statisitcal impuation must happen after splitting so test data doesnt influence training which is the main error we had.

## Step 3: Data Splitting (before any feature engineering)
- Train-validation-test split: 80-10-10 or 70-15-15 split

## Step 4: Encoding of Categorical Variables
- I could encode sex before splitting since it’s just a binary variable (0/1), but I will encode all categorical variables together here for consistency and clarity.
- Creating a new FamilySize feature (SibSp + Parch + 1) to capture group survival likelihood.
- Binning age into categories (child, teen, adult, senior) to account for non-linear effects.

## Step 5: Feature Scailing
- Normalize numerical values (only after splitting).
- In my previous attempt, I scaled the entire dataset before splitting, meaning the mean and STD calculations included test data.
- Now, I will fit StandardScaler only on training data and apply the same transformation to validation/test data.
- Scaling Choice:
    - StandardScaler (Z-score normalization) is best for logistic regression since it assumes normally distributed inputs

## Step 6: Adressing Class Imbalance (training data only)
- I used SMOTE in my first iteration but will revise and see if this is the best approach
- Instead of blindly applying SMOTE, I will first try LogisticRegression(class_weight='balanced') since adjusting weights is often more stable than synthetic sampling.
- If SMOTE is still necessary, I will apply it only on training data to avoid synthetic samples in validation/test sets.


## Step 7: Feature Selection (AFTER Splitting, ONLY on Training Data)
- Remove highly correlated features (avoiding multicollinearity).
- Fixing Data Leakage Issue from First Attempt:
    - In my previous attempt, I removed correlated features using the entire dataset, which means the model indirectly learned from the test set.


## Step 8: Train a Logistic Regression Model
- train model on training data
- Validate on validation set
- Use test set for final evaluation