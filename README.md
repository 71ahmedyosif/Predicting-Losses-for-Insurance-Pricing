# Pre-Modeling:
Prior to developing prediction models, different sub-populations were identified from the dataset. This was done using GMM (Gausian Mixture Modeling), and follows an unsupervised machine learning approach to identify different sub-populations.

The optimal number of sub-populations was determined by plotting the BIC Score against the number of clusters, and selecting the least number of clusters that led to no signficant change in BIC Score (Elbow Method). 
![Segmentation](/assets/img/Segmentation.PNG)
![Segmentation Analysis](/assets/img/anal_segments.PNG)

# Modeling Approach:
For each sub-population, a tweedie regression model was developed using LightGBM to predict the annualized losses. 
![Model Setup](/assets/img/model_setup.PNG)

## Data Split:

Full Dataset was split into 80% (Training) and 20% (Testing. The population segments were stratisfied such that the proportions between the training and testing datasets were conserved. This prevents overfitting/underfitting when developing prediction models for each segment.
The missing values in the full dataset was less than 20%, and therefore simply dropped prior to modelling. Imputting missing values while using GMM to identify sub-population leads to testing data leakage. 

## Feature Engineering
Categorical predictors were dropped, and numerical predictors were standardized. 

## Offset Variable:
Earned Exposure and results from a preliminary GLM model were used as the model offset. The offset term was constructed by multiplying the two. 

## Hyper Parameter Tuning:
5-fold CV was implemented to determine optimal model for each population segment. 

## Performance Metrics:
###Gini Coefficient: 
The area under the Lorenz Curve for the cumulative fraction of losses plotted against the cumalitive fraction of earned exposure. High Gini = Model is able to distinguish between high and low risk properties. 
###100-Bin R-Sqaured: 
Comparison between the average actual and predicted values after sorting the two in ascending order (seperately) and splitting into 100 quantiles. High 100-Bin R-Squared = Model is able to accurately predict the annualized losses. 

## Model Evaluation:
The final results were compared against the results from the initial Baseline Models (GLM). 
![Results](/assets/img/results.PNG)





