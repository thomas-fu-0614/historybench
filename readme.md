ManiSkill 项目修改说明
本文档说明了为支持 HistoryBench 数据集的录制与回放，对 ManiSkill 项目所做的修改和相关脚本的使用方法。
1. 环境文件修改
文件路径: mani_skill/envs/tasks/tabletop/pick_cube_xtimes_v2.py
此文件是主要的环境定义文件。我们对其进行了修改，以支持在路径规划过程中录制数据集。
gym.make() 新增参数
在创建环境实例时，增加了以下几个参数来控制数据集的录制和读取：
HistoryBench_record=True: 当设置为 True 时，开启路径规划数据的录制功能。在进行数据回放或常规训练时，请设置为 False。
HistoryBench_collect_seed=seed: 此参数在 HistoryBench_record=True 时生效。用于指定当前录制数据所使用的随机种子（seed），生成的数据集文件将以此 seed 命名，便于追踪和复现。
HistoryBench_dataset=dataset_path: 用于指定数据集存放或读取的根目录路径。
HistoryBench_episode=episode: 用于在回放数据时，控制读取 dataset_path 目录中第几个 episode 的数据。
2. 路径规划与数据生成脚本
文件路径: mani_skill/examples/motionplanning/panda/solutions/solve_pick_cube_xtimes_v2.py
该脚本负责以下核心任务：
创建 pick_cube_xtimes_v2 环境实例。
执行路径规划算法来解决任务。
当 HistoryBench_record 设置为 True 时，它会将规划出的完整轨迹（包括每一步的关节角度等）写入到指定的数据集路径中。
任务完成后关闭环境。
3. 数据回放脚本
文件路径: mani_skill/examples/demo_replay_action_v2.py
此脚本用于验证和可视化已录制的数据集。
它会根据指定的 seed 和 episode 编号，重新创建一个与数据录制时完全一致的环境。
从数据集中读取对应 episode 的每一步（step）的关节角度数据。
在环境中按顺序执行这些关节角度，从而实现对原始路径规划过程的精确回放。
