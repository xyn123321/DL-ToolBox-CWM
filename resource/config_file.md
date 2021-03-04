# Config file

Author: Weiming Chen

Email: wm_chen@yeah.net

## A standard config file

The config file must be '.py' format.

```python
# config/CIFAR-10/resnet/resnet18-pretrained_bs-64_e-100_lr-1e-2_lrconfig-step_loss-ce_SGD.py
import os
import torch.nn as nn
import torch.optim as optim
from torchvision import transforms
from dl_toolbox_cwm.dataset.classification import CIFAR10
from dl_toolbox_cwm.model.classification import ResNet18


# root path
root_path = '/home/weiming/DL-ToolBox-CWM'  # TODO: rewrite as your root path

# dataset settings
dataset_name = 'CIFAR10'

# Hyper-parameters
epoch = 100
batch_size = 64

# pipeline
train_pipeline = [
    transforms.ToTensor(),
    transforms.Resize(224),
    transforms.RandomHorizontalFlip(p=0.5),
    transforms.RandomVerticalFlip(p=0.5),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
]
val_pipeline = [
    transforms.ToTensor(),
    transforms.Resize((224, 224)),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
]
test_pipeline = [
    transforms.ToTensor(),
    transforms.Resize(224),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
]

# dataset
train_config = dict(
    data_prefix='data/CIFAR-10',
    ann_file=None,
    shuffle=True
)
val_config = dict(
    data_prefix='data/CIFAR-10',
    ann_file=None,
    shuffle=True
)
test_config = dict(
    data_prefix='data/CIFAR-10',
    ann_file=None,
    shuffle=False
)
train_set = CIFAR10(
    data_prefix=os.path.join(root_path, train_config['data_prefix']),
    pipeline=train_pipeline,
    ann_file=None if train_config['ann_file'] is None else os.path.join(root_path, train_config['ann_file']),
    is_train=True
)
val_set = CIFAR10(
    data_prefix=os.path.join(root_path, val_config['data_prefix']),
    pipeline=val_pipeline,
    ann_file=None if train_config['ann_file'] is None else os.path.join(root_path, val_config['ann_file']),
    is_train=False
)
test_set = CIFAR10(
    data_prefix=os.path.join(root_path, test_config['data_prefix']),
    pipeline=test_pipeline,
    ann_file=None if train_config['ann_file'] is None else os.path.join(root_path, test_config['ann_file']),
    is_train=False
)

# model
model = ResNet18(num_classes=10, pretrained='pretrained_model/ResNet/resnet18.pth')

# loss function
criterion = nn.CrossEntropyLoss()

# optimizer
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9, weight_decay=1e-4)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epoch*int(len(train_set)/batch_size))  # optional

# validate config (optional, default=1)
val_interval = 1

# checkpoint saving (optional, default=1)
checkpoint_interval = 1

# log config (optional, default=10)
log_interval = 10

```

### 1. All Variables

```python
all_variables = [
        'epoch', 'batch_size',
        'dataset_name', 'train_pipeline', 'test_pipeline',
        'train_config', 'val_config', 'test_config',
        'train_set', 'val_set', 'test_set',
        'model', 'criterion', 'optimizer', 'scheduler',
        'val_interval', 'checkpoint_interval', 'log_interval'
]
```

### 2. Necessary Variables

```python
necessary_variables = [
        'epoch', 'batch_size',
        'dataset_name', 'train_pipeline', 'test_pipeline',
        'train_config', 'val_config', 'test_config',
        'train_set', 'val_set', 'test_set',
        'model', 'criterion', 'optimizer'
]
```

## Contact

This repository is currently maintained by Weiming Chen.

If you have some other method need to be supported, contact me through Email: wm_chen@yeah.net