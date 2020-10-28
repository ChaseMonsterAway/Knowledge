## PixelLink

### rotation

- p: 0.5

- 90, 180, 270

### expand image

- config中默认为1，所以并未使用这个操作

### crop

- 保证crop出来的区域至少要覆盖n个目标
- 如果gt bbox
- 如果没有gt bbox
  - 随机产生一个crop区域，区域大小设置为box区域，类别为背景类
- crop出区域的aspect ratio (0.5, 2), area range (0.1, 1)

### resize

- 将crop出来的图片直接resize到固定尺寸

### Filter

- 将bbox短边在（m, n）之外的置为ignore, m=16, n





## Text field

### random crop

- aspect ratio: (0.3, 3)
- area ratios: (0.1, 1)
- 保证crop出来的区域包含的gt的面积> random(0.1， 0.3， 0.5， 0.7）*sum(gt.area)

### random rotate

- $$0, \pm90$$

### resize

- $$384*384 or 768*768$$





## CRAFT

- don't mentioned



## PANET

### random scale

- scale: [0.5, 1.0, 2.0, 3.0]

### random horizontal flip

- p: 0.5

### random rotation

- p: 0.5
- degree: (-10, 10)

### 保证短边大于等于input size（为了后面crop）

### random crop

- 存在文本框
  - 放的代码包含两个功能
    - 保证裁剪出来的图片中包含文本框
    - 随机裁剪
  - 写的代码里面使用下面的随机裁剪覆盖了上面的那一段，就很搞笑
- 不存在文本框
  - 随便crop



## PSENET

### random scale

- scale: (0.5, 1.0, 2.0, 3.0)

### random horizontal flip

- p: 0.5

### random rotate

- degree: [-10, 10]

### random crop

- 就是随机裁剪，只是有可能缩小了随机裁剪的范围

### color jitter

- brightness: 32/255
- saturation: 0.5



## DB

