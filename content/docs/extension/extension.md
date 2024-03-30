---
weight: 10
title: "Extension"
description: ""
icon: "article"
date: "2024-03-30T16:59:01+08:00"
lastmod: "2024-03-30T16:59:01+08:00"
draft: false
toc: true
---

An extension is a Python module used to define [nodes](./define_a_node_type) and [commands](./define_a_command).

Each extension makes Grapycal avaliable in a specific domain, such as image processing and deep learning.

## Format

### Simple Extension Format

The simplest way to make an extension is to [define nodes](./define_a_node_type) in a Python module named `grapycal_<name>`.

```python
# grapycal_uwu.py
from grapycal import Node

class Node1(Node):
     ...

class Node2(Node):
     ...
```

When the extension is improted, Grapycal will automatically detect the node types defined in the module and make them available in the node library.

### Standard Extension Format

The standard way is to define an extension class that inherits from `Extension` and lists the node types it contains. The advantage of this approach is that it 

- explicitly defines which node types are included in the extension
- allows you to [define commands](./define_a_command)

```python
# grapycal_owo.py

class Node1(Node):
     ...
 
class Node2(Node):
     ...

class GrapycalOwo(Extension):
     node_types = [
          Node1,
          Node2,
     ]

     @command('do_something')
     def do_something(self, ctx: CommandCtx):
          ...

```

Next, follow this guide to start [Developing an Extension](./develop_an_extension)