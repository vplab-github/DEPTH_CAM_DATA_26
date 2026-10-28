# Dataset for Evaluating the Accuracy of affordable DepthCameras

**Are Depth Cameras Super-Accurate for Visual-Inspection Based Precision Engineering Tasks?**

ICVGIP submission, © 2026.

## Note
**We have proposed the only RGBD dataset having images acquired by affordable RGBD cameras, with corresponding depth GTs at salient points of the scene.
 No LIDAR or laser or structured light source used.**

## Dataset Availability

The dataset is available at:
[Affordable RGB-D Camera Accuracy Dataset Drive](https://drive.google.com/drive/folders/1CgALbJofV7ZUsE0ZCZOtUpjyhqKXdP9A?usp=sharing) 
## About the Data

The dataset consists of synchronized RGB and depth images captured using multiple commercial depth cameras under controlled real-world conditions. It was collected to systematically evaluate the metric accuracy of commercial depth cameras and state-of-the-art MMDE (Metric Monocular Depth Estimation) models, and is made publicly available to facilitate further research on depth estimation, sensor evaluation, and metric depth analysis.

**What it can be used for:**

- Benchmarking raw hardware accuracy of depth cameras against verified ground truth
- Evaluating cross-sensor consistency of Metric Monocular Depth Estimation models
- Developing / testing per-sensor calibration methods (e.g., DepthCal)
- Precision applications: robotic grasping, close-range navigation, clash detection

## Cameras

| Camera | Manufacturer | Modality |
|---|---|---|
| Femto Mega | Orbbec | Time-of-Flight (ToF) |
| Gemini 336 | Orbbec | Active stereo IR |
| Intel RealSense D435f | Intel | Active stereo IR |

For each camera, the full dataset includes:

- RGB images (`.png`)
- 16-bit ground-truth depth maps (`.png`)
- Hand-measured ground-truth distance of the imaged objects (per-case lookup table)

## Dataset Samples

This repository includes a representative sample of the dataset under [`RGB_D samples/`](RGB_D%20samples/), organized per camera. 

```
RGB_D samples/
├── Femto Mega/
│   ├── RGB/                        # RGB captures
│   ├── Depthmaps/                  # 16-bit depth maps
│   └── Lookup Table Femto_cases.xlsx
├── Gemini 336/
│   ├── RGB/
│   ├── Depthmaps/
│   └── Lookup Table Gemini_cases.xlsx
└── Intel Real Sense/
    ├── RGB/
    ├── Depthmaps/
    └── Lookup Table RealSense_cases.xlsx
```

Each camera folder's `Lookup Table *_cases.xlsx` maps every captured case (e.g., "Samsung box @ 0.5 Mtr.", "6 objects + whiteboard") to its hand-measured ground-truth distance and original capture path.

### Example captures

| Camera | RGB | Depth Map |
|---|---|---|
| Femto Mega | ![Femto RGB](RGB_D%20samples/Femto%20Mega/RGB/femto_6obj_rgb_170626.png) | ![Femto Depth](RGB_D%20samples/Femto%20Mega/Depthmaps/femto_6obj_depthmap_170626.png) |
| Gemini 336 | ![Gemini RGB](RGB_D%20samples/Gemini%20336/RGB/Gemini_6objs_rgb.png) | ![Gemini Depth](RGB_D%20samples/Gemini%20336/Depthmaps/Gemini_6objs_depthmap.png) |
| Intel RealSense | ![Intel RGB](RGB_D%20samples/Intel%20Real%20Sense/RGB/Intel_samsungbox_50cm_rgb.png) | ![Intel Depth](RGB_D%20samples/Intel%20Real%20Sense/Depthmaps/Intel_samsungbox_50cm_depthmap.png) |

## How the Data Was Used

Two key findings motivate this dataset:

- **MMDE comparison** — the same RGB frame of an object at 1 m, run through five state-of-the-art metric depth estimation models, yields divergent raw metric depth predictions.
- **Camera comparison** — the same object at 1 m, captured directly by the three depth cameras (no MMDE involved), already shows differing noise patterns and value ranges across Femto, Gemini, and RealSense — the raw hardware-level Cross-Sensor Bias.
  
## A BRIEF OUTLINE ON ACCURATE BENCHMARK RGBD DATASETS, COMPARED WITH OUR COMPILED INDOOR DATASET

**Our indoor depth dataset:** RGBD data with corresponding GT values (accurate upto a mm) at salient points in the scene. Regular objects in scene having linear and curvilinear surfaces. Three affordable of-the-shelf RGBD cameras used to acquire samples. GT obtained by physical measurements using Tape/Scale and a Digital Caliper Measuring tool (sub-mm accurate).

# Millimetre-accurate indoor RGB-D datasets

Recent indoor datasets with such high mm-level precision/accuracy have their **scene geometry or depth maps** acquired using  expensive  survey-grade laser / metrology scanners (±1 mm ranging class).


## What counts as millimetre GT

Commodity RGB-D (Kinect, Structure Sensor, iPhone/iPad LiDAR, RealSense, Matterport) **is centimetre-class** on scene geometry. Millimetre indoor GT almost always means a FARO/Leica/Riegl terrestrial laser scanner, or a handheld metrology scanner (Artec Leo, EinScan), registered to RGB or RGB-D frames. These scanners generally require high processing time to convert and register depth to RBGD frames.

iPhone/iPad streams in ScanNet++ and ARKitScenes are the training/query modality; the laser mesh is the ground truth.

**Explanation of the term "#category" used in table for comparison below:**

- **Scene TLS:** millimetre scene/object geometry from a terrestrial laser scanner (TLS)- laser mesh is often the GT.
- **Image TLS:** image-level TLS depth (no full-room mesh video)
- **Handheld 3D:** handheld metrology scanner (sub-0.2 mm); Mostly require manual operation and processing time.
- **Point GT:** sparse, mm-accurate GT at discrete salient points (not full mesh/dense depth), using tape/scale/digital-caliper.

SceneFun3D reuses ARKitScenes laser rooms (overlap, not additive).

---



### The table below compares key points across benchmark datasets.


| Dataset                                                                             | Year           | #Category                                    | GT accuracy                                    | Sensor Cost                       | Depth range                       |
| ----------------------------------------------------------------------------------- | -------------- | -------------------------------------------- | ---------------------------------------------- | --------------------------------- | --------------------------------- |
| [ScanNet++ v2](https://scannetpp.mlsg.cit.tum.de/scannetpp/)                        | 2023 / v2 2024 | Scene TLS                                    | ±1 mm ranging; ~0.9 mm spacing; 2 mm @ 10 Mtr. 3D | FARO ~$34–44k unit / ~$60–80k kit | TLS 0.5–70/150 Mtr.; iPhone ~0.5–5 Mtr. |
| [ARKitScenes](https://github.com/apple/ARKitScenes/)                                | 2021           | Scene TLS                                    | ±1 mm ranging; ~2 mm @ 10 Mtr. 3D                 | FARO S70 ~$35–40k; iPad ~$0.8–1k  | TLS 0.6–70 Mtr.; iPad ~0.5–5 Mtr.       |
| [SceneFun3D](https://scenefun3d.github.io/)                                         | 2024           | Scene TLS                                    | TLS ±1 mm raw; 5 mm published                  | Same as ARKitScenes               | Same as ARKitScenes               |
| [DIODE Indoor](https://diode-dataset.org/)                                          | 2019           | Image TLS                                    | ±1 mm; 0.009° angular                          | FARO S350 ~$30k+                  | 0.6–350 Mtr.                         |
| [ETH3D](https://www.eth3d.net/)                                                     | 2017           | Scene TLS                                    | ~±2 mm ranging; poses ~1 mm                    | ~$25–50k educational era          | 0.6–330 Mtr.                         |
| [iBims-1](https://www.asg.ed.tum.de/en/lmf/ibims1/)                                 | 2018           | Image TLS                                    | Millimetre-class TLS                           | DSLR + TLS / ??                   | 0.1–50 Mtr.                          |
| [SCRREAM](https://sites.google.com/view/scrream/about)                              | 2024           | Handheld 3D                                  | 0.1 mm / 0.2 mm (Leo)                          | Leo ~$25–35k; EinScan ~$5–10k     | Leo 0.35–1.2 Mtr. per pass           |
| [Our dataset](https://anonymous.4open.science/r/DEPTH_CAM_DATA_26_ICVGIP/README.md) | 2026           | Point GT; Geometrical objects in close range | Accurate up to 1 mm at salient points          | ~$ **0.4 – 0.8** k                | 0–5 Mtr.                             |


---



## Not millimetre-accurate (often confused with SoA indoor RGB-D)

These are important indoor RGB-D resources but their geometry GT is centimetre-class commodity depth or reconstruction, not survey TLS:


| Dataset                         | Sensor                                   | Why it is not mm GT                                          |
| ------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| ScanNet / ScanNet200            | iPad + Occipital Structure Sensor        | Commodity depth, ~cm reconstruction                          |
| Matterport3D, HM3D, Gibson      | Matterport camera / panorama RGB-D       | Building-scale but cm-level meshes                           |
| NYU Depth v2, SUN RGB-D         | Kinect v1/v2                             | Standard depth-estimation train sets; ~cm level              |
| Replica                         | Custom structured-light RGB-D rig + TSDF | High-fidelity mesh (~6k prim/m²) but not laser millimetre GT |
| MultiScan, 3RScan               | Commodity mobile RGB-D                   | Articulation labels, cm geometry                             |
| Realsee3D (real split)          | Galois-P4 LiDAR panorama                 | Public specs ~10 mm error @ 0.1–15 Mtr. — cm-class, not mm      |
| 7-Scenes, BundleFusion, SceneNN | Kinect + KinectFusion-style recon        | No independent millimetre laser GT                           |


---



## How to choose

- **Large-scale learning + mm eval:** ScanNet++ v2 (geometry + semantics + DSLR + iPhone) or ARKitScenes / SceneFun3D (mobile RGB-D + Faro depth).
- **Monocular depth with laser maps:** DIODE Indoor and iBims-1.
- **Watertight millimetre meshes / NVS+SLAM with complete GT depth:** SCRREAM (11 scenes).
- **RGB-D SLAM trajectories:** TUM RGB-D or Bonn (mm poses only; do not treat Kinect depth as mm scene GT).
- **Our dataset:** RGBD data with corresponding GT values (accurate upto a mm) at salient points in the scene. Regular objects in scene with linear and curvilinear surfaces. Three affordable of-the-shelf RGBD cameras used to acquire samples. GT obtained by physical measurements using Tape/Scale and Digital Caliper Measuring tool (sub-mm accurate).

---

We have compiled this information (August 2026) from papers and vendor specs. Dataset downloads themselves are free for research; Cost pricing figures are capture-hardware list prices, not a fee for downloading the data.

## Abstract

Benchmark RGB-D datasets are foundational to a multitude of Computer Vision (CV) tasks, including object recognition, semantic segmentation, and 3D structure estimation. While these tasks often rely on the global structure of depth maps and are resilient to minor inaccuracies, specific applications requiring extremely strict millimeter-scale fidelity, such as precise robotic grasping of small objects, sub-degree level of spatial pose estimation, close-range navigation, collision detection, micro-assembly and alignment, etc. demand super-accurate structural constraints. This paper presents a rigorous empirical evaluation of the real-world macroscopic accuracies of three depth sensing cameras (Time-of-Flight (Orbbec Femto Mega), active stereo (Orbbec Gemini 336), and active-IR stereo (Intel RealSense D435f)), all of which are affordable for practical use for short to medium range of object distances. Crucially, we also expose a significant vulnerability in state-of-the-art DL-based Metric Monocular Depth Estimation (MMDE) models, demonstrating how these algorithms surprisingly predict different metric depths when applied to RGB inputs. Using physically constrained ground truth (GT) data, we quantify hardware-level versus algorithmic depth estimation errors. Finally, we propose DepthCal, an add-on per-sensor piecewise-linear calibration (a closed-form lookup, not a scale-shift) to reduce distance-dependent bias (see supplementary Section 12 for regression-based calibration). Claims are scoped for three sensors, indoors, at 0.36-3.25m.

## Sources

[1] Chandan Yeshwanth, Yueh-Cheng Liu, Matthias Nießner, and Angela Dai. 2023. ScanNet++: A High-Fidelity Dataset of 3D Indoor Scenes. In *Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)*. 12–22.
 
[2] Gilad Baruch, Zhuoyuan Chen, Afshin Dehghan, Tal Dimry, Yuri Feigin, Peter Fu, Thomas Gebauer, Brandon Joffe, Daniel Kurz, Arik Schwartz, and Elad Shulman. 2021. ARKitScenes: A Diverse Real-World Dataset for 3D Indoor Scene Understanding Using Mobile RGB-D Data. In *Proceedings of the Neural Information Processing Systems Track on Datasets and Benchmarks (NeurIPS)*.
 
[3] Alexandros Delitzas, Ayça Takmaz, Federico Tombari, Robert Sumner, Marc Pollefeys, and Francis Engelmann. 2024. SceneFun3D: Fine-Grained Functionality and Affordance Understanding in 3D Scenes. In *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)*. 14531–14542.
 
[4] Igor Vasiljevic, Nick Kolkin, Shanyi Zhang, Ruotian Luo, Haochen Wang, Falcon Z. Dai, Andrea F. Daniele, Mohammadreza Mostajabi, Steven Basart, Matthew R. Walter, and Gregory Shakhnarovich. 2019. DIODE: A Dense Indoor and Outdoor DEpth Dataset. arXiv preprint arXiv:1908.00463 (Aug. 2019).
 
[5] Thomas Schöps, Johannes L. Schönberger, Silvano Galliani, Torsten Sattler, Konrad Schindler, Marc Pollefeys, and Andreas Geiger. 2017. A Multi-View Stereo Benchmark with High-Resolution Images and Multi-Camera Videos. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)*. 3260–3269.
 
[6] Tobias Koch, Lukas Liebel, Friedrich Fraundorfer, and Marco Körner. 2018. Evaluation of CNN-Based Single-Image Depth Estimation Methods. In *Proceedings of the European Conference on Computer Vision (ECCV) Workshops*. 331–348.
 
[7] Tobias Koch, Lukas Liebel, Marco Körner, and Friedrich Fraundorfer. 2020. Comparison of Monocular Depth Estimation Methods Using Geometrically Relevant Metrics on the IBims-1 Dataset. *Computer Vision and Image Understanding* 191 (2020), 102877.
 
[8] HyunJun Jung, Weihang Li, Shun-Cheng Wu, William Bittner, Nikolas Brasch, Jifei Song, Eduardo Perez-Pellitero, Zhensong Zhang, Arthur Moreau, Nassir Navab, and Benjamin Busam. 2024. SCRREAM: SCan, Register, REnder And Map: A Framework for Annotating Accurate and Dense 3D Indoor Scenes with a Benchmark. In *Proceedings of the Neural Information Processing Systems Track on Datasets and Benchmarks (NeurIPS)*.
 
[9] Jürgen Sturm, Nikolas Engelhard, Felix Endres, Wolfram Burgard, and Daniel Cremers. 2012. A Benchmark for the Evaluation of RGB-D SLAM Systems. In *Proceedings of the IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)*. 573–580.
 
[10] Emanuele Palazzolo, Jens Behley, Philipp Lottes, Philippe Giguère, and Cyrill Stachniss. 2019. ReFusion: 3D Reconstruction in Dynamic Environments for RGB-D Cameras Exploiting Residuals. In *Proceedings of the IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)*. 7855–7862.
 
[11] FARO Technologies, Inc. FARO Focus Laser Scanner — Technical Specification Sheet (Premium / S70 / S350 series). Available at: https://knowledge.faro.com/Hardware/Focus/Focus/Technical_Specification_Sheet_for_the_Focus_Laser_Scanner and https://www.faro.com/en/Resource-Library/Tech-Sheet/techsheet-faro-focus-laser-scanners
 
[12] Artec 3D. Artec Leo — Product Page and Specifications. Available at: https://www.artec3d.com/portable-3d-scanners/artec-leo (product brochure PDF: https://www.artec3d.com/files/pdf/Artec3D-Leo.pdf)
 
[13] Leica Geosystems AG. Leica BLK360 — Product Specifications Sheet. Available at: https://shop.leica-geosystems.com/sites/default/files/2022-06/Leica_BLK360-SpecSheet.pdf (support/downloads page: https://shop.leica-geosystems.com/leica-blk/support/blk360-support)
 
Hardware price figures are drawn from public municipal/education procurement quotes (2024–2026) and are not official vendor list prices.

## Citation

A formal citation will be added once the paper is published. 

## Correspondence: sdas@cse.iitm.ac.in
