# CLCDSA – Reproducibility Report

## 1. Artifact Discovery
- The artifact dicovery was quite simple, all was found by searching on google.
 - github repo found: https://github.com/Kawser-nerd/CLCDSA
 - paper found: https://clones.usask.ca/pubfiles/articles/Nafi_CLCDSAASE2019.pdf

## 2. Environment Setup
- no explicit requirements found for OS, run on Windows 11
- The README mentions that its best to train the model with CPU instead of GPU
- missing from the README was instructions for installing dependencies, python version, and more.
- once I found a testing file that looked like I could attempt to run it, I tried to configure a venv for execution
- I installed python 3.8, and seemingly necessary dependencies(TensorFlow, Keras) to prepare the enviroment

## 3. Installation and Execution Steps
- When I cloned the repo, and tried to figure out how to use the tool. The README offers a fairly vague and brief description: 'he trained model in saved both as .h5 and the weight model format under CLCDSA_DLCC. A to test it one needs to configure the data first and then need to load the model.'
- as such, I wasn't able to find what I needed to run the tool
- I managed to find CLCDSA-master/CLCDSA_DLCC/clcdsa_testing.py, which I thought might be what I need to use to utilize the tool. But I also needed some specific csv files to get the file running
- I did find some possible examples for datafile csv at CLCDSA-master/DataFeatures which seems to use data from LCDSA-master/Source Codes/AtCoder
- in the end I wasn't able to discern how to use the tool
- I couldn't find the script to run ANTLR
- I couldn't find out how to generate the parse trees needed, and those provided seemed to have context dependecies for a local machine
- It may be possible to reproduce the results put forward in the paper, but the lack of instructions/information in the README made it extremely diffcult with altering code(especially when I don't know what to alter)

## 4. Benchmarks Used
- None
- If I had gotten things running, I would have used the tool on a simple dataset first to test it, then I owuld have used GoogleCodeJam and GPTCloneBench cross-language section.

## 5. Interventions Performed
- Since I had little to no instructions, I had very little direction as to what file(s) to alter for running the tool, and so I made no changes to repo files

## 6. Execution Outcome
- No execution

## 7. TES Classification
- TES-D (Non-Executable)
- it may be possible to run this tool to reproduce the paper's results, but the lack of instruction and general information makes this extremely difficult to reproduce.
