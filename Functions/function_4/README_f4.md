##Function 4 
•	Input: 4D array 
•	Output: 1D array
•	Goal: Maximise 
•	Y_best: -4.03
•	Address the challenge of optimally placing products across warehouses for a business with high online sales, where accurate calculations are costly and only feasible biweekly. To speed up decision-making, an ML model approximates these results within hours. The model has four hyperparameters to tune, and its output reflects the difference from the expensive baseline. Because the system is dynamic and full of local optima, it requires careful tuning and robust validation to find reliable, near-optimal solutions.

##Model architecture 
The Bayesian optimisation framework consists of a GPR surrogate model with a combination of Matern and White Kernel and an UCB acquisition function. 
•	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
•	I use a Matern kernel with a WhiteKernel noise term. The Matern kernel captures the irregular, non-smooth shape of a 4D hyperparameter landscape. Adding White Kernel accounts for experimental noise expected from this model, preventing the model from overfitting random fluctuations. 
•	UCB Acquisition Function was chosen because it promotes broad exploration of the 4d dynamic landscape and will find a robust well-performing region rather than a fragile optima which is important in this potentially shifting environment. 

##Original Hyperparameters 
-	Matérn kernel with ν = 2.5, as ν = 1.5 would produce an overly rough posterior that was too sensitive to small fluctuations, leading to unstable optimisation.
-	Length scale is set to 0.3 allowing for the moderate bumpiness and local irregularity expected from the model. 
-	Noise level is set to 0.3 to account for moderate noise 
-	UCB is set to 5 to encourage broad exploration of the landscape. 

##Week 1 query 
-	Broad exploration 

##Week 2 query 
-	New y value: -39 – significantly worse than y_best 
-	Continue with beta = 5 for broad exploration 

##Week 3 query 
-	New y value: -0.76 – new y_best 
-	Shift to structured exploration to probe the new best region structure. 
-	Reduce beta to 3. 

##Week 4 query 
-	New y_value: -9.8 worse than y_best 
-	Although close to y_best x inputs, this point is significantly worse suggesting current y_best could be narrow.  
-	Beta is set back up to 5 to continue global exploration 

##Week 5 query 
-	New y_value: -31 significantly worse 
-	Due to convergence warnings, I change the noise lower bound from 1e-4 to 1e-10. I initially enforced a higher noise level to avoid overfitting, but after convergence warnings. I am aware that lowering the noise bound can increase the risk of overfitting, so near the end of the project I will verify that the final solution remains stable across repeated evaluations. 
-	Beta = 5 was producing edge values which previous queries have already established are not productive. Therefore, I change beta back down to 3.5 


##Week 6 query 
- New y value: -0.9 
- this point falls nearby current y_best but confirms that it’s a relatively narrow region as the x values differ slightly and the y value changes by 0.2. 
- However, there is a large performance gap between this area and the rest of the search space. 
- I will continue to probe this area to assess it’s sensitivity. 
- Reduce beta to 3.5 

##Week 7 query 
-	New y value: -1.9 
-	Again, this is nearby y_best and only differs a small amount in comparison to the rest of the search space. 
-	I reduce beta from 3.5 to 3 to continue probing this possible best basin. 

