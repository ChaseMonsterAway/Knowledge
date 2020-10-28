- data

  - crop: 
    - crop patch size=(513, 513)
      - 考虑pad的方式有两种，一种是valid，一种是same，其中valid是只使用有效的元素，same会pad数字保证尺寸。当使用same的方式时，我们的下采样在使用$$3\times3$$的kernel和$$(2,2)$$的stride时，输入尺寸为偶数时，会需要右pad0，导致feature map出现偏移的情况；而当输入尺寸为奇数时，左右都需要pad0，可以减缓这种偏移现象。**当输入尺寸为偶数时，各种下采样为k=2,s=2时，在输入大小为偶数时应该是没有这种情况的。但是还是得综合考虑，当k,s和输入尺寸变化时，可能造成的差异会非常大。**
      - 选取的尺寸大小是和数据集相关联的，**待验证：一般选取的尺寸恰好包含当前数据集图片的最大尺寸**
  - randomly scaling
    - 0.5, 0.75, 1.0, 1.25, 1.5, 1.75
      - 在VOC的数据集下，使用这些scale可以保证输出数据的尺寸为奇数
      - **换用其它数据集的时候这些比例可能需要进行相应的更换**
    - 多尺度形成更大规模的数据分布
  - randomly left-right flipping
    - p: don't know, maybe 1.
    - 增强泛化，帮助后续TTA

- model

  - **Two types**

    - cascaded model
    - ASPP model

  - multi-grid

    - keep the receptive field and keep the spatial size
    - examples
      - output_stride=16, Multi-grid=(1,2,4), the three convolutions will have $$rates=2\times(1,2,4)=> (2, 4, 8)$$ in the block4.

  - atrous spation pyramid pooling (ASPP) with image pooling

    - ASPP

      - 使用大小不一样的感受野可以提取到信息更加丰富的特征

      > it is effective to resample features at differnet scakes for accuracy and efficiently classifying regions of an arbitrary scale.
      >
      > ASPP with different  atrous rates effectively captures multi-scale information.

    - add another image pooling 

      - 扩张卷积在ratio过大的情况下会退化为$$k=1\times1$$的卷积

      > However, we discover that as the sampling rate becomes larger, the number of valid filter weights becomes smaller.

      - 为了避免上述问题，同时补充语义级别的信息，引入Image pooling

- experience in training phase

  - 在backbone以外的模块中使用bn，对于模型的训练帮助非常大
  - set bn `decay=0.9997`, 经验值还是什么其他的。。影响大不大
  - `output_stride=16, batch_size=16`, 在trainaug上训练30k iterations with `init lr=0.007`，freeze bn parameters, employ `output_stride=8`并且在官方的`PASCAL VOC 2012 trainval set`上继续训练30k iterations with `init lr=0.001` **how to do this， and it says that atrous convolution allows us to do this.**
  - upsample logits: 保证gt的尺寸，将输出的map直接上采样到原尺寸。`align_corners=True`保证对齐

  > downsampling the groundtruths removes the fine annotations resulting no back-propagation of details.

  - bootstraping for hard images,  duplicate those image contains hard classes

- optimizer

  - init learning rate: 0.007
  - learning rate scheduler
    - poly learning rate scheduler,  `set power=0.9`, $$(1 - \frac{iter}{max\_iter})^{power}$$
      - 考虑模型的收敛：大学习率保证模型的收敛，相对step的方式平滑降低学习率有利于网络的学习，停留在小学习率的时间较少可能有助于规避网络过拟合的风险
      - **根据数据集的大小种类，可能会发生变化，并不一定**
  - inference
    - 训练阶段output stride 16，测试阶段output stride 8，**一个TRICK，换个数据集不一定可行**
    - TTA
      - multi scales: 0.5, 0.75, 1.0, 1.25, 1.5, 1.75  & left-right flip, average probabilities from each scale and flipped images

- evaluation metric

  - mIoU
    - $$\frac{TP(t)}{TP(t)+FP(t)+FN(t)}$$
  - Dice score
    - $$\frac{2{X}\bigcap{Y}}{\mid{X}\mid+\mid{Y}\mid}$$

  

- 实验思路

  - 验证output stride对实验结果的影响，进而验证保留空间大小的必要性
  - 验证在使用相同output stride时不同backbone的性能表现及对比
  - 验证在使用相同output stride时增加cascade的block对实验性能的影响
  - 验证multi-grid的影响
  - 验证ASPP的实验性能

