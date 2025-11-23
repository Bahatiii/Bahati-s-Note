# RoboSimGS
## 核心问题：
机器学习受限于真实世界数据采集成本高、且模拟与真实之间存在显著的视觉与物理差距尤其是视觉效果和物理互动的缺失，导致在模拟中训练的策略难以迁移到现实。论文提出从真实场景的多视角图像出发，自动生成既有视觉效果又有物理互动仿真环境，从而实现对真实操控任务的zero-shot sim-to real。

## 怎么解决的/方法独特性：
1. 混合场景表示
3DGS重建场景的静态背景以获得高视觉保证，并把需要交互的物体用显式网格（mesh primitives）建成物理可模拟的对象
2. 用MLLM多模态大模型自动推断物理属性和结构
把多模态大语言模型用于从多视图视觉信息自动推测对象的物理参数（密度、刚度）和运动学结构（铰链、滑轨），从而自动生成“可活动”的关节构件

Pipeline:
![alt text](image.png)

## 代码库：
```
Articulation/
DataGeneration/
Gaussians/
assets/
imgs/
third_party/
utils/
requirements.txt
```

Scene reconstruction:
背景重构：用Nerfstudio + 3D Gaussian Splatting 重建背景场景（将场景拍摄多视角图像然后通过 3DGS 得到背景模型）
物体重构：对交互物（可抓取／可操作物体）进行扫描并生成高质量 mesh，并进一步用MLLM自动推断物体的铰接结构、物理参数

Simulated data generation:
坐标对齐：将3DGS真实背景与模拟环境对齐，仓库中icp.py提供了基于ICP的点云注册流程
相机位姿调整：

