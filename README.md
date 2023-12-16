# Creating training and inference environments

## Requirements

This environment was built on Ubuntu 22.04.3 LTS

Python Version - **Python 3.7**

### STEPS TO CREATE ENV

1. Using combination of conda + pip instructions

```
conda create --name robot_arm python=3.7
conda activate robot_arm
conda install -c conda-forge mpi4py mpich
pip install -r requirements.txt
```

or

2. Directly from submission_env.yml file

```
conda env create -f submission_env.yml
```

--------------------------------

# File Descriptions

- **gym_panda/envs/panda_env.py**
In this file you can change the RL environment with which the agent is interacting.
That includes changing the method of the action execution, changing the observation information, obstacle generation, episode length etc.

- **run_training.py**
File with which you run the training.
In this file it's specified which environment, algorithm and training configuration the agent should use. 
Also you can set the name for the output file.

- **test_policy.py**
File to test the learned policy.

--------------------------------

# STEPS TO ADD ADDITIONAL CUBE

## EXAMPLE TO ADD RED LEGO CUBE
copy `lego_red.urdf` to `~/anaconda3/envs/robot_arm/lib/python3.7/site-packages/pybullet_data/lego/`
copy `lego_red.obj` to `~/anaconda3/envs/robot_arm/lib/python3.7/site-packages/pybullet_data/lego/`

To add cubes of different colors:

You should: 
edit the color_name as given in line 20 of sample urdf file - "lego_red.urdf"
edit the rgb values of color to be added as given in line 21 of sample urdf file - "lego_red.urdf"
create a new lego_<color>.obj file similar to the sample provided in this submission - "lego_red.obj"
link the newly .obj file in newly .urdf file as given in line 18 of sample urdf file - "lego_red.urdf"

--------------------------------

# RUN TRAINING PROCESS

```
python run_training.py
```

(or)

If you are using VSCode Debugger to run the training pipeline

`Example VSCode config to run the training pipeline`
```
{
    "name": "train",
    "type": "python",
    "request": "launch",
    "program": "${workspaceFolder}/run_training.py",
    "console": "integratedTerminal",
    "justMyCode": true,
    "args":[
    ]
}
```

Refer - `.vscode/launch.json` file provided in the submission

`Parameters involved in training process - run_training.py`

```
ppo(
	env_fn = lambda : gym.make('panda-v0'),
	ac_kwargs = dict(hidden_sizes=[64,64]),
	logger_kwargs = dict(output_dir='exp-results/'+current_time+file_name, exp_name=(file_name)),
	steps_per_epoch=1000, # default: 4000
    epochs=100,
    max_ep_len=500,
)
```

epochs - Set the number of epochs to run with epoch parameter

Make sure that max episode length is of the same value as defined in *panda_env.py*

--------------------------------

# RUN INFERENCE PIPELINE

```
python test_policy.py /path/to/experiment-results-folder color_query color_name
```

I have added cubes of only two colors:
1. Red
2. Yellow

The steps to add cubes of different colors is specified in "STEPS TO ADD ADDITIONAL CUBE" section above.

Example command
```
python test_policy.py exp-results/2023-12-13_16-28_ppo_action1-EpLen500-spe1000-tip.robot --color_query yellow
```

(or)

If you are using VSCode Debugger to run the inference pipeline

`Example VSCode config to run the inference pipeline`
```
{
    "name": "test",
    "type": "python",
    "request": "launch",
    "program": "${workspaceFolder}/test_policy.py",
    "console": "integratedTerminal",
    "justMyCode": true,
    "args":[
        "exp-results/2023-12-13_16-28_ppo_action1-EpLen500-spe1000-tip.robot",
        "--color_query", "yellow"
    ]
}
```

Refer - `.vscode/launch.json` file provided in the submission

--------------------------------




