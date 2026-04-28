# 	Function 5
-	Input: 4D array 
-	Output: 1D array
-	Goal: Maximise 
-	Current y_best: 1088.86
-	This is a four-variable black box function representing a chemical process in a factory. The function is typically unimodal, with a single peak where yield is maximised. Your goal is to find the optimal combination of chemical inputs that delivers the highest possible yield. The function represents a stable chemical process with a typically smooth, unimodal landscape. As such, the underlying response surface is expected to vary gradually across the four input dimensions. Small parameter changes should yield correspondingly small shifts in output.

## Origional	Model Architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model with a RBF kernel and EI acquisition function. 
-	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	RBF will be able to model the smooth nature of the model 
-	EI is used because the function is smooth and unimodal, so it efficiently balances exploration and exploitation by considering both the likelihood and size of potential improvements. 

## 	Original Hyperparameters 
-	Noise is set to 0.01 because this a chemical process with little noise. 
-	Length scale is set to 0.5 to reflect the expected smooth gradual unimodal increase. 
-	Initial xi term of 0.1 

## 	Week 1 Query 
-	Xi set to 0.1 to balance exploration and exploitation. 
-	There is one unimodal peak so it is better to exploit that than explore globally. 

## Week 2 Query 
-	New y_value: 3681 – significantly better than y_best 
-	Xi is changed to 0.05 to exploit this peak 

## Week 3 Query 
-	New y_value: 6886 – significantly better again 
-	Keep Xi small to focus on exploiting 

## Week 4 Query 
-	New y_value: 7915 – significantly better again 
-	Keep xi the same since it has consistently improved results. 

## Week 5 Query 
-	New y_value: 4040 – worse than other results 
-	I’ve identified a strong optimum at the edge of my search space (week 4 result). Moving away from this boundary has resulted in a significant drop in performance. To confirm that the optimum is near the boundary of the search space, and start to refine my results, I keep the acquisition settings unchanged and systematically perturb one input at a time while holding the others at their optimal values. Allows me to assess the sensitivity of each parameter and verify whether the optimum is truly boundary-limited or whether small adjustments can yield further improvements.
-	I will adjust length scale since I have a convergence warning, moving it from 2 to 3. 

## Week 6 Query 
-	New y value: 7739 – does not beat y_best 
-	Dropping x1 from 0.99 to 0.98 causes a moderate decrease, supporting assumption that the true maximum lies at or beyond the boundary. 
-	Now drop x2 from 0.99 to 0.98

## Week 7 Query 
-	New y value: 7739 – same result 
-	Again, dropping x2 caused a moderate decrease in y value, supporting assumption that the true maximum lies at or beyond the boundary. 

## Week 8 Query
-	7739.62761334158 – same result 
-	Function 5 optimiser working on a grid and instead it needs to be choosing random candidates, I fix that.
-	Then I try the final iteration of keeping all other x values at 0.99 and dropping one x value to 0.98 per week. 

## Week 9 Query
-	7739.62761334158 – same again. 
-	I have made a slight change to every x value and have come to the conclusion that the function is not especially sensitive to one of them. I have got the same result every time. Now, having changed my optimisation to run on random candidates rather than a grid, I hope that the optimiser can improve on it’s original y_best with x values all at 0.99. 
-	I reduce the xi down to 0.01 because it is a unimodal peak. 

## Week 10 Query
-	New y value: 5071.523447259666 
-	No improvement, consolidates theory optimum at upper boundary. 
-	Reduce Xi to 0.001. 
-	Tune length scale and noise because of convergence warnings, however GP tunes length scale up to 20 - too big and risks underfitting so I reduce. 
-	Optimiser proposes similar candidate points to previous, (0.9, 0.8, 0.9, 0.3). Since I know unimodal peak and high values are at boundary, I reject this point. 
-	Manually add 0.001 to each boundary point to continue ‘climbing the peak’ I think I have found. 

## Week 12 Query
-	New y value: 7987.91780121314
-	With only two queries left, I think adding another 0.001 is too conservative, I assume I am climbing unimodal peak. Therefore, I add 0.005 to current X input values. 

## Week 13 Query
-	New y value - 8282.121444742894
-	Performance continued to improve at 0.995 in all four dimensions, optimum must lie between 0.995 and 1. I select boundary adjacent query at 0.999999 in all four dimensions to approximate upper boundary as close as possible. 
-	Result  8662.405001
