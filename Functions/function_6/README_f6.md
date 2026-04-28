# Function 6
-	Input: 5D array 
-	Output: 1D array
-	Goal: Maximise 
-	Current y_best: -0.7 
-	You’re optimising a cake recipe using black-box function with five ingredient inputs, for example, flour, sugar, eggs, butter and milk. Each recipe is evaluated with a combined score based on flavour, consistency, calories, waste and cost, where each factor contributes negative points as judged by an expert taster. This means the total score is negative by design. To frame this as a maximisation problem, your goal is to bring that score as close to a zero as possible or to maximise the negative. The function landscape is expected to be relatively smooth and continuous: small changes in ingredient ratios are likely to produce gradual changes in the final score. However, the presence of human judgement could introduce some noise and irregularities into the surface. 

## Origional Model architecture 
-	The Bayesian optimisation framework consists of a GPR surrogate model with RBF and Kernel and an EI acquisition function. 
-	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	RBF kernel captures the smooth and continuous relationship expected between ingredient ratios and overall score. White kernel accounts for noise introduced by human subjective judgement in scoring process. 
-	EI is picked as it is effective for a relatively smooth area. 

## Original Hyperparameters 
-	Small noise of 0.1 to account for the relatively low to moderate noise we expect from human judgement
-	Length scale of 0.5 as the model is expected to be relatively smooth. 
-	Xi = 0.2 to initially explore 

## Week 1 Query
-	Since the function is expected to be relatively smooth, I can afford to prioritise exploitation around the current best region more than previous functions. At the same time, because there’s a possibility of some noise and no reason why the function can’t be multimodal, I chose a moderate Xi to balance exploration and exploitation. 

## Week 2 Query 
-	New y value: -0.69 – new y_best 
-	Improvement, but still early in process so keep Xi at 0.2 to balance exploration and exploitation. 

## Week 3 Query 
-	New y value: -0.56 – new y_best  - very similar x values to current best. 
-	Exploit the current high performing region slightly to probe the region further. 
-	Xi=0.1

## Week 4 Query 
-	New y value: -0.56 – very similar y value – slightly worse 
-	The Week 4 result is very similar to the current best despite noticeably different ingredient proportions, which suggests we either have a broad well-performing region OR noise masking hidden structure. 
-	It is too early to converge, so I increase exploration from 0.1 to 0.15. This allows the optimiser to stay in the high-scoring region while still probing nearby combinations in case a better configuration exists within this broad plateau or elsewhere, balancing exploration and exploitation. 

## Week 5 query 
-	New y value: -1.02 significantly worse y value 
-	The boundary heavy exploration step produced a substantially worse outcome. 
-	Given the clear contrast between the high performing region and poorer alternatives, I reduce xi to refocus sampling near the promising area while retaining some limited exploration. 
-	Drop Xi back down to 0.1 for a balance of exploration and exploitation. Exploitative enough that the well performing region will be considered, but it will test other combinations keeping some inputs constant too. 

## Week 6 Query 
-	New y value – -0.6 : worse than y_best 
-	The last query balanced exploitation with exploration, having a boundary term as part of the query. This additional exploration hasn’t improved, motivating me to exploit more aggressively. 
-	This time, I reduce xi down to 0.08 to focus on probing the clear promising region. 

## Week 7 Query
-	New y best: -0.1 y best !  
-	I will exploit this further and reduce xi down from 0.08 to 0.05 
-	There is also a noise convergence warning, so I drop the noise lower bound from 1e-4 to 1e-10. 

## Week 8 Query
-	New Y value: -0.65 – worse than Y_best despite exploiting 
-	May be due to optimising on coarse grid, so switch to using random candidates 
-	Reduce Xi down from 0.05 to 0.03

## Week 9 Query 
-	New Y value: -0.3 – not y_best but nearby
-	Although, with more exploitation, I would’ve thought that the y_best would improve. I’m concerned that the -0.1 y_best may have been a noise spike. However, my PCA projection that I just implemented demonstrates that points nearby to this one also perform well in comparison to the rest of the search space, which means this point is actually less likely to be noise than I originally thought. 
-	I’m going to probe the region near the current y_best region to test whether it is noise or not. I will do this by adding 0.03 to existing x inputs for that query. 

## Week 10 Query 
-	New y_best:  -0.1506112686391128 
-	Supports region found is genuinely strong and not noise spike 
-	With only a few iterations remaining, I will continue local refinement and reduce Xi to 0.02 to focus more strongly on exploitation

## Week 11 Query 
-	New y value: -0.45179526302238504
-	Week 10 query very close to y best but output very different, indicating sharp peak. 
-	Reduce Xi to 0.01 
-	Optimiser proposed points are distant from y best basin, (0.346517, 0.217165,0.188469,0.849166,0.195162). I reject this point since it’s final rounds of iteration. Mindful it’s a cake recipie so boundary points less likely to perform well. 
-	Manually probe adding 0.01 to each of the x values that resulted in my y best.

## Week 12 Query
-	New y value: -0.169881494914663
-	Close in parameter space and output to y best. 
-	Manual query between two best performing points so far, query 6 and 9. 

## Week 13 Query
-	New y value: -0.143412680521317 – new y best by small amount. 
-	Continue pattern moving between query 9 and 6, -0.01 from current y best. 
-	Result -0.221842271210347


