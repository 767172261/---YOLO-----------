![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d1e2097b5e0e472193c04dc0048c6d59.png)

### 📊 输电线路三维全景智能检测数据集 描述表

| **类别名称** | **类别ID** | **实例数量** | **描述** |
|--------------|------------|--------------|----------|
| Excavator-挖掘机 | 0 | 1000 | 挖掘机在施工现场常见，可能对输电线路造成破坏 |
| Loader-推土机 | 1 | 400 | 推土机在施工或清理场地时可能出现 |
| Dumb_truck-装载车/渣土车 | 2 | 800 | 装载车和渣土车在运输过程中可能靠近输电线路 |
| Mobile_crane-吊车 | 3 | 500 | 吊车在高空作业时可能触及输电线路 |
| hzyw-火灾烟雾 | 4 | 1068 | 火灾烟雾是山火的标志，对输电线路构成严重威胁 |

> 总图像数：**2768 张**  
> 标注格式：**YOLO**  
> 标注工具：**labelImg**

![---](https://i-blog.csdnimg.cn/direct/06c47d2b1d4f40878af174ed1db5ff20.png)


### 📈 数据集统计图表说明

#### 1. 类别实例数量分布（柱状图）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f84453d89a2a40029ee01aa1e5737cf4.png)


- **Excavator-挖掘机**：最多，共 1000 个实例。
- **hzyw-火灾烟雾**：次多，共 1068 个实例。
- **Dumb_truck-装载车/渣土车**：800 个实例。
- **Loader-推土机**：400 个实例。
- **Mobile_crane-吊车**：500 个实例。

#### 2. 边界框分布（重叠图）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/eaa61dcb368e4f6eb1e2d1f6d8000f76.png)


- 图中显示了不同类别的边界框在图像中的分布情况，颜色代表不同的类别。
- 可以观察到各类别在图像中的位置分布是否有重叠或聚集现象。

#### 3. 目标中心点分布（散点图）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ff49b9a2c6994edfbdcce0e5e1ffb11f.png)

- x 和 y 分别表示目标中心点在图像中的相对位置（归一化坐标）。
- 散点图展示了各类别目标在图像中的空间分布情况，有助于分析目标的位置偏好。

#### 4. 目标尺寸分布（散点图）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/048aa4007b1f4cd190b2b0150d930370.png)


- width 和 height 分别表示目标的宽度和高度（归一化值）。
- 散点图展示了各类别目标的尺寸分布情况，有助于分析目标的大小特征。

---

### 🛠️ 博主建议：数据集使用建议

1. **类别平衡**：虽然 `hzyw-火灾烟雾` 类别实例较多，但其他类别如 `Loader-推土机` 实例较少，建议通过数据增强（如翻转、旋转等）增加少数类别的样本量，避免模型偏向多数类别。

2. **标注质量检查**：使用 labelImg 工具进行人工复核，确保标注框准确无误，特别是对于边缘模糊的目标（如烟雾）。

3. **模型选择与训练**：
   - **模型选择**：YOLOv5/v7 等实时检测模型适合此类任务，可快速识别危险车辆和烟雾。
   - **训练策略**：采用迁移学习，利用预训练权重加速收敛；设置适当的学习率和批量大小，监控验证集上的 mAP 指标。

4. **后处理优化**：
   - 对于烟雾检测，可结合时间序列分析，判断连续帧中的烟雾变化趋势。
   - 对于车辆检测，可引入距离估计模块，评估车辆与输电线路的安全距离。

---
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3c1f96ab27f846e7a4bc5ac640e5a9fb.png)




面向电力系统安全防护的高价值目标检测任务，用于 **防外力破坏**，包含 **2768 张图像**，5 个关键类别，涵盖施工机械与火灾烟雾，使用 **YOLOv8** 进行高效训练与部署。以下文字及代码仅供参考学习使用。

---

## ✅ 一、数据集信息总结

| 项目 | 内容 |
|------|------|
| 图像数量 | 2768 张 |
| 标注格式 | YOLO 格式（`.txt`） |
| 类别数量 | 5 类 |
| 类别名称 | `Excavator-挖掘机`, `Loader-推土机`, `Dumb_truck-装载车/渣土车`, `Mobile_crane-吊车`, `hzyw-火灾烟雾` |
| 标注工具 | labelImg |
| 应用场景 | 输电线路安全监控、防外力破坏、山火预警 |

> 📌 注意：类别名中包含中文和英文，训练时建议使用英文或统一命名（如 `excavator`, `loader`, `dumb_truck`, `mobile_crane`, `smoke_fire`）

---

## ✅ 二、目录结构建议

```
power_line_dataset/
├── images/
│   ├── train/      # ~2214 张 (80%)
│   └── val/        # ~554 张 (20%)
├── labels/
│   ├── train/
│   └── val/
└── data.yaml       # 数据集配置文件
```

---

## ✅ 三、创建 `data.yaml` 配置文件

```yaml
# data.yaml
train: ./power_line_dataset/images/train
val: ./power_line_dataset/images/val

# 类别数量
nc: 5

# 类别名称（建议使用英文命名，与标签中的 class_id 对应）
names:
  - excavator         # Excavator-挖掘机
  - loader            # Loader-推土机
  - dumb_truck        # Dumb_truck-装载车/渣土车
  - mobile_crane      # Mobile_crane-吊车
  - smoke_fire        # hzyw-火灾烟雾
```

> ⚠️ **重要**：确保你的 `.txt` 标签文件中：
> - `class_id` 从 `0` 开始
> - 每行格式：`class_id x_center y_center w h`（归一化到 [0,1]）
> - 图像与 `.txt` 文件同名

---

## ✅ 四、环境准备（推荐 Anaconda）

```bash
# 创建虚拟环境
conda create -n power_line_det python=3.9
conda activate power_line_det

# 安装 PyTorch（根据你的 CUDA 版本）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# 安装 YOLOv8
pip install ultralytics opencv-python matplotlib tqdm

# 验证CUDA
python -c "import torch; print(torch.cuda.is_available())"
```

---

## ✅ 五、自动划分数据集脚本

```python
# split_dataset.py
import os
import random
import shutil
from pathlib import Path

def split_dataset(image_dir, label_dir, output_dir, train_ratio=0.8):
    image_dir = Path(image_dir)
    label_dir = Path(label_dir)
    output_dir = Path(output_dir)

    # 支持常见图像格式
    images = list(image_dir.glob("*.jpg")) + \
             list(image_dir.glob("*.png")) + \
             list(image_dir.glob("*.jpeg"))

    random.shuffle(images)

    train_size = int(len(images) * train_ratio)
    train_images = images[:train_size]
    val_images = images[train_size:]

    # 创建目录
    for folder in ['images/train', 'images/val', 'labels/train', 'labels/val']:
        (output_dir / folder).mkdir(parents=True, exist_ok=True)

    # 复制图像和标签
    for img in train_images:
        shutil.copy(img, output_dir / 'images' / 'train' / img.name)
        label = label_dir / f"{img.stem}.txt"
        if label.exists():
            shutil.copy(label, output_dir / 'labels' / 'train' / label.name)

    for img in val_images:
        shutil.copy(img, output_dir / 'images' / 'val' / img.name)
        label = label_dir / f"{img.stem}.txt"
        if label.exists():
            shutil.copy(label, output_dir / 'labels' / 'val' / label.name)

    print(f"✅ 输电线路数据集划分完成！")
    print(f"   训练集: {len(train_images)} 张")
    print(f"   验证集: {len(val_images)} 张")

# 使用
split_dataset(
    image_dir='raw/images',
    label_dir='raw/labels',
    output_dir='power_line_dataset'
)
```

---

## ✅ 六、YOLOv8 完整训练代码

```python
# train.py
from ultralytics import YOLO
import torch

# 设置设备
device = 'cuda' if torch.cuda.is_available() else 'cpu'
print(f"Using device: {device}")

# 加载预训练模型（推荐 yolov8m 或 yolov8l）
model = YOLO('yolov8m.pt')  # 可选: yolov8s.pt, yolov8l.pt, yolov8x.pt

# 开始训练
results = model.train(
    data='power_line_dataset/data.yaml',     # 数据配置文件
    imgsz=640,                               # 输入尺寸
    epochs=200,                              # 训练轮数（数据量不大，可多训）
    batch=32,                                # 批次大小（根据显存调整）
    name='yolov8m_power_line',
    project='runs/power_line',
    device=device,
    workers=8,                               # 数据加载线程
    optimizer='AdamW',                       # 更稳定优化器
    lr0=0.001,                               # 初始学习率
    momentum=0.937,
    weight_decay=0.0005,
    patience=40,                             # 早停：40轮无提升则停止
    save_period=10,                          # 每10轮保存一次
    exist_ok=True,
    augment=True,                            # 启用增强
    mosaic=1.0,                              # Mosaic增强
    mixup=0.1,                               # MixUp增强
    copy_paste=0.1,                          # Copy-Paste增强（对小目标有效）
    close_mosaic=30,                         # 最后30轮关闭Mosaic
    amp=True,                                # 混合精度训练
    val=True                                 # 每轮验证
)

print("✅ 输电线路智能检测模型训练完成！")
```

---

## ✅ 七、关键训练参数说明

| 参数 | 推荐值 | 说明 |
|------|--------|------|
| `imgsz` | `640` | 可尝试 `800` 提升小目标检测 |
| `batch` | `16~32` | 根据显存调整 |
| `epochs` | `150~200` | 数据量较小，建议多轮训练 |
| `optimizer` | `AdamW` | 比SGD更稳定 |
| `lr0` | `0.001` | 初始学习率 |
| `mosaic`, `mixup`, `copy_paste` | `1.0, 0.1, 0.1` | 增强泛化性 |
| `close_mosaic` | `30` | 后期关闭提升稳定性 |
| `amp` | `True` | 混合精度，节省显存 |

---

## ✅ 八、模型验证（评估性能）

```python
from ultralytics import YOLO

# 加载最佳模型
model = YOLO('runs/power_line/yolov8m_power_line/weights/best.pt')

# 验证
metrics = model.val(
    data='power_line_dataset/data.yaml',
    batch=32,
    imgsz=640,
    device='cuda',
    project='val_results',
    name='power_line_val',
    save_json=True
)

# 输出指标
print(f"mAP@0.5:       {metrics.box.map:.4f}")
print(f"mAP@0.5:0.95:  {metrics.box.map50_95:.4f}")
print(f"Precision:     {metrics.box.precision:.4f}")
print(f"Recall:        {metrics.box.recall:.4f}")
print(f"F1 Score:      {metrics.box.f1.mean():.4f}")
print(f"FPS:           {metrics.speed['inference']:.2f}")
```

---

## ✅ 九、推理代码（图像、视频、摄像头）

### 1. 单图推理

```python
results = model.predict(
    source='test_construction.jpg',
    conf=0.3,
    iou=0.5,
    show=True,
    save=True,
    project='inference_output',
    name='demo'
)
```

### 2. 视频或实时流

```python
# 视频文件
model.predict(source='drone_video.mp4', show=True, save=True)

# 实时摄像头
model.predict(source=0, show=True, conf=0.4)
```

### 3. 批量推理

```python
model.predict(
    source='power_line_dataset/images/val',
    save=True,
    project='batch_infer',
    name='val_results'
)
```

---

## ✅ 十、模型导出（用于部署）

```python
# 导出为 ONNX
model.export(format='onnx', dynamic=True, opset=13)

# 导出为 TensorRT（NVIDIA GPU）
# model.export(format='engine', device=0)

# 导出为 TorchScript
model.export(format='torchscript')
```

---

## ✅ 十一、可视化训练过程

- 查看 `runs/power_line/yolov8m_power_line/results.png` → loss 和 mAP 曲线
- 启动 TensorBoard：

```bash
tensorboard --logdir runs/power_line
```

---

## ✅ 十二、总结

| 步骤 | 内容 |
|------|------|
| 🧰 环境 | CUDA + Ultralytics YOLOv8 |
| 📁 数据 | YOLO 格式，`data.yaml` 配置 |
| 🏃‍♂️ 训练 | `model.train()` 高级调参 |
| 🔍 推理 | 图像、视频、摄像头支持 |
| 📊 评估 | mAP@0.5, mAP@0.5:0.95 |
| 🚀 部署 | ONNX / TensorRT 导出 |

---

📌 **应用场景扩展**：
- 无人机自动巡检
- 变电站安全监控
- 智慧电网防外破系统
- 山火早期预警平台

---

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ff34aa8988ee4b6ba81f2b086208896c.png)
1
以上文字及代码仅供参考学习使用。
