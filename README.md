# direct_visual_lidar_calibration

A targetless, highly configurable, single-shot and automatic Lidar-Camera extrinisc calibration toolbox by Dr. Kenji Koide. 

This package provides a toolbox for LiDAR-camera calibration that is: 


## Getting started

Follow the installation, data collection and calibration examples from the excellent documents hosted by the original repo: **[https://koide3.github.io/direct_visual_lidar_calibration/](https://koide3.github.io/direct_visual_lidar_calibration/)**  


* When cloning this repo, use this command

```bash
cd ~/ros2_ws/src
git clone -b az_dev --single-branch https://github.com/Mechazo11/direct_visual_lidar_calibration.git --recursive
```


* When installing `ceres-solver`, use this cmake command

```bash
cmake .. -DBUILD_EXAMPLES=OFF -DBUILD_TESTING=OFF -DUSE_CUDA=OFF -DBUILD_BENCHMARKS=OFF
```

* Don't echo `PYTHONPATH` command into bashrc, instead copy paste the following line into ros2_config.sh

```bash
export PYTHONPATH=$PYTHONPATH:~/Downloads/SuperGluePretrainedNetwork
```

## Some notes on the rosbags required by this toolbox




## Acknowledgement

Koide et al., General, Single-shot, Target-less, and Automatic LiDAR-Camera Extrinsic Calibration Toolbox, ICRA2023, [[PDF]](https://staff.aist.go.jp/k.koide/assets/pdf/icra2023.pdf)
