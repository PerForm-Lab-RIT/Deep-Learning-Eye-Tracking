# [Using Deep Learning to Increase Eye-Tracking Robustness, Accuracy, and Precision in Virtual Reality](https://arxiv.org/abs/2403.19768)
A compilation of relevant code.

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

Obtain the dependencies by installing the conda environment found in this repository's `environment.yml`.
* Not included in this environment file is PyTorch -- this is because you should install a version with CUDA compatibility according to your own hardware. See: https://pytorch.org/get-started/locally/ for instructions on how to do this.

In `interception-uxf-analysis/.env`, make the following changes:
* Change the path of `CORE_SHARED_MODULES_LOCATION=` to your own `pupil/pupil_src/shared_modules` directory.
* Change the path of `PIPELINE_LOC=` to your own `pupil-core-pipeline/src/` directory.

In `interception-uxf-analysis/PLUGINS.csv`, make the following changes:
* For plugins you would like to use, remove the `#` at the beginning of the line. For plugins you would not like to use, ensure that there is a `#` at the beginning of the line.
* For plugins you have decided to use, change the path at the beginning of the line to your own `pupil-core-plugins` directory.

### Use (Apply Deep Learning Plugin to Pupil Labs Core Data)
This section details the use of this software to improve existing Pupil Labs Core data with a deep learning plugin. This is what you probably want to do -- the other use of this software, the analysis we did described in our work, assumes the specific data format outputted by the Interception UXF data collection process seen in the `Data Collection` section below.

In the `interception-uxf-analysis` folder, create a new directory called `Data/`. Place your Pupil Labs Core data directories into here. Each data folder should be named according to the following convention: `_Pipeline XXX_YYY_Z`, where `XXX` is replaced with a 3 character identifier for the participant, `YYY` is replaced with either `192` (if the eye data is 192x192px) or `400` (if the eye data is 400x400px), and `Z` is replaced with a single-digit identifier in case the same participant-resolution combination was done multiple times. For example, a data folder entitled `_Pipeline u01_192_1` would imply the first data collected from participant "u01" with 192x192px eye videos.

The main file you should run is `pupilCorePipeline.py` within the `interception-uxf-analysis` directory. Make sure you enable the conda environment you created in the `Setup` subsection above. The relevant flags/launch parameters are described below:
* `--not_uxf`: Unless you are trying to replicate our experiment with the `Interception UXF` data collection environment, this flag should _**always**_ be included.
* `--vanilla_only`: This flag ignores the plugins and performs the pupil detection using the native Pupil Labs Core method. This option can be used to test your setup without running any deep learning models.
* `--skip_vanilla`: This flag causes the software to run only your selected plugins, and not the native Pupil Labs Core pupil detection. If this option is not enabled, it will always run the native approach alongside your selected plugins, which may increase processing time.
* `--plugins_file`: This option allows you to specify an alternative to the default `PLUGINS.csv`, in case you would like to make multiple plugin configurations.

The software will ask you to select a gaze mapper (`2D`, `3D`, `Post-Hoc HMD 3D`). In the paper associated with this pipeline, we compared `2D` (a feature-based gaze estimator) to `Post-Hoc HMD 3D` (a 3D model-based gaze estimator). If calibration points are supplied prior to running this pipeline (such as by using the `Interception UXF` data collection pipeline described in the `Data Collection` section below, or by entering the Pupil Labs Core GUI and using it to designate calibration points in the world video), then this pipeline will perform gaze estimation automatically using the method selected here. If no calibration points are supplied, then only pupil detection will be performed. Please select the gaze estimation method you would like to use with the data. **If 2D is selected here, then the resulting pupils may only be used for 2D/feature-based gaze mapping. This is because selecting "2D" causes the pipeline to skip a step critical to 3D model-based gaze mapping in order to save processing time.**

The pupil position information will be saved to `offline_data/<plugin name>/` within each Pupil Labs Core data directory. The three `offline_pupil` files generated within this folder (`offline_pupil.meta`, `offline_pupil.pldata`, and `offline_pupil_timestamps.npy`) can be moved to the parent `offline_data` directory to allow the Pupil Labs Core software to load the deep learning-generated pupil locations. Once this is done, the standard Pupil Labs Core GUI can be used to generate gaze data as normal.

If calibration point information has been supplied, gaze estimation will occur in addition to the pupil detection. Gaze data will be estimated and then exported within each Pupil Labs Core data directory for each plugin that is selected for use:
* `Exports/<plugin name>/gaze_positions.csv` -- An easily parsed .csv file containing normalized gaze coordinates.
* `pipeline-gaze-mappings/<plugin name>/pipeline.pldata` and `.../pipeline_timestamps.npy` -- Pupil Labs Core-compatible gaze data files. These can be dragged into `offline_data/gaze_mappings/` and renamed to replace two existing gaze mappers in order to view the gaze data in the Pupil Labs Core GUI.

## Data Collection
This section is dedicated to the code used for our experiments' data collection.

### Required repositories
* Repository 1 (`"Interception UXF"`): https://github.com/PerForm-Lab-RIT/Interception_UXF/tree/HMD_Eyes_1.4
* Repository 2 (`"pupil"`): https://github.com/PerForm-Lab-RIT/pupil/tree/post-hoc-vr-gazer-capture
  * Note that this is distinct from the `pupil` used in the Analysis portion.

## Citation

- BibTeX:
```
@article{10.1145/3654705,
author = {Barkevich, Kevin and Bailey, Reynold and Diaz, Gabriel J.},
title = {Using Deep Learning to Increase Eye-Tracking Robustness, Accuracy, and Precision in Virtual Reality},
year = {2024},
issue_date = {May 2024},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
volume = {7},
number = {2},
url = {https://doi.org/10.1145/3654705},
doi = {10.1145/3654705},
abstract = {Algorithms for the estimation of gaze direction from mobile and video-based eye trackers typically involve tracking a feature of the eye that moves through the eye camera image in a way that covaries with the shifting gaze direction, such as the center or boundaries of the pupil. Tracking these features using traditional computer vision techniques can be difficult due to partial occlusion and environmental reflections. Although recent efforts to use machine learning (ML) for pupil tracking have demonstrated superior results when evaluated using standard measures of segmentation performance, little is known of how these networks may affect the quality of the final gaze estimate. This work provides an objective assessment of the impact of several contemporary ML-based methods for eye feature tracking when the subsequent gaze estimate is produced using either feature-based or model-based methods. Metrics include the accuracy and precision of the gaze estimate, as well as drop-out rate.},
journal = {Proc. ACM Comput. Graph. Interact. Tech.},
month = {may},
articleno = {27},
numpages = {16},
keywords = {eye tracking, gaze estimation, neural networks, virtual reality}
}
```

- ACM Reference Format:
```
Kevin Barkevich, Reynold Bailey, and Gabriel J. Diaz. 2024. Using Deep Learning to Increase Eye-Tracking Robustness, Accuracy, and Precision in Virtual Reality. Proc. ACM Comput. Graph. Interact. Tech. 7, 2, Article 27 (May 2024), 16 pages. https://doi.org/10.1145/3654705
```
