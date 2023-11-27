# 11月

## 11.6

* [x] java深拷贝和浅拷贝
* [x] java值传递、引用传递
* [ ] 《FN-SSL: Full-Band and Narrow-Band Fusion for Sound Source Localization》看了一点，感觉是借鉴的SE的。

## 11.7

* [x] 找论文
* [x] 小组会分享META-SELD
* [ ] 自己生成音频数据，熟悉流程。
* [ ]  准备看《TF-GRIDNET: MAKING TIME-FREQUENCY DOMAIN MODELS GREAT AGAIN FOR MONAURAL SPEAKER SEPARATION》
* [ ]  有时间的话看看《HYBRID TRANSFORMERS FOR MUSIC SOURCE SEPARATION》 github：7k。。
* [x] java 并发

## 11.8

* [x] 看meta-seld
* [x] 准备看《Sound Event Localization and Detection for Real Spatial Sound Scenes: Event-Independent Network and Data Augmentation Chains》workshop
* [ ] 还有《A Time-Frequency Attention Module for Neural Speech Enhancement》
* [x] 看《FN-SSL: Full-Band and Narrow-Band Fusion for Sound Source Localization》
* [ ] 《TFECN: Time-Frequency Enhanced ConvNet for Audio Classification》新的时间和频率卷积。
* [ ] 《Binaural Sound Localization in Noisy Environments Using Frequency-Based Audio Vision Transformer (FAViT)》双耳定位用到了频率的transformer
* [x] 准备看《**PLDISET: Probabilistic Localization and Detection of Independent Sound Events with Transformers**》workshop
* [x] 准备看《Frequency & Channel Attention for Computationally Efficient Sound Event Detection》workshop
* [x] 准备看《**Multi-Resolution Conformer for Sound Event Detection: Analysis and Optimization**》workshop
* [x] 后面开始先看workshop把
* [x] 该看BLOCKED 与 RUNNABLE 状态的转换了


## 11.9

* [x] meta-seld实验细节
* [ ] 后面看看迁移学习
* [x] 李宏毅meta-learning 
* [x] 准备看《DiffSED: Sound Event Detection with Denoising Diffusion》sed 的diffusion model
* [x] 看《图卷积SELD》准备分享

## 11.10

* [x] 准备看《Divided spectro-temporal attention for sound event localization and detection in real scenes for DCASE2023 challenge》Arxiv 202306
* [ ] 复现sota
* [x] 3D卷积 ，只是多了一个时间维度，用于处理视频或者医疗图像领域
* [x] GCN

## 11.11

* [x] java 的 collection
* [ ] 看一下LSTM
* [x] 看一下soft-parameter

## 11.13

* [x] java字符串题
* [x] 精读SwG-former 并且做ppt草稿
* [x] 看《Divided spectro-temporal attention for sound event localization and detection in real scenes for DCASE2023 challenge》Arxiv 202306  通过减小频率池化层来得到有F的维度（B，T，F，C），然后输入spetro和temporal attention，（BT，F,C）(BF,T,C)
* [x] 看《Data augmentation and squeeze-and-excitation network on multiple dimension for sound event localization and detection in real scenes》 在channel和freq 维度 做 se 
* [x] 看《Sound Event Localization and Detection for Real Spatial Sound Scenes: Event-Independent Network and Data Augmentation Chains》workshop  用了 augmentation chains  ：先copy三份，分别做data augmentation 然后混合在一起。

## 11.14

* [x] 完善ppt
* [x] 准备找一下有关图卷积的音频的文章
* [x] KNN

## 11.15

* [x] 继续完善ppt
* [x] mysql p10

## 11.16

* [x] 看《**PLDISET: Probabilistic Localization and Detection of Independent Sound Events with Transformers**》workshop   用了transformer，PLDISET。以及轨迹生成。
* [x] 看《Frequency & Channel Attention for Computationally Efficient Sound Event Detection》workshop  在freq维度用了SE模块，并且在squeeze操作中只池化了通道维度，以达到对每一帧的注意力。南塔天？
* [x] 准备看《**Multi-Resolution Conformer for Sound Event Detection: Analysis and Optimization**》workshop  超参数设置改进Conformer的效果，STFT变换的选取来分析多尺度分斌率。

## 11.17

* [x] 看《DiffSED: Sound Event Detection with Denoising Diffusion》sed 的diffusion model
* [x] 找一下有关图卷积的音频的文章《Time-Domain Speech Separation Networks With Graph Encoding Auxiliary》、《Compact Graph Architecture for Speech Emotion Recognition》、《Multi-Channel Speech Enhancement Using Graph Neural Networks》
* [x] 找一下有关diffusion的音频的文章 ：《Separate And Diffuse: Using a Pretrained Diffusion Model for Improving Source Separation》和《Analysing Diffusion-based Generative Approaches Versus Discriminative Approaches for Speech Restoration》

## 11.21

* [x] 看 《Time-Domain Speech Separation Networks With Graph Encoding Auxiliary》
* [x] 看 《Multi-Channel Speech Enhancement Using Graph Neural Networks》
* [ ] 《Compact Graph Architecture for Speech Emotion Recognition》有源码（https://github.com/AmirSh15/Compact_SER）

## 11.22

* [x] 《Separate And Diffuse: Using a Pretrained Diffusion Model for Improving Source Separation》
* [ ] 《Analysing Diffusion-based Generative Approaches Versus Discriminative Approaches for Speech Restoration》
* [ ] 准备看《Voice separation with an unknown number of multiple speakers.》音源分离不确定声源个数

## 11.23

* [x] 准备看stft音频（哔哩哔哩）
* [x] baseline配置
* [x] 新中特作业
* [ ] 矩阵论作业
* [ ] 交叉验证是啥?

## 11.24

### 上午

* [x] 看stft音频（哔哩哔哩）
* [x] 分帧加窗过程，如何求帧的个数  frame_num = N - frame_size / hop_size   + 1
* [ ] baseline的label源码（还不会，下午调试）

### 下午

搞baseline代码

## 11.27

### 上午

* [ ] 数据库第六章作业
