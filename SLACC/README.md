# SLACC – Reproducibility Report

## 1. Artifact Discovery
Official Artifact Link: 
https://github.com/DynamicCodeSearch/SLACC?tab=readme-ov-file

How it was discovered:
SLACC Artifact was found on GitHub using the paper title & tool name.

Verification Notes:
This is verified to be the official GitHub because they link the study (https://github.com/DynamicCodeSearch/SLACC/blob/ICSE20/SLACC_preprint.pdf) 
- The author of the repository is the author of the official paper (George Mathew)

Note:
The last commit was 6 years ago. The README also references a preconfigured VirtualBox image hosted on Google Drive,
but it is expired. Therefore, I assume there has been no updates to the project itself.

## 2. Environment Setup
Initial Attempt 1:
The README recommended the easiest way to run the project: a preconfigured VirtualBox image.
BUT the download link was expired and unavailable.

Initial Attempt 2:
Next, I considered using the University Linux VM, but I could not use it as I had no sudo privileges to install 
some of the dependencies

Final Environment Used:
I ended up using my local machine's WSL.
- Windows 10 Pro (10.0.19045 Build 19045) with Ubuntu 22.04.5 LTS running through WSL 2.6.3.

Dependencies:
- WSL Ubuntu
- Java 8 (JDK 1.8)
- Maven
- Python 2.7
- MongoDB

## 3. Installation and Execution Steps

### 1. Installing Dependencies
First, I installed the required dependencies in my WSL Environment:
- Java 8: `sudo apt install openjdk-8-jdk -y` 
- Maven: `sudo apt install maven -y` 
- Python 2.7: `sudo apt install python2 -y`

Then, I had to install the Python dependencies from the codebase
`pip2 install -r requirements.txt` 

Then, I had to install the Java dependencies from the codebase
`mvn clean install` 

### 2. Maven Build
The first Maven build in the `code/` folder failed after about 33 minutes because the `projects` module had not been installed locally.
(refer to `BUILD1_FAIL.txt`)
Note: There was no top-level `pom.xml` file so I had to build them separately (first `projects`, then `code` modules)

So, running the build for `projects` took about 5 minutes.
(refer to `BUILD2_SUCCESS.txt`)

Then, I had to build the `code/` folder which took another 29 minutes.
(refer to `BUILD3_SUCCESS.txt`)

### 3. Initialization
First, I had to initialize the mongo database with example data using:
`sh scripts/common/initialize.sh Example`

This script triggered a FULL REBUILD of both the `projects` and `code`
(refer to `BUILD4_SUCCESS.txt`)

Although the build succeeded, running the code had an error, `scripts/common/initialize.sh: 31: python: not found`.
This was because `python` command was used, rather than `python2` command, which was confusing since Python2 was the dependency.
So, I linked the `python` keyword to `python2` using `sudo ln -s /usr/bin/python2 /usr/bin/python`.

However, running the script again caused it to rebuild, but I could not wait that long, therefore I commented out the `mvn clean install` part
of the BUILD files which was unnecessary since I manually built them already. Then, it ran instantly.

### 4. Execution
Before running execution, I had to update path settings in the relevant files:
- `ROOT_PATH` in `Settings.java`
- `ROOT_HOME` in `properties.py`

Then, I ran it using `bash scripts/common/runner.sh Example`.
It ran, however there were many errors including a MongoDB version incompatibility which was interesting as the
instructions say "We use MongoDB 3.6 for our experiments but it should work on later versions as well."

Java execution also failed since some classes were not found.

Refer to #6. "Observed Failures" for more information and `RUN_LOG.log`

## 4. Benchmarks Used
Primary Benchmark (intended by paper):
- Google Code Jam dataset (cross-language between Java and Python)
Note: Due to execution failures during the pipeline stages, I could not execute it on this benchmark.
Therefore, the `Example` dataset which was included in the official GitHub repository was used only for smoke testing and validation.

Settings Used:
Default settings were used. Other than changing the scripts to not build everytime (as it took too long), there were 
no algorithmic changes.

## 5. Interventions Performed
I had to perform a couple interventions since :
1) the "easier" method was not working and had to set up my environment manually
2) there were version errors and some difficulties with the instructions

1. Switched from unavailable VM image to manual setup (since it wasn't available)
2. Switched from university VM to local computer WSL (since no sudo perms)
3. Built the `projects` module before the `code` module as it was a dependency
4. Linked `python` keyword to `python2` because scripts expected `Python` but dependency was Python 2.x
5. Modified `initialize.sh` script as it was coded to run `mvn clean install` after each time which was redundant (took 30mins each time)
6. Modified `runner.sh` script as it was also coded to clean build after each time too (2x actually)
7. Hardcoded root path settings in `Settings.java` and `properties.py` as it was set incorrectly for my environment

## 6. Execution Outcome
SLACC was partially executable in my environment.
Although I could execute it, just not fully.
(refer to `RUN_LOG.log`)

Succesful Steps:
- Official artifact found and verified
- Downloaded required dependencies (as stated in README)
- Succesfully built the project folders
- Succesfully initialized dataset
- Execution pipeline started the Java and Python stages 

Failures:
- Java stages failed due to MongoDB incompatibility and `ClassNotFoundException` for generated Java classes
- Some scripts had bash/sh compatibility issues
- Python stages went further BUT no valid functions for clusters
- Clustering executed BUT failed with 0 inputs
- Log files in "meta_results/Example/clusters/cluster_testing/eps_0.01/" were created, but all empty

## 7. TES Classification
Overall, the tool didn't complete the full workflow BUT major parts of the workflow did execute
Therefore, I rate it as a "TES-C (partially executable)"

Justification:
SLACC artifact found, installed, built, partially executed. Just didn't complete the full workflow succesfully.

## 8. Reproducibility Issues
Summary of issues impacting ability to reproduce causing a TES-C: 
1. Artifact Age
Last commit was ~6 years ago, making me believe that environment assumptions are outdated

2. Deprecated MongoDB driver
SLACC codebase uses legacy MongoDB commands, which is not supported in the MongoDB version I used.
Additionally, those older MongoDB versions are unavailable with my current WSL Ubuntu version.

3. Python2 Dependency
It requires Python 2.7 which is deprecated

4. Missing VirtualBox image
Artifact documentation recommends preconfigured VM image, but download link is expired
