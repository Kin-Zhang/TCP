# TCP - Trajectory-guided Control Prediction for End-to-end Autonomous Driving: A Simple yet Strong Baseline

Check origin repo from fork, here is for kin comparison

![teaser](assets/teaser_.png)

## Docker and Dataset

### build docker

```
git clone https://github.com/Kin-Zhang/TCP.git
cd TCP
docker build -t zhangkin/tcp .
```

### download dataset
Download our dataset through [GoogleDrive](https://drive.google.com/file/d/1A6k0KpVHs5eSaqunzbRQR-p0b-915O9R/view?usp=sharing)
transfuser to server:
```bash
rsync -rvzc --progress -e 'ssh -p xx' --progress /home/kin/DATA_HDD/tcp_data/tcp_carla_data kinzhang@xx:~/data
```
### run docker
run docker only for training, download their own dataset

```bash
docker run -it  --gpus all -v /dev/shm:/dev/shm -v /home/kin/DATA_HDD/tcp_data/tcp_carla_data:/home/kin/tcp/data --name kin_tcp zhangkin/tcp /bin/zsh
```

## Training

First, set the dataset path in ``TCP/config.py``.
Training:

```
python TCP/train.py --gpus NUM_OF_GPUS
```

## Data Generation

First, launch the carla server,

```
cd CARLA_ROOT
./CarlaUE4.sh --world-port=2000 -opengl
```

Set the carla path, routes file, scenario file, and data path for data generation in ``leaderboard/scripts/data_collection.sh``.

Start data collection

```
sh leaderboard/scripts/data_collection.sh
```

After the data collecting process, run `tools/filter_data.py` and `tools/gen_data.py` to filter out invalid data and pack the data for training.

## Evaluation

First, launch the carla server,

```
cd CARLA_ROOT
./CarlaUE4.sh --world-port=2000 -opengl
```

Set the carla path, routes file, scenario file, model ckpt, and data path for evaluation in ``leaderboard/scripts/run_evaluation.sh``.

Start the evaluation

```
sh leaderboard/scripts/run_evaluation.sh
```

