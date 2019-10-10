
# SBD Group 35 Report 



Abdul Arkhan () 

Andrea Nardi () 

Andrew Gold (4995902) 





--- 



## Assignment Description 

For this lab, we would like you to process the entire dataset, meaning all segments, with 20 c4.8xlarge core nodes, 
in under half an hour, using your solution from lab 1. 

This should cost you less than 12 dollars and is the minimum requirement. 



Note that this means that you should evaluate whether you application scales well enough to achieve this before attempting to 
run it on the entire dataset. 

Your answers to the questions in lab 1 should help you to determine this, but to reiterate: consider e.g. how long it will 
take to run: 



- 1000 segments compared to 10000 segments? 

- on 4 virtual cores compared to 8, and what about 32? 



If your application is not efficient enough right away, you should analyze its bottlenecks and modify it accordingly, or 
try to gain some extra performance by modifying the way Spark is configured. 

You can try a couple of runs on the entire dataset when you have a good understanding of what might happen on the entire dataset. 



For extra points, we challenge you to come up with an even better solution according to the metric you defined in lab 1. 

You are free to change anything, but some suggestions are: 



- Find additional bottlenecks using Apache Ganglia (need more network I/O, or more CPU?, more memory?) 

- Tuning the kind and number of machines you use on AWS, based on these bottlenecks 

- Modifying the application to increase performance 

- Tuning Yarn/Spark configuration flags to best match the problem 





--- 



## Setup and Evaluation 



(*Here we can describe how we evaluated our algorithm(s) on small sets of data (100 to 1000 files) and see if it scales linearly. 
We can discuss then the feasibility of running the entire dataset, given our estimations (via linear regression) for the whole 
dataset. If this estimated time taken exceeds the time limit (30 minutes for >143000 files) then we need to change our 
implementation.*) 





At this stage, we conducted primary testing of several algorithms on very small numbers of documents (<= 1000) to determine 
if there was any significant discrepancy between our various implementations. We have 3 primary algorithms, and 1 serialized 
version of each algorithm (via Kryo), totaling in 6 implementations. 



Initial testing indicates that serializing our classes results in a very miniscule (almost negligible) increase in efficiency, 
however this has been tested on <1% of the total cases. Therefore, we decided that these results cannot be accepted as fact. 



However, in order to determine if our algorithms would run in under 30 minutes on the required number of nodes, we decided to
pick our fastest algorithm (in this case, "Windows_no_kryo") and scaled it up incrementally to determine if its runtime grew 
linearly, or otherwise. 



For a number of files between 1000 and 20,000, it seemed as if the runtime did indeed scale linearly. Our linear regression 
predicted that at this rate, for the full number of files (143435) in the GDELT dataset, our implementation would not run 
in the required time frame. However, after testing 30,000 files, there seemed to be a dropoff in runtime, indicating a 
possible logarithmic growth in runtime. Therefore, in order to confirm this, we ran 50,000 files to see if the 30,000 
runtime was an outlier, or if there was indeed a positive indication of logarithmic runtime growth. 



The results show...
