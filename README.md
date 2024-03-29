# Using Deep Learning to Increase Eye-Tracking Robustness, Accuracy, and Precision in Virtual Reality
A compilation of code relevant to the paper named above.

## Analysis
This section is dedicated to code for analyzing existing eye tracking footage using deep learning networks.

### Required repositories
* Repository 1 (`"pupil"`): https://github.com/PerForm-Lab-RIT/pupil/tree/post-hoc-vr-gazer-analysis
* Repository 2 (`"pupil-core-pipeline"`): https://github.com/PerForm-Lab-RIT/pupil-core-pipeline/tree/main
* Repository 3 (`"pupil-core-plugins"`): https://github.com/PerForm-Lab-RIT/Pupil-Labs-Core-RITnet-Plugins/tree/pipeline-plugins
* Repository 4 (`"interception-uxf-analysis"`): https://github.com/PerForm-Lab-RIT/Interception_UXF_Analysis/tree/new-pipeline

* External repository: (`"ESFnet"`): https://github.com/zhaoyuhsin/Edge-Guided-Near-Eye-Image-Analysis-for-Head-Mounted-Displays
  * Download this into the `pupil-core-plugins` folder and rename it `"ESFnet/"`

### Setup
Clone each of the repositories listed above, renaming the parent directories to the text seen in the parentheses. Note that the final one, `ESFnet`, should be cloned _into_ the `pupil-core-plugins` directory before being renamed.

Obtain the dependencies by installing the conda environment found in <environment yml file name>.
* Not included in this environment file is PyTorch -- this is because you should install a version with CUDA compatibility according to your own hardware. See: https://pytorch.org/get-started/locally/ for instructions on how to do this.

## Data Collection
This section is dedicated to the code used for our experiments' data collection.

### Required repositories
* Repository 1 (`"Interception UXF"`): https://github.com/PerForm-Lab-RIT/Interception_UXF/tree/HMD_Eyes_1.4
* Repository 2 (`"pupil"`): https://github.com/PerForm-Lab-RIT/pupil/tree/post-hoc-vr-gazer-capture
  * Note that this is distinct from the `pupil` used in the Analysis portion.
