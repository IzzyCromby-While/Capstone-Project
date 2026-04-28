# Function 3 
-	Input: 3D array 
-	Output: 1D array
-	Goal: Maximise 
-	Current y_best: -0.03
-	Function three represents a black-box optimisation problem in a drug discovery setting, where each evaluation tests a unique combination of three compounds. The objective is to minimise adverse reactions, but since the challenge is framed as maximisation task, the output represents the negative number of side effects, meaning higher values correspond to safer, more effective combinations. Each experiment is stored in initial inputs as a 3D array, where each row lists the amounts of the three compounds used. After each experiment, the number of adverse reactions is stored in a 1D array. As the function represents a biological process, it is expected to be noisy and non-linear. This is due to biological variability and complex chemical interactions meaning one slight change to the input compound could dramatically change outcomes. 

## 	Origional Model Architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model with a Matern Kernel and EI acquisition function. 
-	GPR surrogate model chosen due to its strong performance in low-dimensional spaces and ability to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	Metern Kernel was chosen to as the model can capture roughness which is appropriate for biological data where the compound interactions make the landscape bumpy. Adding White Kernel accounts for experimental noise arising from biological variability, preventing the model from overfitting random fluctuations. 
-	Expected Improvement (EI) as the acquisition function because the biological process is noisy and contains local optima. EI balances both the probability and magnitude of improvement, making it effective at escaping local optima and exploring a bumpy black box landscape.

## 	Original Hyperparameters 
-	ν is set to 2.5 in the Matern kernel because it models a moderately rough biological response surface—flexible enough to capture non-linear interactions between compounds, but smooth enough to avoid overfitting noise.
-	Noise is set to 0.3 because the initial brief warns to expect noise. 
-	Length scale is set to 1e-3 – 5 so that the GP can model structure without overfitting noise.
-	Xi is set to 0.2 to increase exploration in a noisy, locally irregular objective surface.


## 	Week 1 Query 
-	The landscape is biological and therefore expected to be noisy and moderately bumpy and therefore initial exploration is essential to understand the function landscape. 

## Week 2 Query
-	New y value: -0.07 is worse than y_best 
-	Keep hyperparameters in global exploration 

## Week 3 Query 
-	New y value: -0.12 worse than y_best
-	Keep xi at 0.2 for global exploration 

## Week 4 Query 
-	New y value: -0.25 worse than y_best 
-	After three rounds of exploring high uncertainty regions, all boundaries performed much worse than midrange values 
-	I lower xi to 0.15 allows for exploration but shifts the balance towards mid region 

## 	Week 5 Query 
-	New y value: -0.017 – new y best 
-	Although noisy, midrange compound values repeatedly produce better outcomes
-	So I reduce xi to 0.1 to do targeted exploration around mid-range. I will allow the optimiser to probe nearby mid-range combinations to map the basin shape and consider if it is part of a sharp spike or broader ridge 

## 	Week 6 Query
-	New y value: -0.07 worse than y_best 
-	Although I had decreased xi week 5 query still had one edge value present and this week having a worse result supports my hypothesis that mid-range values perform better. 
-	I reduce xi down again to 0.08

## Week 7 Query
-	New y value : -0.04 worse than y_best but well performing 
-	Query was nearby to current y_best, suggesting well performing basin found. 
-	Reduce xi to 0.04 for targeted exploration around the promising basin. 

## Week 8 Query
-	New y value: -0.101207804750248 worse than y_best 
-	The other input values, x1 and x2 have both been tested towards a boundary and produced y value in the best basin. This suggests that the function is more sensitive to x3 which should be a mid-range value. 
-	I drop xi down to 0.02 to exploit the best region. 

## Week 9 Query
-	New y value: 0.006 – new y_best 
-	In refinement stages, linspace gird is too coarse, causing optimiser to repeatedly select identical candidates. Switch to randomly generated candidate points. 
-	Reduce Xi to 0.01 continue to exploit best region.

## 	Week 10 Query
-	New y value -0.01726728023779992 – worse than y_best 
-	Not significantly worse, but in latest evaluation x2 and x3 changed noticeably. Suggests promising region locally sensitive. Consistent with earlier observation mid-range values perform better. 
-	Keep Xi at 0.01. However, GP prosed, 0.986004, 0.016018, 0.86378. Already established mid range values perform best. Therefore reject this point.  
-	Manually query based on small distance from current best point, probing whether best result is part of broad region or sharp peak. 

## 	Week 11 Query 
-	New y value -0.051319200752823216 – worse than y best
-	Although deliberately probed close to y_best, this output is significantly worse. Suggesting basin likely locally sensitive, optimum inside midrange likely to be narrow. 
-	Small amount of iterations left, smaller manual change around current best. +0.001 to each X_value of Y_best point. 

## Week 12 Query
-	New y value: -0.000902482378480971 – new y best 
-	Mid-range basin confirmed. 
-	Micro manual refinement in same direction +0.001 to current point. 

## 	Week 13 Query
-	New y value: -0.00699344189505451
-	Despite micro refinement, output changed a lot. Suggesting current best point extremely locally sensitive or even a noise spike? 
-	Extremely small probe around current best point +0.0001
-	Result Y = -0.0009099208929336151



