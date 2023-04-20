## Description

This is the code for the paper [Proximal Curriculum for Reinforcement Learning Agents, TMLR 2023](https://openreview.net/pdf?id=8WUyeeMxMH). 

## Getting Started

The organization of the repository has the following structure:

```
tmlr2023_proximal-curriculum-rl
│   README.md
│   main.py    
│   requirements.txt
│
└───abstract_classes
│      AbstractTeacher.py
│      AbstractGymWrapper.py
│
└───envs 
   │───ant_goal
   │   │    AntGoalTeacher.py
   │   │    AntGoalWrapper.py
   │   │    ant.py
   │   └─── task_datasets
   │            ant_goal_train_data.csv  
   │
   └───ball_catching
   │   │    BallCatchingTeacher.py
   │   │    BallCatchingWrapper.py
   │   │    ball_catching_env.py
   │   └─── task_datasets
   │            ball_catching_train_data.csv  
   │            ball_catching_test_data.csv
   │
   └───basic_karel
   │   │    KarelTeacher.py
   │   │    KarelWrapper.py
   │   │    KarelCompiler.py
   │   │    utils.py
   │   └─── data
   │            train
   │            val
   │
   └───pointmass
       │    PointMassTeacher.py
       │    PointMassWrapper.py
       └─── pm_dense
       │        original_pointmass.py      
       └─── pm_sparse
       │        binary_pointmass.py
       └─── tasks_datasets
                cpm_train_100.csv
                cpm_test_100.csv            
```

There are two abstract classes, _AbstractTeacher.py_ and _AbstractGymWrapper_ which contain all the abstract methods that need to be implemented in order to do curriculum experiments for any gym environment. Inside the _envs_ folder there are the 4 subfolders which contains the 5 environment used for the experiments. Each subfolder in addition to the original environment, has concrete implementations of the abstract classes and the datasets used. The _main.py_ can be used to run the different curriculums and environments. For BasicKarel experiments first unzip the data folder containing train/val tasks.

### Dependencies/Installation

We used Python 3.9.4 to run the experiments. You can install all the necessary libraries and dependencies from the _requirements.txt_. For the BallCatching and AntGoal environment you need mujoco-py. For this to work, you have to set up MuJoCo for Python 3 according to this [guide](https://github.com/openai/mujoco-py). Moreover, [Weights and Biases](https://wandb.ai/site) platform is being used for live visualization during training and logging of the experiments. In case you don't want to use it, you can set the argument _--wandb_ to False.

### Executing program

To run the experiments you can use _main.py_ with the following arguments:
```
usage: main.py [-h] [--option [OPTION]] [--wandb [WANDB]]
               [--tensorboard [TENSORBOARD]] [--env_name [ENV_NAME]]
               [--curriculum [CURRICULUM]] [--seed [SEED]]
               [--beta [BETA]] [--noise [NOISE]]
               [--spdl_threshold [SPDL_THRESHOLD]] [--model_path [MODEL_PATH]]
```

Here I explain the arguments:
 - option: can be set to "train", or "test"
 - wandb: boolean, for using wandb integration. In that case you have to specify your API_KEY
 - tensorboard, boolean, for using tensorboard logging
 - env_name: you can set any of the 4 environments, "PointMassSparse", "PointMassDense", "BasicKarel", "BallCatching", "AntGoal"
 - curriculum: any of the curriculum strategies, i.e., "procurl-env", "procurl-val", "procurl-iid", "iid", "space", "space-alt", "spdl", "easy", "hard"
 - seed: you can specify a random seed to set for the experiments
 - beta: parameter used in the curriculum strategy
 - noise: level of noise added to critic values. Used for robustness study
 - spdl_threshold: performance threshold parameter used in SPDL strategy
 - model_path: if you use test option you can specify the path/paths of the trained model/models

We provide an example command for starting training with "procurl-val" curriculum on PointMassSparse environment with wandb and tensorboard logging:

```
python main.py --option train --wandb true --tensorboard true --env_name PointMassSparse --curriculum procurl-val
```

Tensorboard is already integrated in the experiments to visualize training and validation progress. By running _main.py_ a log folder _run_ with the specific run is created.
If you want to deactivate this option set _--tensorboard_ argument to False. Moreover, the best model is being saved in the _models_ directory that is created.

### Citation

```
@article{tzannetos2023proximal,
  author  = {Georgios Tzannetos and B\'arbara Gomes Ribeiro and Parameswaran Kamalaruban and Adish Singla},
  title   = {{P}roximal {C}urriculum for {R}einforcement {L}earning {A}gents},
  journal = {Transactions of Machine Learning Research (TMLR)},
  year    = {2023},
}
```

