# Installation

```
git clone https://github.com/DAC-Method/DAC.git
cd DAC
conda create --name dac --file conda_requirements.txt
conda activate dac
pip install -r pip_requirements.txt
pip install .
```

## Usage
Get attribution and masks for any image pair:
```
python dac.py --net <net> --checkpoint <net_checkpoint> --input_shape <dx> <dy> --realimg <real_img_path> --fakeimg <fake_img_path> --realclass <real_class_idx> --fakeclass <fake_class_idx> --downsample_factors <d0 d1 d2 ...> --output_classes <N_output_classes> --<attribution_method_0> --<attribution_method_1> ...
```

For all flags, see:
```
python dac.py -h
```
# DAC Resources

Download DAC-Resources [here](todo.com).

## Contents:
  - Raw Datasets for MNIST, SYNAPSES, DISC_A, DISC_B
    ```
    dac_resources/data/raw/<experiment>
    ```
  - Cycle GAN translated and filtered datasets for MNIST, SYNAPSES, DISC_A, DISC_B
    ```
    dac_resources/data/translated/<experiment>
    ```
  - Checkpoints for all networks.
    ```
    dac_resources/data/checkpoints/<experiment>
    ```
  - Scripts to reproduce experimental results.
    ```
    dac_resources/data/experiments/scripts
    ```

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
- Create paired dataset:
```
cd dac
python cycle_gan/prepare_dataset.py --data_dir ~/dac_resources/data/raw/<experiment> --classes <class names>
```
E.g. for disc_a:
```
python cycle_gan/prepare_dataset.py --data_dir ~/dac_resources/data/raw/disc_a --classes 0 1
```
- Start training:
```
python cycle_gan/start_training.py --experiment <experiment> --data_root ~/dac_resources/data/raw/<experiment>/cycle_gan
```

- Translate images:
```
python cycle_gan/start_testing.py --experiment <experiment> --data_root ~/dac_resources/data/raw/<experiment>/cycle_gan --aux_net <vgg/res> --aux_checkpoint ~/dac_resources/checkpoints/<experiment>/classifiers/<vgg/res>_checkpoint --gan_checkpoint_dir <path to gan_checkpoint_dir>
```

