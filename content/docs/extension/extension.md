---
weight: 0
title: "Extension"
description: ""
icon: "article"
date: "2024-03-30T16:59:01+08:00"
lastmod: "2024-03-30T16:59:01+08:00"
draft: false
toc: true
---

An extension is a Python module that defines a set of nodes for a specific domain, such as image processing and deep learning. You can define [nodes](./define_a_node_type) and [commands](./define_a_command) in an extension.

An extension looks like this:

```python
# grapycal_builtin/__init__.py
 
class Node1(Node):
     ...
 
class Node2(Node):
     ...
```
