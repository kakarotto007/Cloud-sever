# Data augmentation

To improve the generalization of the model,we employ **random cutout**,**time-frequency masking**, **frequency shifting** and **augmix**.

**在fdy-sed的源码里面有：**

- frame_shift 
- mixup
- time_mask
- feature_transformation
- filt_aug
- freq_mask
- add_noise

**ad-yolo源码用到的有：**

- SpecAug
- RotationAug

https://github.com/sadPororo/AD-YOLO

**L3DAS22-TASK2后面会放data augmentation：**

https://github.com/Jinbo-Hu/L3DAS22-TASK2

**Spatial mixup**：

https://github.com/rfalcon100/Spatial-Mixup-Pytorch

**2022 workshop**

![image-20231107163353857](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231107163353857.png)



# PIT

设事件种类总数为N。模型有多个输出轨道，输出轨道的数量取决于假设的同一时刻最大发生事件数，设为M。单个时间帧内，模型的输出为M*N张量，1表示发生，0表示不发生。假设M=2，N=10，若真实标签为仅事件10发生，则给模型计算loss时分配的标签有两种情况：A. 坐标(1,10)处为1，其余为0；B. 坐标(2,10)处为1，其余为0。A和B两种情况均可表示事件10发生，但会使网络输出产生歧义。更多细节可搜索：排列不变性训练。



# L3DAS22-TASK2复现

## dataset

```html
l3das21
        ├── L3DAS_Task2_dev
        │   ├── data
        │   └── labels
        ├── L3DAS_Task2_test
        │   └── data
        └── L3DAS_Task2_train
            ├── data
            └── labels
```

FOA格式记录， placing **two** first-order Ambisonics microphones in the center of the room and moving a speaker reproducing the analytic signal in 252 fixed spatial positions. 原始声音与Ambisonics脉冲响应（IRs）卷积可以在特定位置上合成具有方向性和定位感的声音。因此，Ambisonics脉冲响应用于将原始声音转换为在三维空间中可定位的声音源，以创建逼真的三维声音体验。

一共900 个 60 秒

训练集（SE 为 44 小时，SELD 为 600 小时）和测试集（SE 为 6 小时，SELD 为 5 小时），注意创建相似的分布。SELD 部分的所有组都分为：OV1、OV2、OV3。 这些分区指的是可能重叠的声音的最大数量，分别是1、2或3。
