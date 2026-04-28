## Model Card

## Overview 
•	Name: Bayesian Optimisation for Black Box Function Optimisation
•	Approach: Black Box Optimisation using Bayesian Optimisation and SVM Classification
•	Developer: Isobella Cromby-While

## Model Architecture
The model is based on Bayesian Optimisation, using Gaussian Process surrogate models to approximate the unknown functions. The GP predicts the function values and uncertainty which are used by the acquisition functions to select the next query point. Acquisition functions used were Upper Confidence Bound and Expected Improvement. PCA projections were used to analyse data patterns. For Function 7, an SVM classifier was applied to approximate a boundary of the high performing region and this was used to filter proposed candidate points. 

## Inputs and Outputs 
Continuous input variables in the range [0, 1], with dimensionality ranging from 2 to 8. A single scalar output representing the performance of each input. 

## Performance
Success in this task is defined by demonstrating an effective optimisation strategy, improving on initial baselines, balancing exploration and exploitation and iteratively improving. This was clearly demonstrated in 6/8 of the functions (2, 3, 4, 5, 6, 7, 8). Performance was limited for function 1 and 2. For full details, see the results section in the README file, or the performance graphs provided for each function in the function folders. 

## Limitations
- The approach relies on the assumption that the Gaussian Process surrogate models adequately represent the underlying function. The nature of the task means this may not always be the case, meaning the GP could potentially miss high performing regions. 
- Having only 13 queries, knowledge of the search sparse, particularly in high dimensional functions. This increases the likelihood of missing the global maximum. 
-The model has been developed as part of a learning process. Earlier rounds may include suboptimal decisions that were refined over time. This has lead to some inefficient use of evaluation rounds. 

## Trade-offs
The trade-off between exploration and exploitation has shaped the optimisation process. Exploring improves understanding of the search space, reducing the likelihood of missing a global maximum, exploiting refines known well performing regions. This reflects the nature of black-box optimisation, where decisions must be made under uncertainty with limited evaluation budgets. 
