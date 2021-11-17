- [paperswithcode](https://paperswithcode.com/task/human-object-interaction-detection)

# DATASETS

1. [**HICO-DET**](http://www-personal.umich.edu/~ywchao/hico/)
2. [**V-COCO**](https://github.com/s-gupta/v-coco)

# READ PAER

1. [Detecting and Recognizing Human-Object Interactions](https://arxiv.org/pdf/1704.07333v3.pdf)

   - 2018

   ![image-20210820161946773](.\imgs\image-20210820161946773.png)

   - 简介

     - 任务定义为Human-Object Interaction(HOI)，那么很明显，主体是人，那么将问题转化为在给定人物情况下，该人物与某个物体产生交互的可能性多高： $p(\langle H, O \rangle | H)$。加上人物与物体交互的动作，那么问题转化为：$p(\langle H, O \rangle | H, A)$
       - $H$表示人，$O$表示物，$A$表示行为
     
     - 将人物交互问题分解为四个元素——人、物、人与行为、人与行为对应的物的位置。整体的模型基于Faster R-CNN，分为三个分支：ObjDetBranch、HumanCentricBranch、InteractionBranch，通过这三个分支来得到当前图中人与对应物体的交互信息。
     
   - 详解

     ​        将HOI问题拆解为$\langle human, verb, object \rangle$，定义一个人是不是以某个动作与某个物体产生交互的计算公式如下：
     $$
     S_{h,o}^{a} = s_h \cdot s_o \cdot s_h^a \cdot g_{h,o}^a
     $$
     其中，$s_h$表示判定为人的概率；$s_o$表示判定为物体的概率；$s_h^a$表示判定人做该动作的概率；$g_{h,o}^a$表示该交互行为***下的物体确实是o的似然***，由${\mu}_h^a$（表示预测的交互的物体的位置）计算得到。

     - $s_h$和$s_o$由ObjDetBranc得到，其实就是普通的目标检测的分类score

     - $s_h^a$由HumanCentricBranch得到，对$h$进行进一步的分类，即动作分类

     - $\mu_h^a$由HumanCentricBranch得到，预测的目标存在的区域$x,y,w,h$，这里的坐标进行过编码:
       $$
       x=\frac{x_o - x_h}{w_h} \\
       y=\frac{y_o - y_h}{h_h} \\
       w=\log \frac{w_o}{w_h} \\
       h=\log \frac{h_o}{h_h} \\
       $$
       
     - $g_{h,o}^a=exp(||b_{o|h}-\mu_h^a||/2\sigma^2)$，其中$b_{o|h}$由Equ. (2)计算得到，在该论文中，$\sigma$在验证集上调参得到，取值0.3。
     
     ObjDetBranch，正负样本1：3,每张图最多采样64个框；HumanCentricBranch，每个人最多产生16个pair对；InteractionBranch只考虑正样本（人、物、行为正确匹配）
     
   - 
   
   
   
2. [Visual Semantic Role Labeling](https://arxiv.org/pdf/1505.04474.pdf)

   - 2015

3. [Learning to Detect Human-Object Interactions](https://arxiv.org/pdf/1702.05448.pdf)

   - 2018
   
4. [Scaling Human-Object Interaction Recognition through Zero-Shot Learning](http://vision.stanford.edu/pdf/shen2018wacv.pdf)

   - 2018

5. [Visual Compositional Learning for Human-Object Interaction Detection](https://arxiv.org/pdf/2007.12407.pdf)

   - 2020

6. [Affordance Transfer Learning for Human-Object Interaction Detection](https://arxiv.org/pdf/2104.02867v2.pdf)

   - 2021

