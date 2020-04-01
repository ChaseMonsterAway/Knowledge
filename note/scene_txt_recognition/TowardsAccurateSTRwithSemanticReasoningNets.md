# 方法特点

1. global semantic reasoning module (GSRM)
2. parallel visual attention module (PAM)
3. visual-semantic fusion decoder (VSFD)
4. Non-RNN based



# 奇怪的话

>First, it can only perceive very limited semantic context from each decoding time step, even no useful semantic information for the first decoding time step.

visual的feature经过LSTM的编码，一般认为是具有语义信息的，而并不应该像文中所说的样子；如果直接是visual feature进行decode，那么和文中所述应该是对的