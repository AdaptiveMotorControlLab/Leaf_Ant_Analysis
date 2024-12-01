# 🍃 Leaf 🐜 Ant Analysis
- Supporting code and models for [Gilbert, Glastad et al.](https://www.biorxiv.org/content/biorxiv/early/2024/11/08/2024.11.07.622473.full.pdf) 

- DeepLabCut tracking and analysis performed by [Mackenzie W. Mathis](https://github.com/mmathislab). Leaf segmentation by [Anastasiia Filippova](https://github.com/nastya236).



## Table of contents:

- Colab notebook that highlights the training, evaluation, and video analysis: `Sharing_Ants_Mackenzie_2024_Colab_Training_VideoAnalysis.ipynb`
- Colab notebook with custom helper code for producing the paper analysis and plots: `Sharing_ant_analysis.ipynb`
  -  utils file to support ant DLC analysis: `utils_ant_analysis.py`
 
- `\data` dir with the raw analysis results file for DLC tracking and segmentation.
- Colab notebook with statistical tests and plots: `Ant_stats.ipynb`
- Colab notebook with segmentation plots: `plot_results_segmentation.ipynb`

## Methods Overview:

**CCAP Chamber analysis with DeepLabCut (DLC):**

<p>
  <img src="https://github.com/user-attachments/assets/8c1ca4f2-1a6b-44b4-8529-f96bf0beac52" alt="ant_heatmap" align="right" width="350" style="margin-right: 15px;">
In this assay we used DeepLabCut version 2.3.9 [Mathis et al, 2018, Nath et al, 2019, Lauer et al. 2022]. Specifically, we labeled 100 frames taken from 5 videos (99% was used for network training). The total number of video frames requiring analysis were 322,124 thus we labeled 0.03% of the total dataset we aimed to analyze. We used the DLCRNet_ms5  neural network with mostly default parameters; for training the `global_scale` was changed to 0.9, `sharpen` was set to true, and `pos_dist_thresh` was set to 11. The model was trained with batch size 8 for 100K iterations. The train error was 2.93 pixels, and the test error was 4.65 pixels (but note the low fraction used for held-out testing). This network was then used to run video inference and tracking (using `auto_track=true` parameters). We then developed custom python code to correct any any ID swapped programmatically, smoothed the keypoint data with a savgol_filter from scipy (cite dlc2kinamtics and scipy), and computed the time the ant spent with the open chamber ROI (defined by DLC keypoints that delineated the bounds of the full chamber vs. the leaf ROI).  Note, when the ant is within the leaf chamber, it is often occluded under leaves, therefore  we cannot always faithfully track it. Thus, we computed the time outside the leaf ROI, given the ant then is nearly always visible (and by default the ant then must be in the leaf ROI). To make the density visualization we used the extracted DLC ant keypoints and applied a `gaussian_filter` (sigma=2) to plot the density of the ant within the open chamber ROI.
<p>





**Leaf Density Analysis:**
<p>
  <img src="https://github.com/user-attachments/assets/21b38648-5fdd-4f9b-b532-da1459e6c1c7" alt="leaf_seg" align="right" width="300" style="margin-right: 15px;">

To quantify how the scram vs. CCAP treated ants moved leaves from the leaf ROI into the open area, we segmented leaves using a custom napari plugin for labeling (https://github.com/AdaptiveMotorControlLab/SegementationLabeler). We cropped the images around the chambers, resulting in a segmentation image size of (710, 1100). Subsequently, we labeled one frame per minute (every 60th frame) for each video. As a result, we obtained 118 binary segmentation masks for video 1, 122 for video 2, 123 for video 3, 123 for video 4, and 61 for video 5, totaling 547 masks. The binary masks conform to the size of the cropped images, with 1 on the mask indicating a "leaf" pixel and 0 representing anything else (such as an ant or background pixel).
<p>
	
-  Napari segmentation labeler: https://github.com/AdaptiveMotorControlLab/SegementationLabeler

## Citation:

Preprint:
```
@article {Gilbert2024.11.07.622473,
	author = {Gilbert, Michael B. and Glastad, Karl M. and Fioriti, Maxxum and Sorek, Matan and Gannon, Tierney and Xu, Daniel and Pino, Lindsay K. and Korotkov, Anatoly and Biashad, Ali and Baeza, Josue and Lauman, Richard and Filippova, Anastasiia and Kacsoh, Balint Z. and Bonasio, Roberto and Mathis, Mackenzie W. and Garcia, Benjamin A. and Seluanov, Andrei and Gorbunova, Vera and Berger, Shelley L.},
	title = {Neuropeptides specify and reprogram division of labor in the leafcutter ant Atta cephalotes},
	elocation-id = {2024.11.07.622473},
	year = {2024},
	doi = {10.1101/2024.11.07.622473},
	publisher = {Cold Spring Harbor Laboratory},
	URL = {https://www.biorxiv.org/content/early/2024/11/08/2024.11.07.622473},
	journal = {bioRxiv}
}
```
