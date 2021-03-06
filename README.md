# GANomaly_s

This repository modification PyTorch implementation of the following paper: GANomaly: Semi-Supervised Anomaly Detection via Adversarial Training [[1]](#reference)

##  1. Table of Contents
- [GANomaly](#ganomaly)
    - [Table of Contents](#table-of-contents)
    - [Installation](#installation)
    - [Experiment](#experiment)
    - [Training](#training)
        - [Training on MNIST](#training-on-mnist)
        - [Training on CIFAR10](#training-on-cifar10)
        - [Train on Custom Dataset](#train-on-custom-dataset)
    - [Citing GANomaly](#citing-ganomaly)
    - [Reference](#reference)
    

## 2. Installation
1. First clone the repository
   ```
   git clone https://github.com/RForestLiu/ganomaly_s.git
   ```
2. Create the virtual environment via conda
    ```
    conda create -n ganomaly python=3.7
    ```
3. Activate the virtual environment.
    ```
    conda activate ganomaly
    ```
3. Install the dependencies.
   ```
   pip install --user --requirement requirements.txt
   ```

## 3. Experiment
To replicate the results in the paper for MNIST and CIFAR10  datasets, run the following commands:

``` shell
# MNIST
sh experiments/run_mnist_s.sh

# CIFAR
sh experiments/run_cifar_s.sh # CIFAR10
```

## 4. Training
To list the arguments, run the following command:
```
python train.py -h
```

### 4.1. Training on MNIST
To train the model on MNIST dataset for a given anomaly class, run the following:

``` 
python train.py 				\
	--dataset mnist				\
	--isize 32				\
	--nc 1					\
	--niter <number-of-epochs>		\
	--manualseed 1				\
	--nz 256				\
	--abnormal_class <0,1,2,3,4,5,6,7,8,9>	\
	--display                               # optional if you want to visualize     
```

Wait for first stage finish.Then run the following:

``` 
python train_z.py 				\
    --dataset mnist				\
    --isize 32 					\
    --load_weights				\
    --niter <number-of-epochs>        	  	\
    --strengthen 1				\
    --nz 256					\
    --classifier				\
    --nc 1					\
    --z_metric roc				\
    --manualseed 1				\ # optional if you want to set seed
    --abnormal_class <0,1,2,3,4,5,6,7,8,9>	\
    --display					# optional if you want to visualize     
```

### 4.2. Training on CIFAR10
To train the model on CIFAR10 dataset for a given anomaly class, run the following:

``` 
python train.py					\
	--dataset cifar10			\
	--isize 32				\
	--niter <number-of-epochs>		\
	--nz 256				\
	--manualseed 1				\  #optional if you want to set seed
	--abnormal_class cat			\
	<plane, car, bird, cat, deer, dog, frog, horse, ship, truck>	\
	--display			# optional if you want to visualize   
```
Wait for first stage finish.Then run the following:

``` 
python train_z.py				\
	--dataset cifar10			\
	--isize 32				\
	--load_weights				\
	--strengthen 1				\
	--nz 256				\
	--classifier				\
	--z_metric roc				\
	--manualseed 1				\ #optional if you want to set seed
	--abnormal_class cat			\
	--display			# optional if you want to visualize
```

### 4.3. Train on Custom Dataset
To train the model on a custom dataset, the dataset should be copied into `./data` directory, and should have the following directory & file structure:

```
Custom Dataset
├── test
│   ├── 0.normal
│   │   └── normal_tst_img_0.png
│   │   └── normal_tst_img_1.png
│   │   ...
│   │   └── normal_tst_img_n.png
│   ├── 1.abnormal
│   │   └── abnormal_tst_img_0.png
│   │   └── abnormal_tst_img_1.png
│   │   ...
│   │   └── abnormal_tst_img_m.png
├── train
│   ├── 0.normal
│   │   └── normal_tst_img_0.png
│   │   └── normal_tst_img_1.png
│   │   ...
│   │   └── normal_tst_img_t.png

```

Then model training is the same as training MNIST or CIFAR10 datasets explained above.

```
python train.py 
	--dataset <name-of-the-data>	\
	--isize  <image-size>		\
	--strengthen 1			\
	--niter <number-of-epochs>	\
	--nz 1024			\
	--display 	# optional if you want to visualize
```

Wait for first stage finish.Then run the following:

```
python train_z.py		\
	--dataset <name-of-the-data>	\
	--isize <image-size> 		\
	--niter <number-of-epochs>	\
	--load_weights 			\
	--strengthen 1 			\
	--classifier			\
	--nz 1024 			\
	--display	# optional if you want to visualize
```

For more training options, run `python train.py -h`.
