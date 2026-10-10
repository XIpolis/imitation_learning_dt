# imitation_learning_lerobot

# 环境复现说明

## 一、前置要求

- 已安装 Miniconda
- 系统为 Ubuntu 22.04（其他 Linux 发行版可能需调整）
- 如需 GPU 支持，需安装 NVIDIA 驱动

## 二、创建环境

conda env create -f environment.yml
conda activate lerobot

注意：如果 PyTorch 相关包安装失败，见下方「常见问题」。

## 三、安装项目源码

在项目根目录执行：

pip install -e ./lerobot

## 四、常见问题

### 1. PyTorch 装不上（No matching distribution found）

+cu118 后缀的包只在 PyTorch 官方源，执行：

pip install torch==2.7.1+cu118 torchvision==0.22.1+cu118 torchaudio==2.7.1+cu118 \
  --index-url https://download.pytorch.org/whl/cu118

### 2. 如果系统里装了 ROS 2

ROS 2 自带的 pinocchio 会和 conda 环境的冲突。运行前执行：

export PYTHONPATH=/你的miniconda路径/envs/lerobot/lib/python3.10/site-packages:$PYTHONPATH

或把它写入 conda 环境的 activate.d 脚本：

mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'export PYTHONPATH=$CONDA_PREFIX/lib/python3.10/site-packages:$PYTHONPATH' \
  > $CONDA_PREFIX/etc/conda/activate.d/set_pythonpath.sh

### 3. 验证环境

python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
python -c "import numpy; print(numpy.__version__)"  # 应为 2.2.6
python -c "from my_code.envs import EnvFactory; print('环境加载成功')"