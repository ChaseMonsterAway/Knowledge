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

## CRAFT

## PANET

## DB

