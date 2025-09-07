# PickCubeXtimes环境
```
mani_skill/envs/tasks/tabletop/pick_cube_xtimes_v2.py
```
重写了PickcubeEnv，为实现数据集的读取写入gym.make()新增以下参数
```
HistoryBench_record=True,#路径规划录制中设为true
HistoryBench_collect_seed=seed,#record=true生效 控制seed名称
HistoryBench_dataset=dataset_path,#控制读取写入的位置
HistoryBench_episode=episode#控制读取dataset中第几个episode
```
# 录制
```
mani_skill/examples/motionplanning/panda/solutions/solve_pick_cube_xtimes_v2.py
```
负责拿去环境路径规划，创建关闭环境

# 回放
```
mani_skill/examples/demo_replay_action_v2.py
```
按照seed重新创建环境 按照关节角回访每一step
