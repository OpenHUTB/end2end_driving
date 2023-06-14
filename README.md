<div align="center">   
  
# 类脑驾驶
</div>



https://github.com/OpenDriveLab/UniAD/assets/48089846/bcf685e4-2471-450e-8b77-e028a46bd0f7


<br><br>

![teaser](sources/pipeline.png)

## 内容列表:
1. [亮点](#high)
2. [开始入门](#start)
   - [安装](docs/INSTALL.md)
   - [准备数据](docs/DATA_PREP.md)
   - [评估例子](docs/TRAIN_EVAL.md#example)
   - [GPU 要求](docs/TRAIN_EVAL.md#gpu)
   - [训练/评估](docs/TRAIN_EVAL.md)
3. [结果和预训练模型](#models)
4. [TODO List](#todos)
5. [License](#license)
6. [Citation](#citation)

## 亮点 <a name="high"></a>

- :oncoming_automobile: **类脑解剖对齐理念**: 类脑驾驶是一个遵循类脑解剖对齐理念的统一自动驾驶算法框架。 不是独立的模块化设计和多任务学习，而是分层次地投放一系列任务，包括感知、预测和规划任务。


## 开始入门 <a name="start"></a>
- [安装](docs/INSTALL.md)
- [准备数据](docs/DATA_PREP.md)
- [评估例子](docs/TRAIN_EVAL.md#example)
- [GPU 要求](docs/TRAIN_EVAL.md#gpu)
- [训练/评估](docs/TRAIN_EVAL.md)

## 结果和预训练模型 <a name="models"></a>
UniAD 分两个阶段进行训练。 将发布两个阶段的预训练检查点，每个模型的结果如下表所示。

### 阶段一: 感知训练
> 我们首先训练感知模块（即跟踪和地图）以获得下一阶段的稳定权重初始化。 BEV 特征聚合为 5 帧 (queue_length = 5).

|   方法    | 编码器  | Tracking<br>AMOTA | Mapping<br>IoU-lane | config |                                                  下载                                                  |
|:-------:|:----:| :---: | :---: | :---:|:----------------------------------------------------------------------------------------------------:| 
| UniAD-B | R101 | 0.390 | 0.297 |  [base-stage1](projects/configs/stage1_track_map/base_track_map.py) | [base-stage1](https://github.com/OpenDriveLab/UniAD/releases/download/v1.0/uniad_base_track_map.pth) |



### 阶段二: 端到端训练
> We optimize all task modules together, including track, map, motion, occupancy and planning. BEV features are aggregated with 3 frames (queue_length = 3).

<!-- 
Pre-trained models and results under main metrics are provided below. We refer you to the [paper](https://arxiv.org/abs/2212.10156) for more details. -->

| Method | Encoder | Tracking<br>AMOTA | Mapping<br>IoU-lane | Motion<br>minADE |Occupancy<br>IoU-n. | Planning<br>avg.Col. | config | Download |
| :---: | :---: | :---: | :---: | :---:|:---:| :---: | :---: | :---: |
| UniAD-B | R101 | 0.358 | 0.317 | 0.709 | 64.1 | 0.25 |  [base-stage2](projects/configs/stage2_e2e/base_e2e.py) | [base-stage2](https://github.com/OpenDriveLab/UniAD/releases/download/v1.0/uniad_base_e2e.pth) |

### Checkpoint Usage
* Download the checkpoints you need into `UniAD/ckpts/` directory.
* You can evaluate these checkpoints to reproduce the results, following the `evaluation` section in [TRAIN_EVAL.md](docs/TRAIN_EVAL.md).
* You can also initialize your own model with the provided weights. Change the `load_from` field to `path/of/ckpt` in the config and follow the `train` section in [TRAIN_EVAL.md](docs/TRAIN_EVAL.md) to start training.


### Model Structure
The overall pipeline of UniAD is controlled by [uniad_e2e.py](projects/mmdet3d_plugin/uniad/detectors/uniad_e2e.py) which coordinates all the task modules in `UniAD/projects/mmdet3d_plugin/uniad/dense_heads`. If you are interested in the implementation of a specific task module, please refer to its corresponding file, e.g., [motion_head](projects/mmdet3d_plugin/uniad/dense_heads/motion_head.py).

## TODO List <a name="todos"></a>
- [ ] Upgrade the implementation of MapFormer from Panoptic SegFormer to [TopoNet](https://github.com/OpenDriveLab/TopoNet), which features the vectorized map representations and topology reasoning.
- [ ] Support larger batch size [Est. 2023/04]
- [ ] [Long-term] Improve flexibility for future extensions
- [ ] All configs & checkpoints
- [x] Visualization codes 
- [x] Separating BEV encoder and tracking module
- [x] Base-model configs & checkpoints
- [x] Code initialization
- [x] Fix bug: Unable to reproduce the results of stage1 track-map model when training from scratch. [Ref: https://github.com/OpenDriveLab/UniAD/issues/21]


## License <a name="license"></a>

All assets and code are under the [Apache 2.0 license](./LICENSE) unless specified otherwise.

## 参考论文 <a name="citation"></a>

该研究基于以下论文的成果：

```bibtex
@inproceedings{hu2023_uniad,
 title={Planning-oriented Autonomous Driving}, 
 author={Yihan Hu and Jiazhi Yang and Li Chen and Keyu Li and Chonghao Sima and Xizhou Zhu and Siqi Chai and Senyao Du and Tianwei Lin and Wenhai Wang and Lewei Lu and Xiaosong Jia and Qiang Liu and Jifeng Dai and Yu Qiao and Hongyang Li},
 booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
 year={2023},
}
```
## 相关资源

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
- [UniAD](https://github.com/OpenDriveLab/UniAD)
- [BEVFormer](https://github.com/fundamentalvision/BEVFormer)
- [ST-P3](https://github.com/OpenPerceptionX/ST-P3) 
- [FIERY](https://github.com/wayveai/fiery)
- [MOTR](https://github.com/megvii-research/MOTR)

