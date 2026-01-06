##Function 5
•	Input: 4D array 
•	Output: 1D array
•	Goal: Maximise 
•	Current y_best: 1088.86
•	This is a four-variable black box function representing a chemical process in a factory. The function is typically unimodal, with a single peak where yield is maximised. Your goal is to find the optimal combination of chemical inputs that delivers the highest possible yield. The function represents a stable chemical process with a typically smooth, unimodal landscape. As such, the underlying response surface is expected to vary gradually across the four input dimensions. Small parameter changes should yield correspondingly small shifts in output.

##Model Architecture 
The Bayesian optimisation framework consists of a GPR surrogate model with a RBF kernel and EI acquisition function. 
-	GPR to provide calibrated uncertainty estimates essential for exploration vs. exploitation trade-offs. 
-	RBF will be able to model the smooth nature of the model 
-	EI is used because the function is smooth and unimodal, so it efficiently balances exploration and exploitation by considering both the likelihood and size of potential improvements. 

##Original Hyperparameters 
-	Noise is set to 0.01 because this a chemical process with little noise. 
-	Length scale is set to 0.5 to reflect the expected smooth gradual unimodal increase. 
-	Initial xi term of 0.1 

Week 1 query 
-	Xi set to 0.1 to balance exploration and exploitation. 
-	There is one unimodal peak so it is better to exploit that than explore globally. 

##Week 2 query 
-	New y_value: 3681 – significantly better than y_best 
-	Xi is changed to 0.05 to exploit this peak 

##Week 3 query 
-	New y_value: 6886 – significantly better again 
-	Keep Xi small to focus on exploiting 

##Week 4 query 
-	New y_value: 7915 – significantly better again 
-	Keep xi the same since it has consistently improved results. 

##Week 5 query 
-	New y_value: 4040 – worse than other results 
-	I’ve identified a strong optimum at the edge of my search space (week 4 result). Moving away from this boundary has resulted in a significant drop in performance. To confirm that the optimum is near the boundary of the search space, and start to refine my results, I keep the acquisition settings unchanged and systematically perturb one input at a time while holding the others at their optimal values. This targeted boundary refinement allows me to assess the sensitivity of each parameter and verify whether the optimum is truly boundary-limited or whether small adjustments can yield further improvements.
-	I will adjust length scale since I have a convergence warning, moving it from 2 to 3. 

##Week 6 query 
-	New y value: 7739 – does not beat y_best 
-	Dropping x1 from 0.99 to 0.98 causes a moderate decrease, supporting assumption that the true maximum lies at or beyond the boundary. 
-	Now drop x2 from 0.99 to 0.98

##Week 7 
-	New y value: 7739 – same result as when we dropped x1 
-	Again, dropping x2 caused a moderate decrease in y value, supporting assumption that the true maximum lies at or beyond the boundary. 
<img width="468" height="641" alt="image" src="https://github.com/user-attachments/assets/afb9fa9f-c97c-4d5d-b4fd-3b3cce6444de" />

