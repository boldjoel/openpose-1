# OpenPose using TensorFlow and TensorLayer

</a>
<p align="center">
    <img src="https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/media/dance_foot.gif?raw=true", width="360">
</p>


## 1. Motivation

[OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose) from CMU provides real-time 2D pose estimation following ["Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields"](https://arxiv.org/pdf/1611.08050.pdf) However, the [training code](https://github.com/ZheC/Realtime_Multi-Person_Pose_Estimation) is based on Caffe and C++, which is hard to be customized.
While in practice, developers need to customize their training set, data augmentation methods according to their requirement.
For this reason, we reimplemented this project in [TensorLayer fashion](https://github.com/tensorlayer/tensorlayer).

🚀 This repo will be moved into example folder of [tensorlayer](https://github.com/tensorlayer/tensorlayer) for life-cycle management soon. More cool Computer Vision applications such as super resolution and style transfer can be found in this [organization](https://github.com/tensorlayer).

## 2. Project files

- `config.py` : to config the directories of dataset, training details and etc.
- `inference.py`: TODO
- `models.py`: to define the model structures, currently only VGG19 Based model included
- `train.py`: includes try training mode, datasetapi(recommonded), placeholder(slower, used for debugging), distributed (TODO)
- `utils.py`: to extract databased from cocodataset and groundtruth calculation
- <s>`visualize.py`: draw the training result</s> (moved to utils.py)
- `inference` folder:

## 3. Preparation

Build C++ library for post processing. See: <https://github.com/ildoonet/tf-pose-estimation/tree/master/tf_pose/pafprocess>

```bash
cd inference/pafprocess
make

# ** before recompiling **
rm -rf build
rm *.so
```

## 4. Use pre-trained model

In this project, input images are RGB with 0~1.
Runs `train.py`, it will automatically download the default VGG19-based model from [here](https://github.com/tensorlayer/pretrained-models), and use it for inferencing.
The performance of pre-trained model is as follow:

<!--
|                  | Speed | AP | xxx |
|------------------|-------|----|-----|
| VGG19            | xx    | xx | xx  |
| Residual Squeeze | xx    | xx | xx  |

- Speed is tested on XXX
-->
- We follow the [data format of official OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/output.md)

## 5. Train a model

For your own training, please put .jpg files into coco_dataset/images/ and put .json into coco_dataset/annotations/

Runs `train.py`, it will automatically download MSCOCO 2017 dataset into `dataset/coco17`.
The default model in `models.py` is based on VGG19, which is the same with the original paper.
If you want to customize the model, simply change it in `models.py`.
And then `train.py` will train the model to the end.

## 6. Evaluate a model

Runs `eval.py` for inference

## 7. Speed up and deployment

For TensorRT float16 (half-float) inferencing, xxx

## 8. Customization

- Model : change `models.py`.
- Data augmentation : ....
- Train with your own data: ....
    1. prepare your data following MSCOCO format, you need to ...
    2. concatenate the list of your own data JSON into ...
- Evaluate on your own testing set:
    1. xx

## 9. Discussion

- [TensorLayer Issues 434](https://github.com/tensorlayer/tensorlayer/issues/434)
- [TensorLayer Issues 416](https://github.com/tensorlayer/tensorlayer/issues/416)

## Paper's Model

- [Default MPII](https://github.com/ZheC/Realtime_Multi-Person_Pose_Estimation/blob/master/model/_trained_MPI/pose_deploy.prototxt)
- [Default COCO model](https://github.com/ZheC/Realtime_Multi-Person_Pose_Estimation/blob/master/model/_trained_COCO/pose_deploy.prototxt)
- [Visualizing Caffe model](http://ethereon.github.io/netscope/#/editor)

## License

- This project is for academic use only.
