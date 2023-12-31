# Data augmentation

To improve the generalization of the model,we employ **random cutout**,**time-frequency masking**, **frequency shifting** and **augmix**.

**《Divided spectro-temporal attention for sound event localization and detection in real scenes for DCASE2023 challenge》和《Data augmentation and squeeze-and-excitation network on multiple dimension for sound event localization and detection in real scenes,》用到的：**

* frameshift
* time masking
* channel swap
* mix-up

**《Sound Event Localization and Detection for Real Spatial Sound Scenes: Event-Independent Network and Data Augmentation Chains》用到的：**  几个操作是干嘛的，论文中有详细介绍

* Mixup [15], 
* Cutout [16], 
* SpecAugment [17],
* frequency shifting [18].
* rotation of FOA signals

![image-20231113215637528](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231113215637528.png)

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

# 生成仿真数据

https://github.com/danielkrause/DCASE2022-data-generator

We generated simulated data using the generator code provided by DCASE1. We synthesize multi-channel spatial recordings by convolving monophonic sound event examples with multi-channel Spatial Room Impulse Responses (SRIRs). Samples of sound events are selected from AudioSet [18] and FSD50K [19], based on the affinity of the labels in those datasets to target classes in STARSS23. PANNs [20] are then employed to clean the selection of the clips. We use pretrained PANNs to infer these clips and select high-quality clips based on output probability above 0.8. We extracted SRIRs from the TAU Spatial Room Impulse Response Database (TAU-SRIR DB)2, which contains SRIRs captured in 9 rooms at Tampere University. It was used for official synthetic datasets in DCASE 20192021 [1, 17, 21]. The 2700 1-minute audio clips that we synthesized using the abovementioned SRIRs from 9 rooms are used for Dtrain, and all of dev-set of STARSS23, recorded in 16 rooms, are used for Dtest.

我们使用DCASE1提供的生成器代码生成模拟数据。我们通过将单声道声音事件实例与多通道空间脉冲响应( Spatial Room Impulse Responses，SRIRs )进行卷积来合成多通道空间记录。声音事件的样本选自AudioSet [ 18 ]和FSD50K [ 19 ]，基于这些数据集中的标签与STARSS23中的目标类的亲和性。然后使用PANNs [ 20 ]来清洗剪辑的选择。我们使用预训练的PANNs来推断这些片段，并根据输出概率在0.8以上来选择高质量的片段。我们从TAU空间房间冲激响应数据库( TAU-SRIR DB)2中提取了SRIR，该数据库包含了在坦佩雷大学9个房间捕获的SRIR。它被用于DCASE 20192021 [ 1、17、21]中的官方合成数据集。2700 1 -



The 2700 1-minute audio clips that we synthesized using the abovementioned SRIRs from 9 rooms are used for Dtrain, and all of dev-set of STARSS23, recorded in 16 rooms, are used for Dtest.

使用上述9个房间的SRIR合成的2700个1分钟音频片段用于Dtrain，16个房间记录的所有STARSS23开发集用于Dtest。

# SELD评价指标

SwG-former

# Multi-Scale

![image-20231114165159533](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231114165159533.png)

![image-20231114165225070](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231114165225070.png)

!(https://raw.githubusercontent.com/kakarotto007/final/master/image-20231114165159533.png)

![image-20231114165216084](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231114165216084.png)

# 分帧

一帧内可以看作是平稳的信号，可以做傅里叶变换来搞清楚内部各个频率成分的分布。

分帧的长度：

- 从宏观上看，它必须足够短来保证帧内信号是平稳的。前面说过，口型的变化是导致信号不平稳的原因，所以在一帧的期间内口型不能有明显变化，即一帧的长度应当小于一个**音素**的长度。正常语速下，音素的持续时间大约是 50~200 毫秒，所以帧长一般取为小于 50 毫秒。
- 从微观上来看，它又必须包括足够多的振动周期，因为傅里叶变换是要分析频率的，只有重复足够多次才能分析频率。语音的基频，男声在 100 赫兹左右，女声在 200 赫兹左右，换算成周期就是 10 毫秒和 5 毫秒。既然一帧要包含多个周期，所以一般取至少 20 毫秒。

这样，我们就知道了帧长一般取为 **20 ~ 50 毫秒**，20、25、30、40、50 都是比较常用的数值。

分帧的帧数：

![image-20231124100429354](https://raw.githubusercontent.com/kakarotto007/final/master/image-20231124100429354.png)

# 加窗

[加窗](https://www.zhihu.com/search?q=加窗&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A225983811})的目的是让一帧信号的幅度在两端**渐变**到 0。渐变对傅里叶变换有好处，可以让频谱上的各个峰更细，不容易糊在一起（术语叫做**减轻频谱泄漏**）

加窗的代价是一帧信号两端的部分被削弱了，没有像中央的部分那样得到重视。弥补的办法是，帧不要背靠背地截取，而是相互重叠一部分。相邻两帧的起始位置的时间差叫做**[帧移](https://www.zhihu.com/search?q=帧移&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A225983811})**，常见的取法是取为**帧长的一半**，或者固定取为 **10 毫秒**。

# 分帧加窗的代码实现

```python
def enframe(signal, frame_len=frame_len, frame_shift=frame_shift, win=np.hamming(frame_len)):
    """
    calculate the number of frames: 
    frames = (num_samples -frame_len) / frame_shift +1
    """
    num_samples = signal.size
    num_frames = np.floor((num_samples - frame_len) / frame_shift)+1  
    # calculate the numbers of frames
    frames = np.zeros((int(num_frames),frame_len))   # (num_frames,frame_len)
    # Initialize an array for putting the frame signals into it
    for i in range(int(num_frames)):
        frames[i,:] = signal[i*frame_shift:i*frame_shift + frame_len]
        frames[i,:] = frames[i,:] * win
    return frames
```

