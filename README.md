# DeepMG: Learning to Generate Maps from Trajectories

In this study, we aim to generate a routable road network from trajectories using a deep learning approach.

## Paper

If you find our code useful for your research, please cite our paper:

*Sijie Ruan, Cheng Long, Jie Bao, Chunyang Li, Zisheng Yu, Ruiyuan Li, Yuxuan Liang, Tianfu He, Yu Zheng. "Learning to Generate Maps from Trajectories". AAAI 2020.*


## Geometry Translation

### Feature Extraction

`python feature_extraction.py ../data/conf_tdrive_sample.json`

### Training

`python train.py --name t2rnet_tdrive_sample --dataroot ../data/tdrive_sample/learning --lam 0.2 --batch_size 8 --model t2rnet --display_id -1 --gpu_ids -1`

### Inference

`python test.py --name t2rnet_tdrive_sample --dataroot ../data/tdrive_sample/learning --lam 0.2 --model t2rnet --gpu_ids -1`

### Region Concatenation

`python region_concatenation.py --tile_path ./results/t2rnet_tdrive_sample/test_latest/images/ --conf_path ../data/conf_tdrive_sample.json --results_path ../data/tdrive_sample/results/`

## Topology Construction

### Graph Extraction

`python main.py --phase 1 --conf_path ../data/conf_tdrive_sample.json --results_path ../data/tdrive_sample/results/`

### Link Generation

`python main.py --phase 2 --conf_path ../data/conf_tdrive_sample.json --results_path ../data/tdrive_sample/results/`

### Map Refinement

#### Map Matching

`python main.py --phase 3 --conf_path ../data/conf_tdrive_sample.json --results_path ../data/tdrive_sample/results/`

#### Edge Pruning

`python main.py --phase 4 --conf_path ../data/conf_tdrive_sample.json --results_path ../data/tdrive_sample/results/`

## Requirements

DeepMG uses the following dependencies with Python 3.6

* gdal
* opencv
* rtree
* networkx
* scikit-image
* pytorch
* torchvision
* cudatoolkit
* tqdm
* dominate

Other packages can be easily installed using `conda install`, while the following scripts are recommended for `gdal`, `opencv` and `pytorch`.

`conda install -c conda-forge gdal`

`conda install -c menpo opencv`

`conda install pytorch torchvision cudatoolkit -c pytorch -c defaults -c numba/label/dev`

`conda install networkx rtree tqdm`

`conda install dominate`

`conda install scikit-image`

Note that `gdal` must be installed first, and a restart might be required after all installation.

## Datasets

* Trajectory Data
    * There are some open-source trajectory datasets, e.g., [TDrive](https://www.microsoft.com/en-us/research/publication/t-drive-trajectory-data-sample/).
    * Data should be organized as text files. A text file can contain several trajectories (refer to [sample.txt.template](https://github.com/sjruan/DeepMG/blob/master/data/tdrive_sample/traj/sample.txt.template))
    
* Road Network Data
    * [OpenStreetMap (OSM)](https://www.openstreetmap.org/) is an open-source map data source
    * The Shapefile road network format can be generated by [osm2rn](https://github.com/sjruan/osm2rn).
