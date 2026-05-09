# CenterFusion: Center-based Radar and Camera Fusion for 3D Object Detection

**Authors:** Abdelrahaman M. Fathi, Amr T. Zaki, Abdelrahman Y. Kamal, Ahmed M. EL Sherbeny, Mohab M. Hassan  
**Institution:** Egypt-Japan University for Science and Technology (E-JUST)  
**Paper:** arXiv:2011.04841v1

## Overview

CenterFusion proposes a novel radar-camera middle-fusion framework for robust 3D object detection in autonomous driving scenarios. Unlike traditional LiDAR-based approaches, CenterFusion leverages the complementary strengths of radar and camera sensors to enable accurate 3D object detection with enhanced velocity estimation in adverse weather conditions.

## Key Contributions

1. **Frustum-Based Radar Association**: A principled mechanism to accurately associate radar detections with their corresponding image-detected objects, solving the radar-camera data association problem.

2. **Pillar Expansion Method**: Compensates for radar's inaccurate height measurements by converting radar points into fixed-size pillars in 3D space.

3. **Middle Fusion Architecture**: Combines image features with radar-derived feature maps containing depth and velocity information for refined 3D bounding box predictions.

4. **Strong Empirical Results**: Achieves 12.25% relative improvement in NDS score over camera-only baselines and 62% improvement in velocity estimation.

## Problem Statement

While LiDAR and camera fusion has become dominant for 3D object detection, it faces critical limitations:

- **Weather Sensitivity**: Both LiDAR and camera degrade significantly in adverse conditions (fog, rain, snow)
- **Velocity Estimation**: Requires temporal information across multiple frames
- **Radar Underutilization**: Radar is robust, long-range (up to 200m), provides instantaneous velocity via Doppler effect, but remains largely unused

### Radar-Specific Challenges

- Sparse point clouds compared to LiDAR
- Inaccurate or missing vertical (z-axis) coordinates
- Data association problem: multiple radar detections may correspond to one object, false positives, occlusion ambiguity

## Methodology

### System Architecture

CenterFusion operates in two stages:

1. **Primary Detection Stage**
   - Camera image → CenterNet backbone
   - Generates preliminary 3D bounding boxes
   - Estimates center points, depth, dimensions, rotation from image features alone

2. **Radar-Fused Refinement Stage**
   - Preprocess radar point clouds via pillar expansion
   - Associate radar detections using frustum-based mechanism
   - Generate complementary radar feature maps (depth, vx, vy)
   - Concatenate with image features
   - Refine depth, rotation, and generate velocity predictions

### Data Preprocessing

- **Radar**: Aggregate 3 sweeps, ego-motion compensation, represent as 3D points P = (x, y, z, vx, vy)
- **Radar Expansion**: Convert to fixed-size pillars of [0.2, 0.2, 1.5]m
- **Camera**: Resize from 1600×900 to 800×450 pixels, apply random horizontal flipping and shifting during training

## Experimental Results

### Models Evaluated

1. **CenterNet Baseline**: Camera-only detection using DLA backbone
2. **CenterFusion with Pillar Expansion (PE)**: Radar pillars mapped to image plane
3. **CenterFusion (PE + Frustum Association)**: Full proposed pipeline

### Performance Metrics

Evaluated on nuScenes 3D detection benchmark using:
- **NDS** (nuScenes Detection Score)
- **mAP** (mean Average Precision)
- **Error Metrics**: mATE, mASE, mAOE, mAVE, mAAE

### Key Results

| Metric | CenterFusion | CenterNet | Improvement |
|--------|-------------|-----------|------------|
| NDS | 0.449 | 0.400 | **+12.25%** |
| mAVE (velocity) | 0.614 | 1.629 | **-62.3%** |

CenterFusion produces tighter 3D bounding boxes and significantly more accurate velocity predictions, particularly for distant objects.

## Dataset

- **nuScenes**: Autonomous driving dataset with synchronized radar, camera, and LiDAR data
- **Scale**: 1000 driving scenes
- **Availability**: Publicly available at [nuscenes.org](https://www.nuscenes.org/)

## Requirements

### Resources
- GPU: NVIDIA Tesla P100 or equivalent
- Cloud Platform: Kaggle (with free GPU access)

### Software & Libraries
- Python 3.x
- PyTorch
- NumPy
- OpenCV
- Standard computer vision libraries

### Pretrained Models
- CenterNet pre-trained weights available at official GitHub repository

## Usage

### Setup

```bash
# Install dependencies
pip install torch torchvision opencv-python numpy

# Download nuScenes dataset
# Visit https://www.nuscenes.org/ for dataset access
```

### Running the Model

```bash
# Training/evaluation code would be provided in the full implementation
python train.py --config config.yaml
python evaluate.py --checkpoint model.pth
```

## Qualitative Results

The paper demonstrates superior performance through:
- Tighter 3D bounding boxes in both camera and Bird's Eye View (BEV)
- Significantly more accurate velocity predictions for distant objects
- Robust detection in challenging urban driving scenarios

## Conclusion

CenterFusion addresses the underutilization of radar in autonomous driving perception by proposing a principled middle-fusion approach. By effectively combining radar's robust weather resistance and velocity sensing with camera's rich appearance features, CenterFusion achieves state-of-the-art 3D object detection performance without relying on LiDAR or temporal information.

## Future Work

Potential extensions include:
- Temporal integration for enhanced tracking
- Multi-frame radar information fusion
- Extension to other sensor modalities
- Real-time deployment optimization

## References

```bibtex
@article{nabati2020centerfusion,
  title={CenterFusion: Center-based Radar and Camera Fusion for 3D Object Detection},
  author={Nabati, R. and Qi, H.},
  journal={arXiv preprint arXiv:2011.04841},
  year={2020}
}
```

Nabati, R., & Qi, H. (2020). CenterFusion: Center-based radar and camera fusion for 3D object detection. arXiv preprint arXiv:2011.04841. https://doi.org/10.48550/arxiv.2011.04841

## Paper Citation

For the full paper and implementation details, please refer to:
- **arXiv**: https://arxiv.org/abs/2011.04841
- **DOI**: 10.48550/arxiv.2011.04841
