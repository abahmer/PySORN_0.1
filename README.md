**Status:** Active (under active development, breaking changes may occur). To reproduce the figures and analysis, navigate to notebooks/alpha_cpu.

For easy install navigate to the latest project https://pypi.org/project/sorn/

# PySORN: Pre-release version 0.1.0

Implementation of SORN for my Master thesis titled

" Self-Organising Recurrent Neural Networks: Prospects of Biologically Plausible Artificial Brain Circuits Solving General Intelligence Tasks at the Imminence of Chaos"

<h4 align="center">SORN Reservoir</h4>

<p align="center">
<a href="url"><img src="https://github.com/Saran-nns/PySORN_0.1/blob/master/v0.1.0/doc/images/SORN1.png" height="350" width="500" ></a>
</p>

<h4 align="center">The evolution of connection strenghts</h4>
 
<p align="center">
<a href="url"><img src="https://github.com/Saran-nns/PySORN_0.1/blob/master/v0.1.0/doc/images/weights.png" height="500" width="450" ></a>
</p>

<h4 align="center">Neural Connectome</h4> 

<p align="center">
<a href="url"><img src="https://github.com/Saran-nns/PySORN_0.1/blob/master/v0.1.0/doc/images/neuralcorrelationall.png" height="450" width="450" ></a>
</p>


### Supporting OS:

Windows 10

### Packages required:

Python 3.6

Pytorch 0.4

OpenAI Gym

CUDA 9.0

For details check requirements.txt

### Installation Instructions

#### i) Create virtual environment

```python
conda create -n virtualenvname python=3.6 anaconda

activate virtualenvname
```

#### ii) Install dependencies

Run:

```python
pip install -r requirements.txt
```
Install OpenAI Gym from source:

Run:
```python
git clone https://github.com/openai/gym
cd gym
pip install -e .
```
Then for complete installation:
 
 ```python
pip install -e .[all]
```
 or
 
 Install it through pip:
 
  ```python
pip install gym
pip install gym[all]
```

#### iii) Add the project folder to sys path:

Navigate to project folder in shell: Eg: /PySORN_0.1/src/alpha_cpu

Run:
```python
python setup.py
```


### Usage: OpenAI Gym

##### Network hyperparameters:

Open conifiguration.ini file and edit network variables as required

##### Sample usage

```python
# Imports

import utils.InitHelper as initializer
from sorn import Sorn, Plasticity, TrainSorn, TrainSornPlasticity
import gym

# Load the simulated network matrices
# Note these matrices are obtained after the network achieved convergence under random inputs and noise

with open('simulation_matrices.pkl','rb') as f:  
    sim_matrices,excit_states,inhib_states,recur_states,num_reservoir_conn = pickle.load(f)


# Training parameters

NUM_EPISODES = 2e6
NUM_PLASTICITY_EPISODES = 20000

env = gym.make('CartPole-v0')

for EPISODE in range(NUM_EPISODES):
    
    # Environment observation
    state = env.reset()[None,:]
    
    # Play the episode
    
    while True:
      
      if EPISODE < NUM_PLASTICITY_EPISODE:
      
        # Plasticity phase
        sim_matrices,excit_states,inhib_states,recur_states,num_reservoir_conn = TrainSornPlasticity.train_sorn(phase = 'Plasticity',
                                                                                                            matrices = sim_matrices,
                                                                                                            inputs = state)

      else:
        # Training phase with frozen reservoir connectivity
        sim_matrices,excit_states,inhib_states,recur_states,num_reservoir_conn = TrainSorn.train_sorn(phase = 'Training',
                                                                                                            matrices = sim_matrices,
                                                                                                            inputs = state)
      
      # Feed excit_states as input states to your RL algorithm, below goes for simple policy gradient algorithm
      # Sample policy w.r.t excitatory states and take action in the environment
       
      probs = policy(np.asarray(excit_states),output_layer_weights))
      action = np.random.choice(action_space,probs)
      state,reward,done,_ = env.step(action) 
      
      if done:
        break
```

