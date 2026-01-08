###Function 7
•	Input: 6D array 
•	Output: 1D array
•	Goal: Maximise 
•	Current y_best: 1.3
•	You’re tasked with optimising an ML model by tuning 6 hyperparameters for example, learning rate, regularisation strength or number of hidden layers. The function you’re maximising is the model’s performance score (such as accuracy or F1), but since the relationship between inputs and output isn’t known, it’s treated like a black box function. Because this is a commonly used model, you might benefit from research best practices or literature to guide initial search space. Your goal is to find the best combination of hyperparameters that yields the highest possible performance. 

###Model architecture 
The Bayesian optimisation framework consists of a GPR surrogate model, a combination of White and Matern Kernel, and the UCB acquisition function.
•	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
•	The Matern kernel captures the rough, irregular behaviour typical of hyperparameter tuning, and the White Kernel models noise from stochastic model training.
•	Hyperparameter tuning is high-dimensional, noisy and highly multi-modal, so UCB is preferred because it keeps exploring broadly and avoids premature convergence

###Original Hyperparameters 
•	Moderate noise of 0.2 is set because ML training is stochastic. 
•	Small length scale of 0.2 set to reflect irregular structure of ML hyperparameter landscapes. 
•	Nu = 1.5 allow GP to model rough surface. 
•	Beta = 10

##Week 1 query 
•	I emphasise exploration because the 6D hyperparameter space is highly multimodal and uncertain. Exploring globally increases the chance of discovering multiple promising regions. 

##Week 2 query 
•	New y value: 0.92 
•	Although this doesn’t beat y_best, it’s nearby in the top 2 or 3 results. 
•	I continue global exploration at this early stage.

##Week 3 query
•	New y value: 0.1
•	Much worse than y_best 
•	At this early stage I keep beta the same and continue global exploration

##Week 4 query 
•	New y value: new y_best: 1.7
•	High values of parameter six are found in other high values suggesting that the optimiser is refining a promising ridge in the 6D space rather than finding a completely new peak. 
•	Push beta up from 10 to 12 to encourage broader exploration at this early stage. The next query keeps the sixth hyperparameter in the high-performing region but makes substantial changes to the other five (especially x4 and x5). This allows UCB to explore a new part of the high-x6 subspace, potentially revealing another local maximum while still exploiting the learned pattern that large x6 values tend to perform well. 

##Week 5 query 
•	New y value: 0.05 – much worse than y_best 
•	I realised I need to scale beta within the range of the uncertainty of the function, so I recalculate a explorative beta=4.8, but this suggests points we’ve already sampled. Therefore,  I push beta to 5.  
•	Also decrease the lower noise bound from 0.05 to 1e-2 due to convergence warning 

##Week 6 query 
•	New y_value: 0.7 
•	Even though the x inputs in the last query varied quite a lot, the y_value is still relatively high, suggesting there could be a stable well performing region when x6 is high. 
•	I keep UCB exploratory and force the next query to be materially different to improve global coverage and reduce the risk of missing a better basin (while also keeping x6 still high)

##Week 7 query 
•	New y_value: 1
•	Despite all other parameters changing a lot, the y_value remains high, suggesting a good basin has definitely been found. 
•	In this case, I reduce beta toward structured refinement. Beta = 4 

##Week 8 query 
•	New y_value: 1.7 
•	Well performing new point, despite not quite beating y_best. 
•	I reduce beta = 3 to promote exploitation of promising basin. 

