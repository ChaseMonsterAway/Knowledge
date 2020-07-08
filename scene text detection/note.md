# 结构整理

1. PSENet

   - Data
     - input: (image, bboxes)
       - bboxes -> relative bboxes (0, 1)
       - image -> random scale
         - keep ratio
         - size of scaled image > min_size
           - if not, scale = (min_size + 10)  / min(h, w)
         - scale params: [0.5, 1.0, 2.0, 3.0]
       - make text map and mask map based on bboxes
       - make kernel map 
         - kernel_nums: 7
         - rate = 1 - (1 - min_scale) / (kernel_nums - 1) * i 
         - d = (area - (1- rate**2))/perimeter
       - random horizontal
       - random rotate
       - random crop
       - ColorJitter
       - Normalize
     - output:
       - image
       - text map
       - mask map
       - kernel maps
   - training
     - model
       - resnet + fpn
     - loss
       - dice loss
       - loss text
         - ohem: p/n=1/3
           - select mask map
         - dice loss(pred text, text map, mask map)
       - loss kernel
         - kernel mask map = mask map * select mask map
         - dice loss(kernel_i, gt_kernel map i, kernel mask map)
       - loss
         - ic15: 0.7 * loss text + 0.3 * loss kernel

2. DB

   - Data

     - input args: (image, bboxes)

       - flip: p=0.5

       - rotation: [-10, 10]

       - resize:  [0.5, 3]

         - keep ratio: False

       - random crop

         - 640*640

       - make mask map

         - shrink

       - make boarder map

       - normalize

         - ```python
           RGB_MEAN = np.array([122.67891434, 116.66876762, 104.00698793])
           image -= RGB_MEAN
           image /= 255.0
           ```

     - output

       - image, mask map, boarder map,

   - training

     - model
       - backbone
         - resnet 
       - enhance
         - fpn
           - fusion: concat
       - head
         - init text map
         - threshold map
         - binary map
     - loss
       - init text map: ls
         - BCE
       - threshold map: lt
         - L1
       - binary map: lb
         - BCE

   - postprocess

     - get text map -> find contours -> selected based on area or other constrains
     - unclip
       - unclip_ratio = 1.5



# 伪代码结构

- DATA

  - dataloader

    ```python
    for batch in dataloader:
        model_input = batch['image']
        target: list = batch['tgt']
    ```

    

  - dataset

    ```python
    class TestDataset:
        def __init__(self, img_txt, img_root, gt_root, transforms=None):
            super(TestDatset, self).__init__()
            assert transforms is not None
            self.transforms = transforms
            self.img_txt = img_txt
            self.img_root = img_root
            self.gt_root = gt_root
            self.item_lists = self.get_needed_item()
        
        def get_needed_item(self):
            need_items = []
            with open(self.img_root, 'r') as f:
                for line in f.readlines():
                    line = line.strip()
                    poly_list, tag_list = self.load_ann(line)
                    need_tuple = (os.path.join(self.img_root, line), poly_list, tag_list)
                    need_items.append(need_tuple)
                    
            return need_items
        
        def load_ann(self, name):
            poly_list = []
            tag_list = []
            with open(os.path.join(self.gt_root, name), 'r') as f:
                for line in f.readlines():
                    line = line.split(',')
                    line = list(map(int, line))
                    poly_list.append(line[:-1])
                    tag_list.append(line[-1])
                    
            return poly_list, tag_list
                
        def __getitem__(self, index):
            im_path, polys, tags = self.item_lists[index]
            image = cv2.imread(im_path)
            if self.transforms:
                result = self.transforms(image, polys, tags)
            
            return result
        
        def __len__(self):
            
            return len(self.item_lists)
            
    ```

    

  - transforms

- MODEL

  - backbone
  - enhance module
  - head

- LOSS

- METRIC

