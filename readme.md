修改了mani_skill/envs/tasks/tabletop/pick_cube_xtimes_v2.py
作为env文件和控制数据集的写入
gym.make(
>新增以下参数
HistoryBench_record=True,#路径规划录制中设为true
HistoryBench_collect_seed=seed,#record=true生效 控制seed名称
HistoryBench_dataset=dataset_path,#控制读取写入的位置
HistoryBench_episode=episode#控制读取dataset中第几个episode


使用mani_skill/examples/motionplanning/panda/solutions/solve_pick_cube_xtimes_v2.py数据
负责路径规划，创建关闭环境

使用mani_skill/examples/demo_replay_action_v2.py
按照seed重新创建环境 按照关节角回访每一step
