An English version is provided below the Chinese version.
ManiSkill 项目修改说明 (ManiSkill Project Modifications for HistoryBench)
本文档说明了为支持 HistoryBench 数据集的录制与回放，对 ManiSkill 项目所做的修改和相关脚本的使用方法。
1. 环境文件修改
为支持在路径规划过程中录制数据集，我们对主要的环境定义文件进行了修改。
文件路径: mani_skill/envs/tasks/tabletop/pick_cube_xtimes_v2.py
gym.make() 新增参数
在创建环境实例时，增加了以下几个参数来控制数据集的录制和读取：
HistoryBench_record=True: 当设置为 True 时，开启路径规划数据的录制功能。在进行数据回放或常规训练时，请设置为 False。
HistoryBench_collect_seed=seed: 此参数在 HistoryBench_record=True 时生效。用于指定当前录制数据所使用的随机种子（seed），生成的数据集文件将以此 seed 命名，便于追踪和复现。
HistoryBench_dataset=dataset_path: 用于指定数据集存放或读取的根目录路径。
HistoryBench_episode=episode: 用于在回放数据时，控制读取 dataset_path 目录中第几个 episode 的数据。
2. 路径规划与数据生成脚本
该脚本负责创建环境、运行路径规划算法，并在录制功能开启时，将规划出的轨迹数据写入指定的数据集路径中。
文件路径: mani_skill/examples/motionplanning/panda/solutions/solve_pick_cube_xtimes_v2.py
核心功能
创建 pick_cube_xtimes_v2 环境实例。
执行路径规划算法来解决任务。
当 HistoryBench_record 设置为 True 时，它会将规划出的完整轨迹（包括每一步的关节角度等）写入到指定的数据集路径中。
任务完成后关闭环境。
3. 数据回放脚本
此脚本用于验证和可视化已录制的数据集。它会通过回放精确的动作序列来复现原始的路径规划过程。
文件路径: mani_skill/examples/demo_replay_action_v2.py
核心功能
根据指定的 seed 和 episode 编号，重新创建一个与数据录制时完全一致的环境。
从数据集中读取对应 episode 的每一步（step）的关节角度数据。
在环境中按顺序执行这些关节角度，从而实现对原始路径规划过程的精确回放。
英文版 (English Version)
ManiSkill Project Modifications for HistoryBench
This document outlines the modifications made to the ManiSkill project to support the recording and replay of the HistoryBench dataset, along with instructions for using the relevant scripts.
1. Environment File Modifications
The primary environment definition file has been modified to support dataset recording during path planning.
File Path: mani_skill/envs/tasks/tabletop/pick_cube_xtimes_v2.py
New gym.make() Parameters
Several parameters have been added to the environment creation process to control dataset recording and playback:
HistoryBench_record=True: Set to True to enable the recording of path planning data. This should be set to False during data replay or standard training.
HistoryBench_collect_seed=seed: This parameter is active when HistoryBench_record is True. It specifies the random seed used for the current data recording session. The resulting dataset file will be named with this seed for easy tracking and reproducibility.
HistoryBench_dataset=dataset_path: Specifies the root directory for storing or reading the dataset.
HistoryBench_episode=episode: Used during data replay to specify which episode to load from the dataset_path directory.
2. Path Planning and Data Generation Script
This script is responsible for creating the environment, running the path planning algorithm, and writing the trajectory data to the specified dataset path when recording is enabled.
File Path: mani_skill/examples/motionplanning/panda/solutions/solve_pick_cube_xtimes_v2.py
Core Funtionality:
Creates an instance of the pick_cube_xtimes_v2 environment.
Executes a path planning algorithm to solve the task.
If HistoryBench_record is set to True, it writes the complete trajectory (including joint angles for each step) to the specified dataset path.
Closes the environment upon task completion.
3. Data Replay Script
This script is used to validate and visualize the recorded datasets by replaying the exact sequence of actions.
File Path: mani_skill/examples/demo_replay_action_v2.py
Core Funtionality:
Recreates an environment identical to the one used during data recording, based on the specified seed and episode number.
Reads the joint angle data for each step from the corresponding episode file in the dataset.
Executes these joint angles sequentially in the environment, enabling a precise replay of the original path planning process.
