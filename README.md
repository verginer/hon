# HON
Code for generating Higher-order Network (HON) from data with higher-order dependencies

## Requirements
Use Common Lisp to compile and run. SBCL recommended.
http://www.sbcl.org/
Some libraries may require Quicklisp.
http://www.quicklisp.org/

_A python implementation is under development. If you are interested, please let me know._

## Workflow
### Rule extraction
Use build-rules.lisp to extract rules from trajectories (or other types of data).
This corresponds to Algorithm 1 in paper.
Run function (main)
### Network wiring
Use build-network.lisp to convert rules into High Order Network (HON) representation.
This corresponds to Algorithm 2 in paper.
Run function (main)

## Synthesized data
### Note
We use a new way to synthesize the data to allow us adding higher order rules during movement synthesis (it is not feasible to add rules beyond third order using the same setting in paper, because movements matching multiple previous steps will unlikely happen). 
### Script
The script synthesize-trace-mesh.py (run with python or python3) synthesizes trajectories of vessels going certain steps on a 10x10 mesh (i.e. grid).
10,000,000 movements will be generated by default.
Normally a vessel will go either up/down/left/right with the same probability (see function NextStep)
(vessel movements are generated almost randomly with the following exceptions)

We add a few higher order rules to control the generation of vessel movements:
2nd order rule: if vessel goes right from X0 to node X1 (X means a number 0-9),
then the next step will go right with probability 70%, go down 30% (see function BiasedNextStep).
3rd order rule: if vessel goes right to node X7 then X8, then the next step will go right with probability 70%, go down 30%.
4th order rule: if vessel goes down from 1X to 2X to 3X to 4X,
then the next step will go right with probability 70%, go down 30%.
There are ten 2nd order rules, ten 3rd order rules, ten 4th order rules, no other rules.

### Data
Data generated using the aforementioned script is provided as traces-simulated-mesh-v100000-t100-mo4.csv

### Rule extraction with HON
Use HON with a default setting of MaxOrder = 5, MinSupport = 5
It should be able to detect all 30 of these rules of various orders,
And will not detect "false" dependencies such as 5th order rules.
The result is provided as rules-simulated-mesh-v100000-t100-mo5-ms5-t01.csv, note that for illustration here (since we are not going to build a network now), in this file when higher order rules are detected, corresponding lower order rules are not added recursively.
If all preceding rules are added, following the algorithm in the paper, the expected result should be rules-simulated-mesh-v100000-t100-mo4-kl.csv.
The whole time to process these 10,000,000 movements should be under 5 seconds (unoptimized code, on single core, excluding disk IO).


## Contact
Please contact Jian Xu (jxu5 at nd dot edu) for any question. 
_A python implementation is under development. If you are interested, please let me know._
