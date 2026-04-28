# Datasheet 

## Motivation
This dataset was created to support a black-box optimisation capstone project within the Imperial College London Professional Certificate in AI and ML. The goal of the project is to maximise eight unknown functions using only thirteen queries. The dataset records a set of original input queries and output values for each function, which have been added to weekly as a part of the optimisation process. 
It was created by the Imperial College London teaching team to support this exercise and is inspired by the problems from the NeurIPS 2020 Black-Box Optimisation Challenge.

## Composition
For each function the dataset contains an initial set of query points and outputs provided as part of the challenge and additional queries which have been added weekly as part of the optimisation process. They range in length from 20 – 50 samples per function. 
There are eight functions in total with input dimensionalities ranging from two to eight variables. All inputs are continuous variables ranging from zero to one and each query returns a singe output value. 
The data is stored in a tabular format with a separate sheet for each function. Each row represents one query and data fields include, ‘Iteration’, Inputs’ and ‘Outputs’. 
The dataset represents a limited sample of the surface of each function and coverage is likely to be uneven due to the nature of the black-box optimisation process, where ‘promising regions’ are exploited and queried more. There are no missing values. 

## Collection process
On top of the initial set of data provided as part of the challenge, queries were generated sequentially as part of the black-box optimisation process. Each week a new query was selected based on the results from the previous week. A range of methods were used to choose the next query point including Bayesian Optimisation, a support vector classifier and manual querying. Each iteration had the aim of identifying higher performing regions of the search space. Generally, early queries prioritised exploration while later queries exploited ‘promising regions.’ Data collection took place over a thirteen week period, with one query added every week. 

## Preprocessing
No significant preprocessing was applied to the dataset. The raw data is preserved in a tabular format without transformation. 
 
## Uses
The dataset is intended to support the iterative process of evaluating queries week-by-week and record the black-box optimisation process as part of the capstone project. 
The dataset should not be used as a complete representation of the underlying functions as it only represents limited samples. 

## Distribution
The dataset is available through the GitHub repository associated with the capstone project. It is shared online for marking and peer review as part of the certificate course. 

## Maintenance
The dataset will be maintained by me (Izzy) throughout the capstone project. After the final submission, it will not be maintained as it is not an ongoing project. 
