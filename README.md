# 4th CLVision Workshop @ CVPR 2023 Challenge

This is the official starting repository for the Continual Learning Challenge held in the 4th CLVision Workshop @ CVPR 2023.

Please refer to the [**challenge website**](https://sites.google.com/view/clvision2023/challenge) for more details!

## Getting started

The devkit is based on the [Avalanche library](https://github.com/ContinualAI/avalanche). We warmly recommend looking at the [documentation](https://avalanche.continualai.org/) (especially the ["Zero To Hero tutorials"](https://avalanche.continualai.org/from-zero-to-hero-tutorial/01_introduction)) if this is your first time using it!
Avalanche is added as a Git submodule of this repository. 

The recommended setup steps are as follows:

1. **Install [conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html)** (and [mamba](https://github.com/mamba-org/mamba); recommended)

2. **Clone the repo** and **create the conda environment**:
```bash
git clone --recurse-submodules https://github.com/ContinualAI/clvision-challenge-2023.git
cd clvision-challenge-2023
conda env create -f environment.yml
```

3. **Start training**: you can directly start training a baseline strategy by running `python train.py`  


The aforementioned steps should be OS-agnostic. However, we recommend setting up your dev environment using a 
mainstream Linux distribution.

## Code Structure

    
    ├── avalanche                      # Avalanche library (as a submodule) 


    ├── benchmarks
        ├── ... 
        ├── cir_benchmark.py           # benchmark generator for the challenge
     
    ├── data 
        ├── ...                        # dataset train/test splits 
    
    ├── models
        ├── ...                       
        ├── resnet_18.py               # base resnet-18 architecture 
    
    ├── scenario_config
        ├── ...                        # config files used for benchmark generation
    
    ├── strategies
        ├── my_plugin.py               # template for implementing new plugins
        ├── my_strategy.py             # template for implementing new strategies

    ├── utils
        ├── ...                        # utility scripts 


    ├── train.py                       # trainer script 
    
## Implementing a strategy 
Put your problem-solving skills to the test and implement new strategies for class-incremental with repetition scenarios in this challenge. You have two options for implementing a new strategy:

#### Strategy as a plugin
The straightforward method to design a strategy is to implement it as a plugin. Plugins extend an existing strategy by implementing a particular set of callbacks. You can implement your plugin in `strategies/my_plugin.py`, and add it a base strategy (e.g. Naive strategy) in `train.py`.

#### Strategy as a subclass
Another way to implement your strategy is to define a class that inherits from `SupervisedTemplate` class. This method is suggested only when the training epoch loops or other behaviors in a strategy are different from thed default ones defined in the `SupervisedTemplate`, and cannot be implemented by extending existing strategies via plugins.  


*For a deeper dive into the implementation of strategies, please refer to [**this link**](https://avalanche.continualai.org/from-zero-to-hero-tutorial/04_training). 

## Submitting a solution
Solutions must be submitted through the CodaLab portal:

A solution consists of a single file that contains the predictions
obtained on the full test set. The training script automatically generates the predictions after each run.

The maximum number of allowed submissions is 20. Only 3 solutions can be submitted each day.

Note: the evaluation for detection tracks may take some minutes to complete.

## Hints

- The devkit may be updated when new features are requested by participants. We recommend checking if there are new updates frequently.
- The `InteractiveLogger` will just print the progress to stdout (and it is quite verbose). Consider using dashboard loggers, 
such as *Tensorboard* or *Weights & Biases*. See the tutorial on 
loggers [here](https://avalanche.continualai.org/from-zero-to-hero-tutorial/06_loggers). 
You can use more than one logger at the same time!

# IDE-specific hints

- When using **Visual Studio Code**, to get a proper type checking and smooth terminal execution, consider:
    - installing the official [Python language support](https://marketplace.visualstudio.com/items?itemName=ms-python.python).
    - creating a `.env` file in the root of the project with the following content:
        ```
        PYTHONPATH=:<project_path>/avalanche:<project_path>
        ```
        where you should replace `<project_path>` with the *absolute path* of the project directory. Alas, utilities like `${workspaceFolder}` will not work here.

    - creating a `.vscode/settings.json` file. Create the `.vscode` directory and `settings.json` file if they do not exist.
    In the `settings.json` file, we recommend pasting the following content:
        ```json
        {
            "terminal.integrated.env.linux": {
                "PYTHONPATH": "${env:PYTHONPATH}:${workspaceFolder}:${workspaceFolder}/avalanche",
                "CUDA_VISIBLE_DEVICES": "1"
            },
            "python.envFile": "${workspaceFolder}/.env",
            "python.terminal.activateEnvironment": true,
            "python.analysis.typeCheckingMode": "basic",
            "python.analysis.extraPaths": [
                "avalanche"
            ],
            "python.languageServer": "Pylance"
        }
        ```
    Once you have followed these steps, close and reopen Visual Studio Code.
