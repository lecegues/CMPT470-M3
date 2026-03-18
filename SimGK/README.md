# SimGK – Reproducibility Report

## 1. Artifact Discovery
- The paper was found by simple google search: https://dl.acm.org/doi/10.1145/3769305
- However, a repo was not discovered, nor any binaries for code execution.
- I attempted to find a repo for SimCL/SimGK but couldn't find a project under either name
- I also checked the authors of the paper

Description of the tool:
  - This tool is a multi-language clone detection tool that converts languages like python, java and C++ into a neutral language for fair comparison via ULR(Unified Intermediate Representation)
  - It looks at the flow of logic of the program by building a unified control flow graph, and then compares graphs to find simularities in logic.
  - It is used to find code clones and expose vulnerabilies, even across languages.
  - the paper describes some very impressive results, which can be found in the results table, reporting recall of 0.95 and precision of 0.97

## 2. Environment Setup
N/A

## 3. Installation and Execution Steps
N/A

## 4. Benchmarks Used
- Since this is a cross-language tool, GoogleCodeJam and GPTCloneBench cross-language section would have been used

## 5. Interventions Performed
- N/A

## 6. Execution Outcome
- N/A

## 7. TES Classification
- TES-D(Non-Executable)
- the git repo could not be obtained
