# Fault-location-in-distribution-network

## Research motivation<br>
At present, deep learning has achieved remarkable results in many fields and a lot of work related to data-driven has been done for fault location in distribution network as discussed above, while the deep learning is faced with ***two main challenges*** not considered in previous studies when applied in fault location in distribution network:<br>
* Most of the data obtained based on PMU are steady-state data, while the amount of fault data is relatively small due to the high reliability of the power system. Thus, over-fitting may occur during the  training progress of fault location model by only using the fault data, which would decrease the accuracy of fault location.<br>
* Most of the previous studies assume that the training dataset covers the total operating space of the distribution network, while the historical fault data measured by the PMU cannot completely cover all fault conditions. When a fault condition that does not exist in the training samples needs to be predicted by the pre-trained machine learning model, the fault may not be accurately located.<br>

## My proposed methodology<br>
The architecture of fault location framework:<br>
![图片名称](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Fault%20location%20model%20in%20distribution%20network.png) 

AC-GAN link
The model can be divided in three parts:

* Spectrogram prediction network
* Wavenet vocoder

## Model training strategy<br>
The architecture of fault location framework


## Dataset<br>
The architecture of fault location framework

## Simulation results<br>
The architecture of fault location framework

