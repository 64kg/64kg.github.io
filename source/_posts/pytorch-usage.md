---
title: Pytorch usage
date: 2022-03-25 15:55:16
categories:
- Tech
tags:
- ML
- Python
- Software
---

Usage of PyTorch

<!-- more -->

# Installation

Follow the instruction on [official website](https://pytorch.org/get-started/locally/).

In my case, the command is
```sh
mkdir pytorch && cd pytorch
pipenv install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
```

It downloads ~2.6GB of resources.