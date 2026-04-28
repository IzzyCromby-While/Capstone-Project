# Function 2
-	Input: 2D array 
-	Output: 1D array
-	Goal: Maximise 
-	Current y_best: 0.6
-	Function two represents a black-box model that takes two numerical inputs and returns a log-likelihood score. The aim is to identify the input values that maximise this score. Each evaluation is noisy (ie. There’s randomness as part of the function and therefore you won’t get the same output every time), and you could get trapped in a local optimum. 

## Origional Model Architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model with a combination of Matern and White Kernel and an EI acquisition function. 
-	GPR surrogate model chosen due to its strong performance in low-dimensional spaces and ability to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	The Matern kernel is chosen because it accommodates the moderate roughness expected from this model. The White kernel models the inherent noise and stops the GP from overfitting. 
-	EI acquisition function is chosen because it measures probability and magnitude of improvement making it more robust in a bumpy black-box landscape. 

## Original Hyperparameters 
-	Noise is set to 0.3 because the initial brief warns to expect noise. 
-	Length scale is set to 1e-3 – 5 so that the GP can model structure without overfitting noise. 
-	Xi was set to 1 originally to encourage exploration in a locally irregular surface. 

## Week 1 Query 
- Log-likelihood values range from -0.07 to 0.61, suggesting moderate variability and confirming the presence of some noise. 
- The highest observed point so far is y = 0.61. 
- Iteration should focus on exploration to begin with. Although a promising area has emerged, we should verify whether the high value region extends or forms an isolated peak. 

## Week 2 Query 
- New y value: -0.04 – worse than y_best
- Beta is changed to 2.5 to encourage exploration of the 2D space

## Week 3 Query
-	New y value: 0.14 – worse than y_best 
-	I noted that I already have a promising region around (0.7, 0.93). With my week 2 query being slightly more exploratory but yielding a much worse result, I decide to switch to balanced exploration around the promising region. 
-	I keep the Xi at 2.5 but accept a point close to current y_best input values. 

## 	Week 4 Query 
-	New Y value: 0.2 – although the query is close in search space to the current y_best, y_value is a lot lower, suggesting there is a strong local peak around (0.7, 0.9). 
-	The brief warns about a noisy surface and so to avoid being trapped in local optima I change beta from 2.5 to 3 to encourage more exploration. I will try to find other high-value basins before committing to exploitation at this early stage. 

## 	Week 5 Query 
-	New y value: 0.3 – worse than y_best. 
-	Convergence warnings on length scale and noise levels. I assumed the log-likelihood surface was moderately smooth and noisy. GP pushed both length-scale and noise level to lower bounds, indicating function varies more locally than smoothness settings allowed. 
o	Length scale min 0.05 -> 1e-3
o	Noise min 1e-4 -> 1e-6
-	Realised EI should be expressed in the same units as the function. Xi = 1  -> Xi = 0.1. This is still substantial fraction of the observed output range and therefore still promotes exploration. However, after implementing this new Xi value, the acquisition function was converging prematurely. 
-	Switch to UCB to enforce broader exploration. I calculated the UCB beta value based on the GP-UCB confidence parameter, Beta = 5.9
-	Even with UCB, the acquisition function still selected the known best basin. To avoid early convergence, it’s important to explore, so I apply a minimum distance of 0.07, corresponding to seven grid steps, which is large enough to prevent repeated sampling. 

## 	Week 6 Query 
-	New y value: 0.03 – deliberately far from best known basin, but worse y_value 
-	At this relatively early stage, I will continue to explore to gain an understanding of the search space. 

## 	Week 7 Query 
-	New y value: 0.2 - worse than y_best 
-	Realise that yet to explore search space close to (0.15, 0.85) and (0.9, 0.5). Therefore, I do targeted exploration here. 
-	Due to a convergence warning I also decrease lower bound of noise level from 1e-6 to 1e-10. 

## 	Week 8 Query 
-	New y value: 0.4 – not y_best but well performing 
-	Possibly found another local optimum, will return to this point when exploitation starts.
-	I will do one more exploration step aiming for top left corner of the search space which is yet to be explored – it’s important to cover here due to the prior warning about a multi-modal surface. I choose to do a manual query. 

## 	Week 9 Query
-	New y value: 0.0694698453614947 – poor performing
-	Improve interpretability, visualisations of observed data, posterior mean and acquisition surface made. Posterior mean surface showed overfitting (sharp peaks), so increased noise lower bound to 1e-6. 
-	Second half optimisation process, reduce Beta =3, start to deprioritise exploration. 

## Week 10 Query 
- New y value: 0.639782210977777 – new y_best 
- Week 9 query point close in parameter space to another high performing point (0.7, 0.92, y = 0.61), suggesting consistent high value basin. 
- Shift from exploration to local refinement by switching to Expected Improvement, setting Xi = 0.03 focus on incremental gains around the current best.

## 	Week 11 Query 
-	New y value: 0.5004626497577797 
-	Fairly strong eventhough did not beat y_best. Supports point that I found high-performing basin. 
-	Slightly worse point may be due to switching to EI and using Xi = 0.03. May not be exploitative enough. 
-	Reduce Xi = 0.02 prioritise exploitation of promising region. 

## 	Week 12 Query 
-	New y value: 0.7208195831473718 – new y_best 
-	Validates high performing basin
-	Reduce Xi to 0.01

## Week 13 Query
-	New y value: 0.58423381447619 – same region but below y_best
-	Possibly diminishing returns? Or just improvements now local and sensitive. 
-	Will finish with a manual query likely to improve. Previous attempts to improve on y_best pushed x values up by 0.01 which caused diminished outputs. Instead I increase current best point 0.711680, 0.972452 by 0.004. 
-	Result : Y = 0.570626585791083






