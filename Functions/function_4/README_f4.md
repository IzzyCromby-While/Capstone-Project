# Function 4 
-	Input: 4D array 
-	Output: 1D array
-	Goal: Maximise 
-	Y_best: -4.03
-	Address the challenge of optimally placing products across warehouses for a business with high online sales, where accurate calculations are costly and only feasible biweekly. To speed up decision-making, an ML model approximates these results within hours. The model has four hyperparameters to tune, and its output reflects the difference from the expensive baseline. Because the system is dynamic and full of local optima, it requires careful tuning and robust validation to find reliable, near-optimal solutions.

## Origional 	Model architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model with a combination of Matern and White Kernel and an UCB acquisition function. 
-	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	I use a Matern kernel with a WhiteKernel noise term. The Matern kernel captures the irregular, non-smooth shape of a 4D hyperparameter landscape. Adding White Kernel accounts for experimental noise expected from this model, preventing the model from overfitting random fluctuations. 
-	UCB Acquisition Function was chosen because it promotes broad exploration of the 4D dynamic landscape and will find a robust well-performing region rather than a fragile optimum which is important in this potentially shifting environment. 

## Original Hyperparameters 
-	Matérn kernel with ν = 2.5, as ν = 1.5 would produce an overly rough posterior that was too sensitive to small fluctuations, leading to unstable optimisation.
-	Length scale is set to 0.3 allowing for the moderate bumpiness and local irregularity expected from the model. 
-	Noise level is set to 0.3 to account for moderate noise 
-	UCB is set to 5 to encourage broad exploration of the landscape. 

## Week 1 Query 
-	Broad exploration 

## Week 2 Query 
-	New y value: -39 – significantly worse than y_best 
-	Continue with beta = 5 for broad exploration 

## Week 3 Query 
-	New y value: -0.76 – new y_best 
-	Shift to structured exploration to probe the new best region structure. 
-	Reduce beta to 3. 

## Week 4 Query 
-	New y value: -9.8 worse than y_best 
-	Although fairly close to y_best x inputs, this point is significantly worse suggesting current y_best could be narrow. Something to note here is that x3 dropped significantly so although I suggested may be a structured exploration step, probably better classified as global exploration. 
-	Beta is set back up to 5 to continue global exploration 

## 	Week 5 Query 
-	New y_value: -31 significantly worse 
-	Due to convergence warnings, I change the noise lower bound from 1e-4 to 1e-10 due to convergence warnings. I initially enforced a higher noise level to avoid overfitting. I am aware that lowering the noise bound can increase the risk of overfitting, so near the end of the project I will verify that the final solution remains stable across repeated evaluations. 
-	Beta = 5 was producing edge values which previous queries have already established are not productive. Therefore, I change beta back down to 3.5 

## 	Week 6 Query 
-	New y value: -0.9 
-	this point falls nearby current y_best but confirms that it’s a relatively narrow region as the x values differ slightly and the y value changes by 0.2. 
-	However, there is a large performance gap between this area and the rest of the search space. 
-	I will continue to probe this area to assess it’s sensitivity. 
-	Reduce beta to 3

## Week 7 Query 
-	New y value: -1.9 
-	Again, this is nearby y_best and only differs a small amount in comparison to the rest of the search space. 
-	I reduce beta from 3 to 2.5 to continue probing this possible best basin. 

## 	Week 8 Query 
-	New y value: surprising value: 0.414 which is positive 
-	Since this is ‘distance from an expensive baseline’, I assume that reaching a positive means I have  exceeded the baseline.
-	I should probe this region, but I’m also concerned I haven’t explored enough. Therefore, I will make one deliberate probe here now and then next round do a global exploratory step. 
-	Reduce beta from 2.5 to 2. 
-	I tried this, but the optimizer picked the same grid point again, pointing to the issue that my grid space is too coarse for this stage of exploitation, switch to random candidates. 

## Week 9 Query 
-	New y value: -1.8 
-	Even though I reduced beta, the new query is significantly worse. 
-	My options here are to either probe / exploit the y_best point, the one that produced a y value of 0.4. For that, it would be better to change from UBC acquisition function to EI or PI, which are more exploitative. Or to inquire whether the point is a result of noise, a sharp spike, or a genuinely good basin. Therefore, I choose to increase the x values for the Y_Best by 0.03 to check this. (manual)

## 	Week 10 Query 
-	New y_value: -2 
-	Large drop from 0.414 to -2 after increasing all X inputs by 0.03 suggests positive part of sharp optimum or noise. 
-	Switch to EI to refine around sharp local peak / understand if it is a result of noise. Xi = 0.03

## 	Week 11 Query 
-	New y value: -0.1 
-	Does not exceed current best, but is significantly better than previous points, suggesting positive result not purely isolated noise spike. 
-	Reduce the amount I adjust by. Adding 0.03 significantly decreased output. This time I add 0.001 to each X value of best point. 

## Week 12 Query
-	New y value:  0.37141486634922005
-	Slightly worse but close to y best. Useful to understand original positive y best was not a result of noise. 
-	Last week added to current X values, instead minus 0.01 from current best point. 

## 	Week 13 Query
-	New y value: 0.455064728399716 – New y best 
-	Continue in this direction by making small perforations because even medium changes seem to yield worse results. 
-	Result: 0.474702



