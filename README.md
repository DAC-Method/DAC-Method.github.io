# Installation

```
git clone https://github.com/DAC-Method/DAC.git
cd DAC
conda create --name dac --file conda_requirements.txt
conda activate dac
pip install -r pip_requirements.txt
pip install .
```

# DAC Resources

Download DAC-Resources [here](todo.com).

## Contents:
  - Raw Datasets for MNIST, SYNAPSES, DISC_A, DISC_B
  - Translated and processed datasets for MNIST, SYNAPSES, DISC_A, DISC_B
  - Checkpoints and architecture specifications for all networks.
  - Scripts to reproduce experimental results & train cycle GANs.

## Instructions
### Run attribution & mask extraction on already translated images

```
cd dac_resources
python experiments/scripts/run_dac.py --config experiments/configs/<experiment>.ini --net <net>
```

For experiment in {synapses, mnist, disc_1a, disc_1b}, net in {VGG, RES}. Results will be stored in 
```
experiments/results/<experiment>
```

Default behaviour is to run the script locally using only a single worker. This can be changed via editing 
the submit command and num_workers in the respective experiment config file at:
```
experiments/configs/<experiment>.ini
```

### Visualize and calculate DAC scores

```
python experiments/scripts/plot_dac.py --result_dir experiments/results/<experiment> --experiment <experiment> --net <net>
```
Plots will be stored at 
```
./dac_plot_<experiment>_<net>.png
```
AUC scores for each method shown in console.
   

### Train cycle GANs and generate translated datasets from scratch
