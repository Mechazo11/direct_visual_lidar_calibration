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

* Analyzing both rosbag2 files, it appears the following topics and the naming are at least required

```bash
Topic information: Topic: /points | Type: sensor_msgs/msg/PointCloud2 | Count: 265 | Serialization Format: cdr
                   Topic: /image | Type: sensor_msgs/msg/Image | Count: 53 | Serialization Format: cdr
                   Topic: /camera_info | Type: sensor_msgs/msg/CameraInfo | Count: 53 | Serialization Format: cdr
```

* `preprocess_ros2.cpp` can automatically handle `CompressedImage` messages

## Running the `ouster` calibration example

* Assuming this package was created in the `~/ros2_ws/` 

* Create in `home` a directory called `CAMERA_LIDAR_CALIB_BAGS`

* Extract and put the rosbags related to `ouster` lidar in the above directory. All the rosbags should be in `.db3` format

* Step 0: Change directory into `CAMERA_LIDAR_CALIB_BAGS` directory

```bash
cd ~/CAMERA_LIDAR_CALIB_BAGS/
```

* Step 1: Create desnse lidar clouds and create static lidar scan - image pairing with intensities. Output is a static directory called `ouster_preprocessed`

```bash
# -a : Detect points/image/camera_info topics automatically
# -d : Use dynamic points integrator
# -v : Enable visualization
ros2 run direct_visual_lidar_calibration preprocess ouster ouster_preprocessed -adv
```

* Step 2: use superglue to run automatic pairing and get initial T_camera_lidar (lidar to camera) frame transform

```bash
ros2 run direct_visual_lidar_calibration find_matches_superglue.py ouster_preprocessed
ros2 run direct_visual_lidar_calibration initial_guess_auto ouster_preprocessed
```

* Step 3: Perform fine registration to fine tune the transform. Visualization already conforms which lidar cloud corresponds to the image seen

```bash
ros2 run direct_visual_lidar_calibration calibrate ouster_preprocessed
```

## Acknowledgement

Koide et al., General, Single-shot, Target-less, and Automatic LiDAR-Camera Extrinsic Calibration Toolbox, ICRA2023, [[PDF]](https://staff.aist.go.jp/k.koide/assets/pdf/icra2023.pdf)
