# 	Function 7
-	Input: 6D array 
-	Output: 1D array
-	Goal: Maximise 
-	Current y_best: 1.3
-	You’re tasked with optimising an ML model by tuning 6 hyperparameters for example, learning rate, regularisation strength or number of hidden layers. The function you’re maximising is the model’s performance score (such as accuracy or F1), but since the relationship between inputs and output isn’t known, it’s treated like a black box function. Because this is a commonly used model, you might benefit from research best practices or literature to guide initial search space. Your goal is to find the best combination of hyperparameters that yields the highest possible performance. 

## Origional Model architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model, a combination of White and Matern Kernel, and the UCB acquisition function.
-	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	The Matern kernel captures the rough, irregular behaviour typical of hyperparameter tuning, and the White Kernel models noise from stochastic model training.
-	Hyperparameter tuning is high-dimensional, noisy and highly multi-modal, so UCB is preferred because it keeps exploring broadly and avoids premature convergence

## 	Original Hyperparameters 
-	Moderate noise of 0.2 is set because ML training is stochastic. 
-	Small length scale of 0.2 set to reflect irregular structure of ML hyperparameter landscapes. 
-	Nu = 1.5 allow GP to model rough surface. 
-	Beta = 10

## Week 1 Query 
-	Emphasise exploration because the 6D hyperparameter space is highly multimodal and uncertain. Exploring globally increases the chance of discovering multiple promising regions. 

## Week 2 Query 
-	New y value: 0.92 
-	Although this doesn’t beat y_best, it’s nearby in the top 2 or 3 results. 
-	I continue global exploration at this early stage.

## Week 3 Query
-	New y value: 0.1
-	Much worse than y_best 
-	At this early stage I keep beta the same and continue global exploration

## Week 4 Query 
-	New y value: new y_best: 1.7
-	High values of parameter six are found in other high values suggesting that the optimiser is refining a promising ridge in the 6D space rather than finding a completely new peak. 
-	Push beta up from 10 to 12 to encourage broader exploration at this early stage. The next query keeps the sixth hyperparameter in the high-performing region but makes substantial changes to the other five (especially x4 and x5). Allows UCB to explore a new part high X6 subspace, potentially revealing another local maximum while still exploiting the learned pattern that large x6 values tend to perform well. 

## Week 5 Query 
-	New y value: 0.05 – much worse than y_best 
-	Realised I need to scale beta within the range of the uncertainty of the function, so I recalculate a explorative Beta=4.8, but this suggests points already sampled. 
-	Increase Beta to 5.  
-	Also decrease the lower noise bound from 0.05 to 1e-2 due to convergence warning 

## Week 6 Query 
-	New y_value: 0.7 
-	Even though the x inputs in the last query varied quite a lot, the y_value is still relatively high, suggesting there could be a stable well performing region when x6 is high. 
-	I keep UCB exploratory and force the next query to be materially different to improve global coverage and reduce the risk of missing a better basin (while also keeping x6 still high).

## 	Week 7 Query 
-	New y_value: 1
-	Despite all other parameters changing a lot, the y_value remains high, suggesting a good basin has been found. 
-	In this case, I reduce beta toward structured refinement. Beta = 4 

## Week 8 Query 
-	New y_value: 1.7 
-	Well performing new point, despite not quite beating y_best. 
-	I reduce beta = 3 to promote exploitation of promising basin. 

## Week 9 Query 
-	New y value: 1.9 – new y_best 
-	This week, I have added a PCA plot to visualise my data. It provides a visual summary of sampled points in the high dimensional search space. Points are coloured by objective value allowing identification of promising regions and broad patterns. 
-	In Function 7, the PCA plot showed a more concentrated cluster of high-performing points with many lower-performing samples outside this region. This resembles a basin or “island” of strong performance. In such cases, a classifier-based filter could be defensible because it may help prevent evaluations in clearly unpromising areas while still allowing the GP surrogate to optimise within the promising region.
-	The data is clearly non-linear so a soft-margin SVM will be used. The SVM will act as a qualifier of new prospective points. Once they are suggested, I will check them on the classifier to understand where they are likely to sit. Only points in a ‘good’ region will be accepted. Class of promising and not promising separating the promising points as the top 20% of performers. RBF kernel for non-linear separation. Allow c =1 to keep the SVM, since large c increases risk of overfitting. 
-	Roughly 15% of my current data is y>1. This is what will count as a promising region. To mitigate the risk of missing other optimal areas, I will increase the boundary of the line to top 20% of points. 
-	I also reduce beta down to 2.5 to promote exploitation of promising region. 
-	I used the SVM classifier to predict whether the new point is in the top 20% promising region. It is, so I accept it. 

## Week 10 Query
-	New y value 2.6545586608715093 – new y_best 
-	The Week 10 result of 2.65 is a large improvement over the previous best of 1.9, confirming a strong high-performing basin. With only four iterations remaining, broad exploration is less valuable.  I switch from UCB to Expected Improvement to focus on local refinement of promising region. 
-	Xi = 0.02 prioritise exploitation
-	Continue using SVM classifier as qualifier

## Week 11 Query 
-	New y value:  2.560121342153576
-	Nearby to y best
-	Reduce Xi to 0.01

## Week 12 Query 
-	New y_value: 2.3095030840233
-	Possibly diminishing returns? 
-	Tighter local refinement not yielding improvements, increase Xi =0.03 to stay in promising region while also making optimiser less greedy. 

## Week 13 Query
-	New y value: 2.14178803062066
-	Again, weaker than y best 
-	In final round, I assume I’m close to the maxima. When there is little improvement to be found, Expected Improvement can prioritise uncertainty. This round I run both EI and Probability of Improvement (which just prioritises improvement despite the magnitude). I run both to give me options of proposed candidate points. 
-	I choose PI (Xi = 0.005) which produces candidate point which aligns with what I high performing point looks like based on my developed understanding of the function landscape. Eg. High x6 value and nearby to current best basin. 
-	Result: 2.652186
