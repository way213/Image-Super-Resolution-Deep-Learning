
# This repository contains an augmented version of the FSRCNN model developed by Lornatang referenced below.

## Authors: Masaaki Kato (mak991@nyu.edu) , William Yang (way213@nyu.edu)


Objective: 

We are planning to modify the Fast Super-Resolution Convolutional Neural Network (FSRCNN) model introduced in the “Accelerating the Super-Resolution Convolutional Neural Network” (Chao Dong et.al. 2016) paper. We hypothesize that our model will decrease computation and inference time while maintaining accuracy. We will follow the same steps for training and testing in the paper so that we can compare the performance of our models with that of the original architecture. The number of epochs will be reduced due to hardware inavalibilities.

# Steps to Proceed:

To start, the datasets have to be downloaded and unzipped in the link below, then placed into the "data" folder. 

https://drive.google.com/drive/folders/1A6lzGeQrFMxPqJehK9s37ce-tPDj20mD?usp=sharing

Now, the "run.py" file was ran, to initialize and seperate the data into train/test. 

## We are now ready to train the model through running the "train.py" file:

Modifications to the "config.py" file will be needed in reguards to the variables below:


upscale_factor : (2,3,4).

mode : (valid,train).

validate_data : (Set5,Set14,BSDS200).

exp_number : (0,1,2).


The variable names are self explanatory, and the varaible "exp_number" references on how we will be building the model. In our proposal, we suggested that we could try and manipulate the mapping layers of the model to make it so that the inference time could be reduced. 

The mapping layer in the original FSRCNN uses a Conv(3x3) filter. We propose 2 modified architectures: 1) 2  Conv(2x2) filters, 2) a Conv(1x3) filter followed by a Conv(3x1) filter (1xN → Nx1).

The exp_number variable references the different types of archeticture we will base the model on, as seen above.


For example, the "train.py" file would be run where the variables within the "config.py" file are as such : upscale_factor = 2, mode = train, exp_number = 0.

This would create a model that was trained while using the original archeticture with an upscale_factor of 2. For each case, we will re-train the model on the same dataset, and identify the runtime and accuracy.

## After creating every possible model conbination, we are now ready to validate the models:

"validate.py" will be ran and the model will be validated based on what the variable "validate_data" is referencing, please note to apply this change "mode = valid", when running "validate.py".

The results of the validated images will be stored in a folder with it's corresponding name under "results/test/{here}"

Kato Results
| Test Set/Experiment | original  | exp0    | exp1    | exp2    |
|---------------------|------------|-----------|-----------|-----------|
| Set5_x2            | 36.94(0.068) | 27.95(0.0245) | 29.17(0.0296) | 29.80(0.0288) |
| Set14_x2           | 32.54(0.16) | 25.89(0.0169) | 26.80(0.0184) | 27.39(0.02) |
| BSD_x2             | 31.73(0.098) | 26.66(0.0018) | 27.31(0.0022) | 27.88(0.0024) |
| Set5_x3            | 33.06(0.027) | 24.09(0.0234) | 21.88(0.0255) | 25.67(0.0264) |
| Set14_x3           | 29.37(0.061) | 23.03(0.0168) | 20.86(0.0178) | 24.08(0.0201) |
| BSD_x3             | 28.55(0.035) | 23.98(0.0017) | 21.45(0.0021) | 25.08(0.0024) |
| Set5_x4            | 30.55(0.015) | 22.02(0.0228) | 18.34(0.0533) | 18.09(0.0278) |
| Set14_x4           | 27.50(0.029) | 21.52(0.017) | 18.20(0.0225) | 17.77(0.0193) |
| BSD_x4             | 26.92(0.019) | 22.50(0.0018) | 19.20(0.0021) | 18.55(0.0024) |

| Upscale Factor/Exp | Exp0 (seconds) | Exp1 (seconds) | Exp2 (seconds) |
|--------------------|----------------|----------------|----------------|
| x2                 | 409.6121       | 501.4475      | 592.1323      |
| x3                 | 376.0743       | 473.6100      | 584.1110      |
| x4                 | 372.0422       | 468.29960     | 568.9024      |


Yang Results: 
| Test Set/Experiment | original  | exp0    | exp1    | exp2    |
|---------------------|------------|-----------|-----------|-----------|
| Set5_x2            | 36.94(0.068) | 27.75(0.157) | 29.17(0.1903) | 29.72(0.1696) |
| Set14_x2           | 32.54(0.16) | 25.73(0.094) | 26.80d(0.102) | 27.36(0.1023) |
| BSD_x2             | 31.73(0.098) | 26.56(0.003) | 27.36(0.0043) | 27.84(0.0055) |
| Set5_x3            | 33.06(0.027) | 23.86(0.185) | 21.92(0.2103) | 25.44(0.324) |
| Set14_x3           | 29.37(0.061) | 22.89(0.139) | 20.95(0.1412) | 23.88(0.3) |
| BSD_x3             | 28.55(0.035) | 23.73(0.005) | 21.50(0.0054) | 24.78(0.0076) |
| Set5_x4            | 30.55(0.015) | 21.97(0.174) | 18.30(0.1551) | 18.50(0.3429) |
| Set14_x4           | 27.50(0.029) | 21.42(0.088) | 18.06(0.0848) | 18.17(0.1962) |
| BSD_x4             | 26.92(0.019) | 22.28(0.004) | 18.98(0.005) | 19.02(0.0073) |

| Upscale Factor/Exp | Exp0 (seconds) | Exp1 (seconds) | Exp2 (seconds) |
|--------------------|----------------|----------------|----------------|
| x2                 | 672.2016       | 667.001299     | 698.184        |
| x3                 | 504.1806       | 497.7157       | 506.2721       |
| x4                 | 432.3168       | 487.743        | 543.1881       |

## Conclusions:

Unfortunately, our experiment models had a longer inference/training time compared to the original model.

This may be because the network is shallow. So the effects of the small filters are not visible

Interestingly, our experiment models has a better PSNR than the original model (at least for upscale factor of 2) despite using small filters.

Exp0 (original replication) model had the lowest inference time across both machines. The goal of the project was not met; however, we have seen that the PSNR of our proposed models (upscale factor of 2) are slightly higher than the original replication. 

Original experiment is more adaptable compared to our models. Looking across all the different upscale factors we can see that the original model is more resilient to such parameters.

Overall, although our experiment did not accomplish the original goal (lower inference time), we believe our experiment results can contribute to future research on Super-Resolution

