# MV-GLMB-AB (Multiview GLMB-Adaptive Birth)

This is the C++ implementation for the paper:
```
@artcile{linh2024inffus,
      title={Track Initialization and Re-Identification for {3D} Multi-View Multi-Object Tracking}, 
      author={Linh Van Ma, Tran Thien Dat Nguyen, Ba-Ngu Vo, Hyunsung Jang, Moongu Jeon},
      journal={Information Fusion},
      year={2024}
}
```
A pre-print version of the article is available at http://arxiv.org/abs/2405.18606.

### Quick Overview
The algorithm estimates 3D tracks from 2D bounding box detection.

![](assets/overview.png)

### System Installation (require Docker)
Download the Docker image at [Docker Hub](https://hub.docker.com/r/isplcurtin/mv-glmb-ab).
- Clone this repository:
    ```sh
    git clone https://github.com/TranThienDat-Nguyen/3D-VisualTracking.git
    # or alternatively: gh repo clone TranThienDat-Nguyen/3D-VisualTracking
    ```
- Follow instructions [here](https://docs.docker.com/engine/install/) to install docker on your host computer.
- Run this repo via `docker` command from the `main source code` directory: 
    ```bash
    cd 3D-VisualTracking
    docker run -d -t --rm --name 3D-VisualTracking -p 8888:8888   -v $(pwd):/workspace:Z  isplcurtin/mv-glmb-ab:latest
    docker exec -it 3D-VisualTracking bash
    ```
The docker also supports running codes via [browser](http://localhost:8888) with login password `abc123`. 

### Preparing Data
 - Download the datasets: 
    - CMC datasets (CMC1, CMC2, CMC3, CMC4, CMC5) can be download from [MEGA CLOUD, NZ](https://mega.nz/file/LKxAyZiT#wa-aMQmgk9guNkjj1olaPeUf-LgPS5P9iYBmZSLFnp8).
    - WILDTRACK dataset is available  [EPFL CVLAB](https://www.epfl.ch/labs/cvlab/data/data-wildtrack/).
- Place the downloaded data into the following folder structure
    ```
    |-- source code
    |   |-- data
    |   |   |-- images
    |   |   |   |-- CMC1
    |   |   |   |   |-- Cam_1
    |   |   |   |   |-- Cam_2
    |   |   |   |   |-- Cam_3
    |   |   |   |   |-- Cam_4
    |   |   |   |-- ...
    |   |   |   |-- CMC5
    |   |   |   |-- WILDTRACK
    |   |-- cpp_ms_glmb_ukf
    |   |-- detection
    |   |   |-- cstrack
    |   |   |   |-- CMC1
    |   |   |   |   |-- Cam_1.npz
    |   |   |   |   |-- Cam_2.npz
    |   |   |   |   |-- Cam_3.npz
    |   |   |   |   |-- Cam_4.npz
    |   |   |   |-- CMC2
    |   |   |-- fairmot
    |   |-- experiments
    |   |-- ms_glmb_ukf
    |   |-- README.md
    ```
- Update the folders containing detection files, for example, `../detection/fairmot/` in `gen_meas.py`.
    - FairMOT 2D image detector (output bounding boxes and re-identification feature) https://github.com/ifzhang/FairMOT
    - CSTrack 2D image detector (output bounding boxes and re-identification feature) [https://github.com/JudasDie/SOTS](https://github.com/JudasDie/SOTS/blob/MOT/CSTrack/lib/tutorial/CSTrack/cstrack.md)
    - We can use pre-trained weights from FairMOT and CSTrack but to improve the accuracy of our 3D tracking algorithm on CMC and WILDTRACK datasets, 2D image detectors need to be re-train on the CMC4 dataset (see [gen_labels_cmc.py](detection/fairmot/cmc/gen_labels_cmc.py)).
- Update image files for visualization `../data/images/` in `gen_meas.py`.
- Prepare ground truth data `gt_data_dir="../data/images/"` for performance evaluation using `CLEAR MOT` in `clearmot.py` and `OSPA2` in `ospa2.py`.

### Compiling Default Tracking Algorithm (MV-GLMB-AB)
- Navigate to `cpp_ms_glmb_ukf` folder.
- Run `python setup.py build develop`.

### Running Default Tracking Algorithm (MV-GLMB-AB)
- Navigate to 'ms-glmb-ukf'.
- Modify parameters in the demo.py  file.
- Run 'python demo.py'.

### Sample Outputs
![Video Demo for CMC4 dataset](assets/cmc4_demo.gif)

### Writing Your Own Tracking Algorithm
- To be updated.

### Acknowledgement
The source codes are published by Linh Ma (linh.mavan@gm.gist.ac.kr), Machine Learning & Vision Laboratory, GIST, South Korea.

The OSPA(2) metric used for evaluation is described in the paper:
```
@article{rezatofighi2020trustworthy,
  title={How trustworthy are the existing performance evaluations for basic vision tasks?},
  author={Tran Thien Dat Nguyen and Hamid Rezatofighi and Ba-Ngu Vo and Ba-Tuong Vo and Silvio Savarese and Ian Reid},
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence},
  year={2022}
}
```

