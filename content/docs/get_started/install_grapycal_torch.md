---
weight: 50
title: "Install grapycal_torch"
description: ""
icon: "article"
date: "2024-03-31T10:35:55+08:00"
lastmod: "2024-03-31T10:35:55+08:00"
draft: false
toc: true
---

If you want to use Grapycal with PyTorch, you need to install the grapycal_torch extension.

1. If you have GPU, install PyTorch with CUDA support [here](https://pytorch.org/get-started/locally/).

1. Install grapycal_torch as an editable package:

    ```bash
    pip install -e extensions/grapycal_torch
    ```