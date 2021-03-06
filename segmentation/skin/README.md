# 皮肤分割

这里的代码写了主要是用于皮肤分割，也适用于其他的二分类（医学图像分割）

## 兼容
python:3.6  
keras:2.2.4 
tensorflow-gpu:1.8.0  
torchvision:0.2.1

## 更新
| Date     | Update |
|----------|--------|
| 2019-02-27 | 第一次上传 |



## 数据
我的数据来源于：[LIP](http://sysu-hcp.net/lip/overview.php)，拿到的human parsing数据先试用人脸检测抠出人脸部分，做上半身的皮肤分割。
如果想训练你自己的数据，那么你的数据结构需要如下（文件夹格式）  

        DATA_DIR/
                |-- imgs/
                |    |-- xxx1.jpg
                |    |-- xxx2.jpg
                |    |-- ...
                |-- masks/
                |    |-- xxx1.png
                |    |-- xxx2.png
                |    |-- ...

## 训练
```
$ python train.py imgs主目录 masks主目录
```
训练之前会按照9:1自动划分训练和验证，并且会自动保存到txt文件夹内

## 文件介绍
- data_info和eval_iou都是使用了[ESPNet](https://github.com/sacmehta/ESPNet)下的代码，主要用于计算label权重和每次迭代末计算各种评测指标
- data_loader，数据生成器，能够自动读取图片，自动进行train/val划分，自动保存txt以备后续验证结果
- dataAug，用于测试
- utils，keras的一些回调函数放置处
- train，训练主函数

## 测试结果展示
<div align="center">
<img src="images/00056.jpg" width="256"/> <img src="images/00057.jpg" width="256"/>
<p> results of segmentation </p>
</div>


## 参考
  - [segmentation_models](https://github.com/qubvel/segmentation_models)：构建了几个分割模型库，方便直接调用，省去了手撸，添加了预训练权重（imagenet），适当的可以微调
  - [keras issue#11796](https://github.com/keras-team/keras/issues/11796)：keras训练过程保存model的一个问题，可能以后更新就不会有
  - [albumentations](https://github.com/albu/albumentations)：一个强大数据增强库，适用于分类，分割，检测



## 讨论
- 借用了qubvel搭建的分割库，构建现有的model，而不需要自己去手撸一个
- 我尝试的是皮肤分割，一个二分类问题，医学图像类似，一般它们都是使用dice loss，jaccard loss，该[segmentation_models](https://github.com/qubvel/segmentation_models)库中都有现成的，很方便
- keras的一个bug，涉及到hdf5 best模型覆盖保存问题，找到一个临时解决方法是保存之前先把原先的删掉，在utils文件内可以看到，这个可能以后升级就不会有了。

