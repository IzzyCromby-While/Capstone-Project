# Function 1 

## Overview 
- Input: 2D array 
- Output: 1D array
- Goal: Maximise 
- Current y_best: -0.003606063
- Function 1 represents a two-dimensional contamination detection problem, where the objective is to identify the coordinates corresponding to the highest radiation reading. The function output (y) represents the detected intensity, which increases near the contamination source but remains near zero elsewhere. 

## Original Model Architecture
- The Bayesian optimisation framework consists of a GPR surrogate model with an RBF Kernel and UCB acquisition function. 
-	GPR surrogate model chosen due to its strong performance in low-dimensional spaces and ability to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	RBF Kernel was chosen to model a smooth contamination field where radiation intensity typically changes gradually rather than suddenly.
-	UCB Acquisition Function was chosen was chosen as initial strategy is exploration which this acquisition function supports. 

## Original Hyperparameters 
-	Noise is set to 1e-3 as the data includes non-physical negative values and tiny numerical fluctuations, so a higher noise level is used to ensure the GP treats these as noise rather than contamination structure. 
-	Length scale range is set to 0.1-5, because the contamination field should vary smoothly. The initial data contained non-physical noise that caused the GP to collapse and the length scale to become tiny. Enforcing a larger range prevents this overfitting and ensures the model reflects the true smoother behaviour of a contamination field. 
-	Beta was set at 15 because the noisy radiation data forced the model to underestimate uncertainty preventing exploration. Beta was set to a higher value to counter this. 

## Week 1 Query 
-	Initial readings show no significant variation in y-value / radiation intensity. Most values are extremely close to zero. This suggests that the sampled points are in a low-radiation region. 
-	For week 1, the focus should therefore remain on broad exploration to better understand the 2D landscape to improve the likelihood of detecting a region with non-zero readings.

## Week 2 Query 
-	New y value: -1.58147478428314E-92 – much worse than y_best 
-	Beta: 16 increase exploration to continue to explore the 2d surface 

## Week 3 Query 
-	New y value: 1.35826688617176E-152 – much worse than y_best 
-	No signal detected so far, so I’m deliberately staying in exploration mode, keeping hyperparameters the same to explore areas of uncertainty. 

## Week 4 Query 
-	New y value: 1.8647707194978913-163 – worse again 
-	Still exploring largely flat, low-signal regions of the contamination field, continue to prioritise exploration 

## Week 5 Query
-	New y value: 0 – worse 
-	After sustained exploration, I’m yet to find a meaningful peak and therefore I will reevaluate my approach. 
-	First, I will change my length scale and noise levels due to convergence warnings. My original assumption was that radiation intensity should change smoothly in space and the tiny variations are not real signal but noise. However, convergence warnings indicate that these assumptions were enforced too rigidly. I will therefore relax the bounds on length-scale and noise. 
-	Change length scale bounds from 0.1-5 to 1e-3-5
-	Change noise level bounds from 1e-6 – 1e-1 to 1e-6 – 1 
-	Initially, I used a large constant Beta to encourage exploration. However, I realised that this value was outside of the theoretical GP-UCB formulation, meaning uncertainty dominated the acquisition function, causing exploration beyond what the model’s uncertainty justified. Therefore, I change my beta to 5.8 based on the GP-UCB confidence parameter. 
-	The proposed points, 0.610000, 0.860000, were nearby to regions already sampled and therefore I applied a minimum-distance rule (0.05) corresponding to approximately 5 grid steps. 

## Week 6 Query
-	New y value: 9.43801108841813E-80 still flat 
-	Sensible to stay in exploration mode, with the UCB 5.9 and keep enforcing the minimum distance rule so every sample is exploring a new special information. 

## Week 7 Query
-	New y value: -3.538204E-180
-	I found a bug! I had written, idx = np.argmax(X_grid[mask]), instead of idx=np.argmax(ucb[mask]). So, I’d stopped picking optimum points based on the acquisition function, rather I was choosing highest values on the grid. I’ve fixed this now. 

## Week 8 Query 
-	New y value: -3.5E-180 – still flat 
-	This could be to do with the bug I found so I will do another round with the same exploratory UCB but with the bug fixed. 

## Week 9 Query 
-	New y value: -1.25E-105 – still flat 
-	Since it’s week 9, I reduce UCB to 3. I have done multiple rounds of global exploration without yielding any better returns. I also reduce minimum distance rule. 
-	I have implemented a posterior mean visualisation to increase interpretability. Having done this, it’s easy to see my posterior mean is too flat.
-	Also accidentally used two noise assumptions Using both can make the GP overestimate noise, which can flatten the posterior mean. I fixed this. 
-	I check my code for bugs and found another. Fixed a bug where predictions and candidate selection were misaligned (used X_grid instead of X_candidates), causing incorrect indexing when selecting the next query point.

## Week 10 Query
-	New y value: -5.16447618383621E-34 – still flat 
-	New observation does not improve y_best, but since it follows corrections to GP and Acquisition code, it is a stabilisation phase rather than lack of signal. 
-	Reduce Beta to 2.5 to shift from broad exploration to balanced focus on moderately promising regions given limited iterations remaining. 

## 	Week 11 Query 
-	New y value: -1.36145674295435E-33 – still flat 
-	Does not improve on current y_best. Following corrections, results are more reliable. 
-	Signal clearly highly localised. Reduce Beta to 1.5 to prioritise exploitation moderately promising regions given limited iterations remaining. 

## Week 12 Query
-	New y value: -1.34995975470089E-20
-	Due to no improvement, used both UCB and EI to generate candidate points. UCB will give weight to under sampled areas (exploration) while EI will try to make improvements (exploitation). I choose candidate points based on novelty from current points which have only produced flat outputs. 
-	Rejected EI point (0.492500, 0.042031) – falls in an already heavily sampled low output region. Selected UCB (β = 1.0): (0.334463, 0.152813) – provides a novel evaluation. 

## Week 13 Query
-	New y value: 9.09143026949069E-57
-	Reduced Beta to 0.5 and accepted a new sample region
-	Result 5.32562551723011E-24



