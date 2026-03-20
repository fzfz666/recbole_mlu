# recbole_mlu
recbole适配mlu380，仅交流学习

# 🚀 RecBole-MLU: 寒武纪算力卡开箱即用版

> **简介**：本项目是针对 **寒武纪 MLU370** 算力卡深度适配的伯乐（RecBole）推荐系统框架。已完美解决底层算子映射、设备识别硬编码、依赖冲突等问题。无需修改代码，直接开箱即用！

## 📌 环境基线要求
- **硬件**: 寒武纪 MLU370 系列
- **OS**: Ubuntu / Linux
- **Python**: 3.10
- **PyTorch**: >=2.5.0 + `torch_mlu` 预装环境

## 🛠️ 极速安装指南

### 1. 解压源码包
```bash
tar -zxvf RecBole_MLU_Ready.tar.gz
cd RecBole_mlu
```

### 2. 配置加速源（极度重要）

国内服务器请务必开启阿里源，否则下载推荐系统巨大的依赖库会卡死：

```bash
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```

### 3. 安装依赖（🚨 避坑预警）

**绝对不要直接 `pip install -r requirements.txt`！** 这会导致自动下载官方版的 Torch 并覆盖掉你珍贵的寒武纪 `torch_mlu`。请严格执行以下步骤：

```bash
# ① 先安装除了 torch 之外的所有底层依赖包
cat requirements.txt | grep -v "torch" | xargs pip install

# ② 补齐部分缺失的可视化与日志包
pip install colorlog==4.7.2 plotly tabulate tensorboard texttable colorama==0.4.4

# ③ 以“不检查依赖”的模式，将本框架安装进系统
pip install -e . --no-deps
```

## 🏃 运行测试 (One-Line Run)

所有底层接口已由本项目接管。**请注意：命令行中的 `--use_gpu=True` 必须保留**，底层会自动将其映射为 MLU 算力卡计算。

```bash
# 跑通基线 BPR 模型 + ml-100k 数据集
python run_recbole.py --model=BPR --dataset=ml-100k --use_gpu=True --gpu_id=0
```


如果在控制台看到巨大的 RecBole 字符 Logo，且进度条飞速滚动，恭喜你，MLU 已全速运转！
