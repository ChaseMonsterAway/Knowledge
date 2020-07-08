# Blogs

1. [google AI Blog](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)

2. [tensorflow tutorial](https://www.tensorflow.org/tutorials/text/transformer?hl=zh_cn)

3. [The illustration of transformer](http://jalammar.github.io/illustrated-transformer/)

   

# Paper

1. [Attention is All You Need](https://arxiv.org/abs/1706.03762)




# Structure

**Transformer**

- Embedding & Positional Encoding

  - ***embedding***
    - initial vector
  - ***positional encoding***
    - give position information

- Encoder

  - block

    - ***Multi-Head-Attention***
      - **input_args** : Q, K, V, mask
      - $mask = mask * 1e-9$  if mask is not None else $mask = mask * 0$
      - attention_map = $(mask+=(Q*K.T)).softmax(dims=-1)$
      - **return** : attentioned_v = attention_map * V
    - residual structure & layer norm
    - ***Point-wise-Feed-Forward***
      - **input_args** : X
      - $[nn.Linear(), nn.ReLU(), nn.Linear]$
    - residual structure & layer norm

    

- Decoder
  - block
    - ***Target-Multi-Head-Attention***
      - **input args** : t_Q, t_K, t_V, lookahead_mask
      - $lookahead\_mask = lookahead\_mask * 1e-9$  if lookahead_mask is not None else $lookahead\_mask = lookahead\_mask * 0$
      - attention_map = $(mask+=(t\_Q*t\_K.T)).softmax(dims=-1)$
      - **return:** attention_v = attention_map * t_V
    - residual structure & layer norm
    - ***Target-Src-Multi-Head-Attention***
      - **input_args:** Q ( attention_v ),  K(encoder out), V(encoder out), pad_mask
      - $pad\_mask = pad\_mask * 1e-9$  if pad_mask is not None else $pad\_mask = pad\_mask * 0$
      - attention_map = $(pad\_mask+=(Q*K.T)).softmax(dims=-1)$
      - **return** : attentioned_v = attention_map * V
    - residual structure & layer norm
    - Point-wise-Feed-Forward
      - **input_args** : X
      - $[nn.Linear(), nn.ReLU(), nn.Linear]$
    - residual structure & layer norm
- Output
  - **fully-connected layer**
    - **input args:**  X (batchsize * target_length * channel)
    - **output: ** X (batchsize * target length * num_classes+1)

