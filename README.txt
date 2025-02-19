# Starbucks Union Elections Analysis (2021-2023)

I implemented Bayesian logistic regression on a dataset of Starbucks union elections from August 2021- August 2023. The data on locations, dates, and results came from unionelections.org. I modified the dataset to include potentially relevant attributes for study.

## Input Features
- Right to Work state status
- Median household income (2020 census) for election location
- State minimum wage
- State Gini coefficient (income inequality)
- State union membership percentage
- State Democratic vote percentage (2020 presidential election)
- State margin shift from Republican to Democrat (2020)
- State immigrant population percentage
- Initial vs revised vote status
- Percentage of eligible workers that voted
- Total eligible voters
- Relative date filed (from first union election filing)
- Relative tally date
- Days between filing and voting
- Number of prior elections in state

Most demographic data came from the census. Data was limited by having to use state-level demographics rather than local data. Features were scaled to mean 0 and variance 1 for better interpretation and SGD performance. This resulted in 451 total observations. The response variable measured election success (votes to join > votes to not join).

## Model Parameters

I chose a large prior variance for the Gaussian prior since I didn't know the parameter effects well. Small variances led to more test data overfitting, while larger variances generalized better but performed worse on training data.

Given the small dataset, I used a small batch size for SGD. Very small batches were unstable but large batches took longer to improve. I ultimately chose a batch size of 48.

I decreased the learning rate until the objective function stabilized. The smaller batch size required a smaller learning rate (0.0001) for stability.

I set the number of iterations (300) based on where test cross entropy loss peaked to prevent overfitting.

## Results

The resultant beta coefficients were:
```
[0.27227037,  0.00954723, -0.23437002,  0.54283922, 0.37180527,
 0.58296698,  0.48565024, -0.57440793,  0.1693631, -0.18562403,
-0.10718817,  0.08338559,  0.00400394, -0.35841185, -0.14250866]
```
with a bias term of 1.49145598

## Key Findings

Strongest positive correlations with union election success:
- States with higher income inequality
- More Democratic-voting states
- States with stronger Democratic shifts in 2020

This could indicate that exposure to income inequality increases likelihood of understanding class concepts and supporting unions, along with the association between left politics and unionism.

Negative correlations:
- States with higher immigrant populations
- Longer time between filing and voting

This may reflect immigrant communities' hesitation to join unions due to fear of repercussions, while increased filing-to-voting time could indicate more contested elections.

Town median income and election order (measuring "momentum") showed minimal correlation with election outcomes.

Course project as part of STCS 6701: Probabilistic models and machine learning, Columbia University
