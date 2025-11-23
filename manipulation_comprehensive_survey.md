# Manpulation综述

https://arxiv.org/pdf/2510.10903

## Background
1. hardware

2. Non-learning: interpretability and safety in well-defined settings
   * Interpolation-based planning: e.g cubic splines perform well in lightweight and straightforward scenes, however limited adaptability to dynamic/uncertain environments
   * Sampling-based panning:
   * Optimization-based planning: offline/online 

   Learning-based: flexible generalization in complex or uncertain environments
   * MDP base
   * RL:
   * IL: BC: without an explicitly specified reward function , replaced by supervised learning ,mimick expert actions
   * IRL: inspect expert actions to learn the reward function ,not to mimick directly
   * GAIL生成式对抗模仿学习:

3. robotics models
   * VM: 
   * LM:
   * VLMs:
   * VAM VLAM:

4. evaluation:
   * Evaluation Metrics:success rate task completion time and action frequency return
   * Model selection: evaluate the model every 𝑘 epochs and select the checkpoint with the highest success rate.

## simulators benchmarks datasets
1. Grasping datasets:
   * rectangle-based :using a 5-dimensional grasp rectangle representation, defined by the center position (𝑥, 𝑦), width and height, and the orientation angle relative to the horizontal axis.  
   * 6-DOF:(x,y,z)+(orientation)
2. Single-Embodiment Manipulation Simulators and Benchmarks：
   * Basic Manipulation Benchmarks:basic:pick-and-place, sorting, pushing, inserting, opening, closing, and pouring. 
   * Further: multi-step operations, incorporating language prompts to guide trajectory generation,etc
   * Dexterous Manipulation benchmarks:
   * Deformable Object Manipulation Benchmarks:like cloth,rope,fluids

4. Trajectory Datasets: 
   * structured collections of time-ordered data that capture the sequential states, actions, and sensory observations of an agent interacting with an environment.include:robot joint states, end-effector poses, control inputs, and multimodal observations (e.g., RGB images, depth maps, force-torque readings) 拥有从低保真度的远程操作数据到高质量的专家演示,许多高质量的数据集包括语义标签、任务定义和多模态观
察（table4 有许多数据集）

5. Embodied QA and affourdance datasets
   * EQA:面向基础模型的具身化回答，要求理解环境以回答关于环境的问题
   * Affordance Datasets:针对与对象的低级功能交互，如抓取或工具使用
   They focun on:1.Visual Perception 2., Spatial Reasoning机器人与其环境之间空间关系的推理。例如确定抓取器和目标物体之间的相对方向。 
   * Functional and Commonsense Reasoning:理解物体的特性[159]和功能用途[153]，例如，识别盘子的魔杖是用来清洁餐具的，或者一把刀应该用刀柄抓住
## Manipulation tasks
1. grasping
   * =grasp detection and grasp generation,The primary focus is on predicting the position and orientation of the gripper 
   * These tasks involve identifying feasible grasp configurations from sensor inputs such as images or point clouds, allowing robotic end-effectors to securely pick up objects. 
   * Non-Learning-based Grasp:
   * Rectangle-based Grasp:
   * 6-DoF Grasp.:
   * Language-driven Grasp:1. multimodal feature fusion 2.leverages existing grasp models to generate large numbers of grasp candidates, followed by ranking or scoring with LLMs or VLMs to select the most confident grasps 3.directly fine-tunes MLLMs on grasp foundation models with large-scale grasp datasets
3. Dexterous Manipulation
   * =the capability of robotic systems equipped with multi-fingered or anthropomorphic hands to achieve precise and coordinated object control through complex contact interactions. 
   * Non-Learning-based Methods
   * Learning-based Methods.
4. Soft Robotic Manipulation：
5. DOM:perceive and control non-rigid objects
6. Mobile Manipulation：
7. Quadrupedal Manipulation：
8. Humanoid Manipulation

## high-level planner
1. LLMs and multimodal LLMs (MLLMs) are increasingly used for task planning, code generation, and even motion planning
   * LLM-based Task Planning:
   * MLLM-based Task Planning:LLM only process textual information,while visual inputs are typically handlef by
3. Code generation:y introducing perception and control programming interfaces as prompts for an LLM, enabling the direct generation of executable code to govern robot behavior
5. Affordance as Planner:
   * Geometric Affordance.
   * Visual Affordance:directly learning interaction possibilities from 2D visual data, such as RGB or RGB-D images. Good:reducing the reliance on expert demonstrations. 
   * Semantic Affordance:how robots could acquire manipulation skills by associating semantic properties with observed human actions, like linking objects with trajectories
   * Multimodal Affordance:
6. 3D representation:
   * producing structured action proposals (e.g., 6-DoF grasps, relational rearrangements, or optimization costs)现在的发展趋势：1.editable,real-time Gaussian Splatting 2.implicit or descriptor fields that distill features from 2D foundation models into 3D for correspondence and language grounding.
   * Gaussian-splatting:
   * Physically Embodied GS couples 
   * particles (physics) to 3DGS (vision) for a real-time, visually-correctable world model

## low-level learning-based control
### learning strategy
Higher:While high-level planning determines what to do and in what order, such as task decomposition, skill sequencing, or goal reasoning, low-level control determines how to act by learning precise visuomotor mappings that enable robust manipulation in dynamic environments.
Divide to:1.input modeling(确定使用什么感知模态和编码方式) 2.latent learning（将感知转化为可迁移的嵌入表示）3.policy learning(将潜在表示解码为可执行动作)
1. Learning strategy:RL:
   * RL：By leveraging high-dimensional perceptual inputs (e.g., vision or proprioception) and reward signals as feedback, RL enables agents to learn control policies through trial-and-error interaction with the environment.
   * model-free (不依赖环境动力学显式模型，通过与环境交互直接学习):
   * RL in pre-training:QT-Opt：适用于连续动作空间 为基于视觉的机器人抓取任务提供可扩展的自监督强化学习框架，先学习一个Q函数，再用随机优化算法替代贪婪搜索法寻找最优动作
   * PTR：用大规模数据集上的离线强化学习预训练，使机器人能够仅凭少量新演示就快速适应未见过的环境和任务
   * V-PTR：利用互联网上海量的人类示范数据。通过训练价值函数提取稳健的、与操作相关的视觉表征，进而针对下游任务进行微调。除预训练策略与价值函数外，通过预训练奖励函数亦可加速新任务训练。
   * RL in fine-Tuning:
   * RL in VLA
   * Model-based Methods:
   * Imagination Trajectory Generation.
   * Planning.
   * Differentiable RL.
2. Learning strategy:IL:
  * 相比较RL避免了昂贵的奖励设计和广泛的环境交互 引入ResNet Transformer等深度结构 随后加入了大模型视觉 多模态预训练 LLM LVM
  * 有前景的研究方向包括：将视觉学习架构 (VLA) 模型和语言学习模型 (LLM) 与机器人技术相结合的基础模型驱动型智能学习 (IL)；基于行为演算和反事实推理的因果智能学习；跨任务和跨实例的泛化和迁移；通过认证和运行时监控实现安全可靠的部署；以及更高效的人机交互，以减少学习过程中对人类反馈的依赖
  * Imitation from action:
  * BC:根据专家状态-动作对，学习从状态到底层控制指令的直接映射
  * MP-based IL:
  * Search-based:
  * Optimization-based IL:
  * Reward-based IL.:recover an underlying reward or cost function that explains expert demonstrations
  * IRL(逆强化学习):寻找奖励或成本使专家轨迹接近最优，
  * AIL:goalGAIL:解决传统算法如BC在demonstrations中没有出现的state，agent的表现非常不如人意的问题
  * Latent Learning for IL：把高维感知输入（图像、力/力矩、关节位姿等）压缩到低维“潜在表示（latent）”，并用这些潜变量来做模仿学习，从而减少冗余、强调任务相关信息、提高多模态建模与几何一致性，最终改善泛化和效率。
  * Imitation from observation:只能获取专家演示的状态或视觉轨迹，无法获得相应的专家动作标签，必须从观察到的状态转换（例如视频、动作捕捉）中推断策略基于观察的奖励：
3. Learning with Auxiliary Tasks：
  * World models：核心思想：机器人自己学会“预测未来会发生什么”通过观察和动作去预测下一步状态 
  * Generative Visual WMs：学习“视频未来帧生成”
  * Image or Video Prediction
  * .Vision-Grounded Goal Extraction 包括Detection and Segmentation. 
  * Text-Grounded Goal Extraction
  * Contrastive Learning
  * Reconstruction:包括 masked modeling+ generative reconstruction paradigms掩码建模+生成式重建 核心思想：从部分输入中恢复或预测缺失或新的观测值，例如被掩码的像素、标记或三维表征
  * 2D masked reconstruction:
  * 3D reconstruction:还用到了点云、多视图图像等

### Input Modeling
1. VAM:2D vision as input
2. VLAM:
  * 2D vision as input
  * Non-LLM-based VLA :直接把视觉观察 + 文本指令 → 序列化建模 → 低层控制动作但对数据依赖大
  * LLM/VLM based VLA:能用较少的数据实现0-shot/less-shot的迁移   
3. Tactile-based Action Models
### latent learning
利用紧凑、结构化且可迁移的表征，从而连接感知和控制。它专注于发现能够捕捉任务相
关语义、动态和可供性信息的中间表征，进而提升泛化能力和样本效率
1. pretrained latent learning:通过大规模预训练学习通用的视觉或多模态表征
根据训练数据的来源，机器人表征大致可以分为三类：基于通用数据集（例如 ImageNet[923]）、以人类为中心的数据集（例如 Ego4D[924]）和机器人专用数据集（例如 BridgeV2[145]）训练的表征。
2. latent action learning

### policy learning
1. 早期：MLP-based-policy基于多层感知器
2. transformer-based policy  
3. DP:
  * 将动作生成重新表述为一个去噪过程，从而能够进行多模态轨迹采样，并在各种操作任务中展现出强大的泛化能力。具体而言，DP 首先将机器人视觉运动控制建模为一个去噪扩散过程，并通过随机朗之万动力学学习动作得分梯度，从而实现对多模态动作的鲁棒处理，并引入滚动时域控制、视觉条件反射和时间序列扩散变换器，以在机器人操作任务中取得显著的性能提升。
  * 为什么有优势：多模态动作：抓取、插入等任务经常有多种可行轨迹，扩散能采样得到多种候选并选择最优（或用聚合策略）。鲁棒性：迭代去噪过程可以抵抗输入噪声，并通过多次采样找到更稳健解。与 planning 的结合自然：可以在 latent/world model 中做回溯采样与评估，适合 receding-horizon / MPC 式控制。可结合视觉、触觉、3D 信息：条件化机制易于融合多模态输入。
  * 3DDP：把动作、轨迹的条件转为3D token/点云特征（DP3等）
## key bottlenecks
1. data collection and utilization
  * human teleoperation and demonstration
  * human-in-the-loop enhancement
  * synthetic and automatic data:主要思路和方法：利用LLM/VLM生成任务、演示、标注，利用LLM把高层任务拆成子任务，产生指令序列或者伪代码，用规划期或RL把子任务变成轨迹或动作序列用VLM对视频、演示自动生成自然语言描述或动作标签，把生成的语言、脚本再用作条件来训练
  * Crowdsourcing-based Data Collection
  * data utilization data selection/retrieval/augmentation/expansion/reweighting

2. generalization
  * environment generalization:操控策略的环境适应力 关键问题：sim-to-real-gap与跨场景、设备泛化
  * Sim2real real2sim2real generalization:i.simultaion-only training 在仿真里注入尽可能多的变化 期望训练出的策略能直接迁移到真实世界，不做现场微调ii. Real-world adaptation 先在仿真中训练，再用少量真实数据模仿自适应
  * task generalization:i.for long-horizon tasks;Ii.compositional generalization;Iii.few-shot learning;v.meta learning元学习


