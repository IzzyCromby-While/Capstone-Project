# Function 8
-	Input: 8D array 
-	Output: 1D array 
-	Goal: Maximise 
-	Current y_best: 9.3 
-	You’re optimising an eight-dimensional black-box function, where each of the eight input parameters affects the output, but the internal mechanics are unknown. Your objective is to find the parameter combination that maximises the function’s output, such as performance, efficiency or validation accuracy. Because the function is high-dimensional and likely complex, global optimisation is hard, so identifying strong local maxima is often a practical strategy. For example, imagine you’re tuning an ML model with eight hyperparameters: learning rate, batch size, number of layers, dropout rate, regularisation strength, activation function (numerically encoded), optimiser type (encoded) and initial weight range. Each input set returns a single validation accuracy score between 0 and 1. Your goal is to maximise this score.

## 	Week 1 Query
-	Global exploration to understand the search space. 

## 	Week 2 Query 
-	New y value: 9.8 – new y_best 
-	In high dimensions a single improvement is not strong evidence that I am near the global optimum. To avoid prematurely over-exploiting a potentially shallow or noisy peak, I will modestly increase the exploration strength. I raise UCB’s β from 1 to 1.5

## Week 3 Query 
-	New y value: 9.9 – new y_best
-	Keep beta at 1.5 to continue exploring

## 	Week 4 Query 
-	New y value: 9.7 
-	Although this doesn’t beat y_best, it’s still in a similar range. 
-	Optimiser has been exploring relatively nearby promising region, so I increase beta to 2 to encourage discovery of other promising areas.
  
## Week 5 Query 
-	New y value: 9.5 
-	Although I increased beta, the optimiser still sampled a relatively nearby region. 
-	To try to push the optimizer away from this region, I increase beta to 3. 
-	I also increase length scale upper bound from 2 to 3 due to convergence warning. 

## Week 6 Query 
-	New y value: 9.7 
-	Although x inputs variably different from the current y best, another high scoring region has emerged, suggesting that another peak has been discovered or there is a broad well-performing basin. 
-	Increase beta to 4 to continue exploration 

## 	Week 7 Query
-	New y value: 8.9 
-	This is relatively distant again from well performing regions. Again, suggesting a broad good basin or many local peaks. 

## Week 8 Query 
-	New y value: 9.4 
-	Potentially another peak or potentially connected to the y best region.
-	At week 8, I’ve either identified multiple good peaks or one stable region. Therefore, I begin to exploit. 
-	I decrease beta to 3.5 

## Week 9 Query
- New y value: 9.2 
- Potentially another peak or potentially connected to the y best region.
- This week, I have added a PCA plot to visualise my data. It provides a visual summary of sampled points in the high dimensional search space. Points are coloured by objective value allowing identification of promising regions and broad patterns. 
- The PCA plot of function 8 demonstrated a promising region with a cluster of 5-8 good points within the 44 points sampled so far. This prompted me to consider using an SVM classifier to qualify any further iterations, ensuring the come from the ‘good’ vs. ‘bad’ region. However, I realised that with only 44 points, SVM may be sensitive to small changes in the data. This could risk cutting off nearby regions. Another issue is that In Function 8, the PCA projection revealed a smooth gradient in objective values across the search space. Performance transitions gradually from high to medium to low rather than forming a clearly separated cluster of strong points. There is no clear boundary between good and bad regions. Any threshold used to define “good” versus “bad” points would therefore be somewhat arbitrary. But points just below the threshold (e.g. y=8.9) may still lie close to the optimum and contain useful information about the gradient of the landscape. Filtering them out could prevent the optimiser from exploring parts of the slope that might lead to the global maximum. I decided to continue without it. 
- Again, I’ve either identified multiple good peaks or one stable region. Therefore, I begin to exploit. 
- I decrease beta to 2.5 

## Week 10 query 
-	New y value 9.8030475428424
-	Nearby y best 
-	Not too far off y best of 9.9. 
-	Given the repeated high values found over recent iterations, and with only a few evaluations remaining, I now switch from UCB to Expected Improvement to focus on local refinement around the incumbent. I use a small Xi of around 0.05 to prioritise exploitation while still allowing slight flexibility in this high-dimensional setting (especially because there are multiple local peaks)

## Week 11 Query: 
-	New y_value  9.8174729542709
-	Again nearby y best, repeated strong values near here
-	Reduce Xi to 0.02

## Week 12 Query: 
-	New y value: 9.8673964200306
-	Although it does not beat y_best it’s still in a stable, well performing region and since it have been exploiting I’ve seen consistent improvement. 
-	I reduce xi down to 0.01. 
-	Shared length scale across 8 dimensions could be too restrictive. I switch to using ARD / anisotropic length scales, which optimise length scale for each dimension separately. 
-	Increase random candidates from 5000 -> 10,000 – near end of optimisation process 5000 too coarse for local refinements. 
-	Increase upper noise bound to 1. 

## Week 13 Query: 
-	New y value: 9.867044314756
-	Keep Xi =0.01 
-	Result 9.8684756342581
