# Transclone – Reproducibility Report

## 1. Artifact Discovery
Official artifact link how the artifact was discovered, verification notes

Link to artifact: https://github.com/subrotonpi/transclone.git

How artifact was discrovered: Found documentation and link to the git hub in this article on IEEE Xplore https://ieeexplore.ieee.org/abstract/document/10473630

For verification the publisher added our prof as part of the listed authors: S. N. Pinku, D. Mondal and C. K. Roy, "TransClone: A Language Agnostic Code Clone Detector," 2023 IEEE 17th International Workshop on Software Clones (IWSC), Bogotá, Colombia, 2023, pp. 29-32, doi: 10.1109/IWSC60764.2023.00012. keywords: {Software maintenance;Java;Codes;Conferences;Cloning;Detectors;Maintenance engineering;Code Clone Detection;Cross-Language Clones;Software Maintenance Tools},


## 2. Environment Setup
Operating system, programming language & version, dependencies, hardware (if needed)

OS: Windows 11

Programing Language: Java,Python

Dependencies: both client and dev version of [srcML](https://www.srcml.org/#download)

## 3. Installation and Execution Steps
Installation commands used, execution commands used

Used `wsl --install` to create a Linux environment (Ubuntu 24.04)

`sed -i 's/\r$//' setup.sh`: Removes hidden carriage-return characters (\r) at line ends

Created Python virtual environment on the Linux filesystem instead of the OneDrive path to avoid slow file I/O: `python3 -m venv ~/transclone-venv` then `source ~/transclone-venv/bin/activate`

`setup.sh` uses `pip` which was not available. Installed dependencies manually using `pip install` for each package group. torch 1.13.0 is incompatible with Python 3.12 so the latest CPU version was installed instead. numpy was downgraded to <2 as required by the project. `slimit` required `sudo apt-get install -y 2to3` before it could be built.

The srcML server referenced in `setup.sh` (131.123.42.38) was unreachable. Downloaded srcml_1.1.0 `.deb` manually from https://www.srcml.org/#download and installed with `sudo dpkg -i`, then ran `sudo apt-get install -f` to resolve missing dependencies.

TXL was not available via apt. Downloaded and installed manually from https://www.txl.ca/txl-download.html using `sudo ./InstallTxl`.

Built NiCad with `make`. The `tools/` directory failed because `tpc` (Turing compiler) is unavailable on Linux, but the pre-compiled binaries were restored from git with `git checkout -- tools/`.


## 4. Benchmarks Used
Benchmark name(s), dataset size (if subset used), settings used

Benchmark: CLCDSA – GoogleCodeJam dataset from https://github.com/Kawser-nerd/CLCDSA

Subset used: Year 2017, Problem 01 (`CodeJamData/17/01/`) — 21 Java files and 19 Python files (40 files total)

NiCad extracted 112 Java functions and 14 Python functions from the subject system

Settings used: `--src_lang python --tgt_lang java --threshold 0 --cuda False`

## 5. Interventions Performed
Description of any fixes, dependency updates, configuration changes, or troubleshooting steps

`torch 1.13.0` listed in `requirements.txt` is incompatible with Python 3.12. Installed `torch 2.10.0+cpu` with matching `torch-geometric` wheels instead.

`fastBPE` had no wheel available for Python 3.12. Compiled the C++ extension from source at `codegen_sources/model/tools/fastBPE/` using `g++` then installed with `python setup.py install`.

The srcML download server hardcoded in `setup.sh` (131.123.42.38) was unreachable. Downloaded `srcml_1.1.0.deb` manually from https://www.srcml.org/#download and installed with `sudo dpkg -i` then `sudo apt-get install -f`.

TXL was not available via apt. Downloaded TXL 10.8b from https://www.txl.ca and installed with `sudo ./InstallTxl`.

NiCad's `tools/` build requires the Turing compiler (`tpc`) which is unavailable on Linux. Restored pre-compiled binaries from git with `git checkout -- tools/`.

`nicad6`, all files in `scripts/`, `config/default.cfg`, and `custom_nicad_pipeline.sh` had Windows-style line endings causing bash parse errors. Fixed all with `sed -i 's/\r$//'`.

`bytes_to_unicode` was removed in newer versions of `transformers`. Downgraded to `transformers 4.29.2` using `pip install --no-deps` with `tokenizers 0.15.2`. Patched `dependency_versions_check.py` in the installed package to suppress the resulting version mismatch error.

PyTorch 2.6 changed the default of `weights_only` in `torch.load()` from `False` to `True`, causing `_pickle.UnpicklingError` when loading the pretrained model. Added `weights_only=False` to all affected `torch.load()` calls in `codegen_sources/model/translate.py` and `codegen_sources/model/src/model/__init__.py`.

Spaces in the project path (`CMPT 470`) caused NiCad's internal `cp`/`mv` commands to fail. Worked around by running NiCad manually to pre-generate the XML files (`codejam_gcj17p01_functions_java.xml`, `codejam_gcj17p01_functions_py.xml`) in `storage/`.

`lxml` was required by `pandas.DataFrame.to_xml()` but not listed in `requirements.txt`. Installed with `pip install lxml`.

## 6. Execution Outcome
Did the tool run succesfully, did it complete the full workflow, any crashes or partial execution?

Transclone completed the full workflow successfully after the interventions listed in Section 5. All three phases (NiCad function extraction, TransCoder translation, GMN similarity detection) ran without fatal errors.

Results on the CLCDSA GoogleCodeJam 2017 Problem 01 benchmark:
- Clone pairs: 10 / 10 (100%)
- Non-clone pairs: 0 / 10 (0%)

The 100% clone rate is the expected result — all solutions in CLCDSA solve the same competition problem and are semantically equivalent cross-language clones by design.

Full logs and results are available in `output_log.txt`, `execution_commands.sh`, and `results.csv`.

## 7. TES Classification
Final TES category & justification

TES-B (Executable with Intervention)

The tool successfully completed the full intended workflow and produced correct outputs, but only after 10 non-trivial interventions including fixing PyTorch and transformers version incompatibilities, manually compiling fastBPE, sourcing unavailable dependencies (srcML, TXL), fixing Windows line endings in shell scripts, patching torch.load breaking changes, and installing a missing runtime dependency (lxml). The tool runs end-to-end but cannot be considered executable out of the box following the authors' original instructions.