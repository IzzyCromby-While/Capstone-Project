Capstone-Project

### Project Overview 

This is a black box optimisation challenge undertaken as a part of a Professional Certificate in Machine Learning and Artificial Intelligence with Imperial College London. The objective is to maximise a set of unknown functions over a thirteen-week period using only one evaluation per week. 

The challenge is designed to reflect real world machine learning scenarios where models must guide decision making under uncertainty and limited evaluation budgets. By developing and iteratively refining my optimisation strategies as new data becomes available each week, I am gaining practical experience in implementing optimisation workflows, building and updating models, and translating results into clear technical decisions. 


### Inputs, Outputs 

There are eight black box functions with input dimensionalities ranging from two to eight. Each query consists of continuous inputs in the range 0 to 1 and each evaluation returns an output value representing performance. The functions are inspired by real-world optimisation scenarios, ranging from contamination detection, optimally placing products across warehouses and hyperparameter tuning problems. Detailed descriptions of each function, their inputs and outputs are provided in separate READ_ME files. 


### Constraints and Limitations 

-	Unknown function structure means optimisation must rely entirely on observed inputs and outputs. 
-	Limited number of evaluations makes every query decision costly and requires careful prioritisation of informative evaluations. 
-	Noise in observations makes it difficult to distinguish true performance from random variation, increasing the risk of misleading early results. 
-	High dimensionality in some functions means exhaustive exploration is infeasible. 
-	Function surfaces are likely to be non-linear and multi-modal increasing the risk of premature convergence. 


### Technical Approach 

I have chosen Bayesian optimisation as the main method of optimising, as it supports uncertainty-aware decision making well. As part of this approach, Gaussian Process surrogate models are used to approximate the structure of the underlying function. Acquisition functions have been chosen based on dimensionality and the expected surface of the function. Heuristics and simpler approaches such as classifiers are used for intuition when making decisions but have not been used as primary optimisation tools. 

The core consideration driving my approach is the trade-off between exploration and exploitation. Early queries focus on exploration to reduce uncertainty across the input space, with a gradual shift towards exploitation as promising regions emerge. However, my strategy remains flexible. I will allow early probing of strong candidates before returning to exploration, helping to validate or disqualify hypothesises and use queries efficiently. 

More details on the exact technical approach for each function are available in the individual READ_ME files. 

