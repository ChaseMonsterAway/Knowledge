# 方法特点

1. 使用一个较小的网络进行视觉特征提取
2. adaptive 2d positional encoding
   - to do ...........
3. point-wise feed forward -> locality-aware feedforward
   - local & global

# 总结

- data
  - MJ + ST
  - word level annotation
  - augmentation:
    - image size: 32*100
    - rotation : normal distribution N(0, (34^2))
  - train: case sensitive    test: case insensitive
  - batch size: 256
- model
  - backbone
    - two layers convolution (1/4 size)
  - encode/decode
    - modified transformer
    - encoder: point-wise feedfoward layer -> locality-aware feedforward
    - N_e = 12, N_d = 6
- loss
  - 
- training
  - Adam, init_lr: 3e-4
  - learning rate scheduler:  cyclic learning rate, step=250000
  - max_epochs: 4
- positional encoding感觉挺有意思的；场景文本识别不只是需要全局信息，局部的信息也很重要，采用conv的形式是有道理的。

