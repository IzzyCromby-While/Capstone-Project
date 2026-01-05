##Function 1 
•	Input: 2D array 
•	Output: 1D array
•	Goal: Maximise 
•	Current y_best: 7.71E-16
•	Function 1 represents a two-dimensional contamination detection problem, where the objective is to identify the coordinates corresponding to the highest radiation reading. The function output (y) represents the detected intensity, which increases near the contamination source but remains near zero elsewhere. 


##Model Architecture
The Bayesian optimisation framework consists of a GPR surrogate model with an RBF Kernel and UCB acquisition function. 
•	GPR surrogate model chosen due to its strong performance in low-dimensional spaces and ability to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
•	RBF Kernel was chosen to model a smooth contamination field where radiation intensity typically changes gradually rather than suddenly.
•	UCB Acquisition Function was chosen was chosen as initial strategy is exploration which this acquisition function supports. 

##Original Hyperparameters 
•	Noise is set to 1e-3 as the data includes non-physical negative values and tiny numerical fluctuations, so a higher noise level is used to ensure the GP treats these as noise rather than contamination structure. 
•	Length scale range is set to 0.1-5, because the contamination field should vary smoothly. The intial data contained non-physical noise that caused the GP to collapse and the length scale to become tiny. Enforcing a larger range prevents this overfitting and ensures the model reflects the true smoother behaviour of a contamination field. 
•	Beta was set at 15 because the noisy radiation data forced the model to underestimate uncertainty preventing exploration. Beta was set to a higher value to counter this. 

##Week 1 Query 
•	Initial readings show no significant variation in y-value / radiation intensity. The majority of values are extremely close to zero. This suggests that the sampled points are located in a low-radiation region. 
•	For week 1, the focus should therefore remain on broad exploration to better understand the 2D landscape to improve the likelihood of detecting a region with non-zero readings.

##Week 2 Query 
•	New y value: -1.58147478428314E-92 – much worse than y_best 
•	Beta: 16 increase exploration to continue to explore the 2d surface 

##Week 3 Query 
•	New y value: 1.35826688617176E-152 – much worse than y_best 
•	No signal detected so far, so I’m deliberately staying in exploration mode, keeping hyperparameters the same to explore areas of uncertainty. 

##Week 4 Query 
•	New y value: 1.8647707194978913-163 – worse again 
•	Still exploring largely flat, low-signal regions of the contamination field, continue to prioritise exploration 

##Week 5 Query
•	New y value: 0 – worse 
•	After sustained exploration, I’m yet to find a meaningful peak and therefore I will reevaluate my approach. 
•	First, I will change my length scale and noise levels due to convergence warnings. My original assumption was that radiation intensity should change smoothly in space and the tiny variations are not real signal but noise. However, convergence warnings indicate that these assumptions were enforced too rigidly. I will therefore relax the bounds on length-scale and noise. 
-	Change length scale bounds from 0.1-5 to 1e-3-5
-	Change noise level bounds from 1e-6 – 1e-1 to 1e-6 – 1 
•	Initially, I used a large constant Beta to encourage exploration. However, I realised that this value was outside of the theoretical GP-UCB formulation, meaning uncertainty dominated the acquisition function, causing exploration beyond what the model’s uncertainty justified. Therefore, I change my beta to 5.8 based on the GP-UCB confidence parameter. 
•	The proposed points, 0.610000, 0.860000, were nearby to regions already sampled and therefore I applied a minimum-distance rule (0.05) corresponding to approximately 5 grid steps. 

##Week 6 Query
•	New y value: 9.43801108841813E-80 still flat 
•	Sensible to stay in exploration mode, with the UCB 5.9 and keep enforcing the minimum distance rule so every sample is exploring a new special information. 

##Week 7 
New y value: 
•	I found a bug! I had written, idx = np.argmax(X_grid[mask]), instead of idx=np.argmax(ucb[mask]). So, I’d stopped picking optimum points based on the acquisition function, rather I was choosing highest values on the grid. I’ve fixed this now. 
<img width="468" height="638" alt="image" src="https://github.com/user-attachments/assets/755e7496-ab25-47e0-a2f1-ebdda8f4ab86" />

