# Fault-location-in-distribution-network

## Research motivation and solutions<br>
At present, deep learning has achieved remarkable results in many fields and a lot of work related to data-driven has been done for fault location in distribution network as discussed above, while the deep learning is faced with ***two main challenges*** not considered in previous studies when applied in fault location in distribution network:<br>
* Most of the data obtained based on PMU are steady-state data, while the amount of fault data is relatively small due to the high reliability of the power system. Thus, over-fitting may occur during the  training progress of fault location model by only using the fault data, which would decrease the accuracy of fault location.<br>
* Most of the previous studies assume that the training dataset covers the total operating space of the distribution network, while the historical fault data measured by the PMU cannot completely cover all fault conditions. When a fault condition that does not exist in the training samples needs to be predicted by the pre-trained machine learning model, the fault may not be accurately located.<br>

In order to realize the accurate fault location in multi-source distribution network by using fault data with small size and low completeness, there are the ***breif solutions*** showing as follows:<br>
*	**Convolution-transposed convolution network** ([CTN](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8618415)) is adopted to establish the mapping from stable-state data to fault data to realize the estimation of fault conditions not collected in historical dataset. **Auxiliary classifier generative adversarial networks** ([AC-GAN](https://arxiv.org/pdf/1610.09585.pdf)) based on CNN is proposed here which can effectively learn the distribution characteristics of real fault data and generate high quality samples of fault data in line with the actual situation, thus augmenting the dataset. <br>
*	In view of the extraordinary feature extraction ability of **CNN** on image recognition, time series signals of fault voltage and current acquired by PMU are stored as multi-dimensional arrays which are similar to the form of images and we leverage a CNN classifier to perform automatic feature extraction on them for locating fault. <br>

## My proposed methodology<br>
The architecture of fault location model:<br>

![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Fault%20location%20model%20in%20distribution%20network.png) 

The model can be divided in three parts:

* fault data estimation (constructed by CTN)
* fault data augmentation (constructed by AC-GAN)
* fault location classifier (constructed by CNN)

Firstly, incomplete dataset are input into CTN to get fault estimation dataset.
Secondly, the complete dataset is used as the real data Xreal for training AC-GAN. The augmentation dataset is generated by the trained AC-GAN generator under the specified category code c, and the total dataset is obtained by combining augmentation dataset and  complete dataset.<br>

If you are not familiar with **GAN**, there is the papper you [need](https://arxiv.org/pdf/1406.2661.pdf). The structure of GAN consists of discriminator (D) and generator (G), which is shown as:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/ac-gan%20arcitecture.png) 

## Model training strategy with transfer learning<br>
A parameter fine-tuning method based on steady-state data pre-training is leveraged to solve the over-fitting problem and speed up the training of the overall model. The breif steps are as follows:<br>
* Pre-training CTN
* Pre-training AC-GAN discriminator
* Parameter transfer for fault location classifier 
* Training process optimization through Batch Normalization

If you are not familiar with **transfer learning**, there is the papper you [need](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5288526). 

## Dataset<br>
### A standard IEEE 33 bus model is built in PSCAD/EMTDC for data acquisition:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/33-bus%20multi-source%20distribution%20network.png) 

### The second harmonic signals of negative sequence voltage and current measured at bus 22 when two-phase short circuit occurs at bus 6, and fualt resistance is 0.01Ohm during simulation time of 0.4s~0.7s:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Second%20harmonic%20signals%20of%20negative%20sequence%20voltage%20and%20current%20measured%20at%20bus%2022.png) 

In this way, fault data and steady-state data under various operation conditions can be collected as multidimensional array, and ensure there is **no intersection** between training samples and testing samples.<br>

## Some simulation results<br>
### Fault data estimation results:<br>
>The fault data can be well estimated by steady-state data along with the fualt resistance.
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Comparison%20of%20%20CTN%20fault%20data%20estimation%20results.png)<br>

### Fault data augmentation results under different training epochs:<br>
>The data generated by AC-GAN after 300 training epochs are very close to the real data with high quality.
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/AC-GAN%20data%20generation%20results%20when%20C%3D1%20during%20training%20process.png)<br>
 
### Visualization of AC-GAN discriminator output features:<br>
>The t-SNE two-dimensional visualization results of the complete dataset are shown. The numbers in color bar stand for corresponding fault location categories. The clustering of the same types of faults shows that AC-GAN has the ability to distinguish data categories. 
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Visualization%20of%20discriminator%20output%20features%20of%20AC-GAN.png)<br>

### Curves of various errors and fault location accuracy with our without training strategy:<br>
>The training process would be more steady and the converge speed would be much more quick. 
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/training%20strategy.png)<br>

### The influence of convolution structure change on fault location accuracy:<br>
>Adding or subtracting the layer of convolution network has slight influence on location accuracy, which shows that the model has good robustness. <br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/structure%20change.png)<br>

### The influence of data normalization on fault location accuracy:<br>
>![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/normalization.png)

