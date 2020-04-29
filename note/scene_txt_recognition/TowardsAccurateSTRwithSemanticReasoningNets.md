# 方法特点

1. parallel visual attention module (PVAM)
   - backbone输出的feature的每一个空间位置看作(k, v)对
   - 使用预先定义的order的信息作为q
   - 就是传统的（q | k,v）进行特征编码（attention）
2. global semantic reasoning module (GSRM)
   - 上述编码过的特征会先经过一个分类器进行text的预测，其预测值则可以认为是一个估计的text，使用一个Transfomer的单元进行预测
3. visual-semantic fusion decoder (VSFD)
   - 将visual feature和semantic feature结合，学习一个权重，使用该权重平衡这两个特征
   - 使用fusion feature进行预测
4. Non-RNN based



# 总结

- data
  - MJ+ST
  - word level annotation
  - augmentation
    - image size: 64*256
    - random resize the width (64, 128, 192, 256), then pad them to64*256
    - rotation
    - perspective distortion
    - motion blur
    - gaussian noise
  - max length: 25
  - class: 0-9, a-z, 'EOS' 共37类
  - batch size: 256
- model
  - backbone: resnet50(pretrained)+fpn
  - transformer unit (stacked 2)
  - PVAM
  - GSRM
  - VSFD
- loss
  - CE
- training
  - warming-up
  - train SRN 3epoch， then joint training
  - ADAM, 1e-4
- 感觉起作用最大的部分是后面的GSRM，将根据visual feature识别错的拉回来了，跟词分布有关。观察文中所给的good case和bad case，发现bad case中的词是在训练集中未出现过的，而good case中的词在训练集中是出现过的



# 奇怪的话

>First, it can only perceive very limited semantic context from each decoding time step, even no useful semantic information for the first decoding time step.

visual的feature经过LSTM的编码，一般认为是具有语义信息的，而并不应该像文中所说的样子；如果直接是visual feature进行decode，那么文中所述应该是对的