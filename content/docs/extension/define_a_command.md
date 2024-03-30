---
weight: 300
title: "Define a Command"
description: ""
icon: "article"
date: "2024-03-30T21:40:36+08:00"
lastmod: "2024-03-30T21:40:36+08:00"
draft: false
toc: true
---

A command is a function that can be [called from the GUI](#command-execution). Commands are used to automate tasks, such as create multiple nodes at once.

To define a command, create a method in an extension class and decorate it with `@command`. The method should accept a [`CommandCtx`](#commandctx) object as its argument.

For example:

```python
# grapycal_owo.py

from grapycal import Extension, command, CommandCtx, GRID
from grapycal_builtin import EvalNode, MultiplicationNode, PrintNode
import random

class GrapycalOwo(Extension):

    @command('Make multiplication problem')
    def make_multiplication_problem(self, ctx: CommandCtx):
        x, y = ctx.mouse_pos
        
        a = random.randint(1, 10)
        b = random.randint(1, 10)
    
        a_node = self.create_node(EvalNode, [x, y])
        b_node = self.create_node(EvalNode, [x, y + GRID*6])
        mul_node = self.create_node(MultiplicationNode, [x + GRID*5, y + GRID*3])
        print_node = self.create_node(PrintNode, [x  + GRID*10, y + GRID*3])
    
        a_node.expr_control.set(str(a))
        b_node.expr_control.set(str(b))
    
        self.create_edge(a_node.out_port, mul_node.get_in_port('items'))
        self.create_edge(b_node.out_port, mul_node.get_in_port('items'))
        self.create_edge(mul_node.get_out_port('product'), print_node.get_in_port(''))


```

Result: 

![Image](https://i.imgur.com/VPhyjFZ.png)

## CommandCtx

The `CommandCtx` object provides information about the command execution context. It has the following attributes:
- `mouse_pos`: The position of the mouse when the command was executed.
- `client_id`: The ID of the client that executed the command.
- `editor_id`: The ID of the editor where the command was executed.

## What Can Commands Do?

Commands can do anything that can be done in Python code. For example, they can create nodes, create edges, modify node attributes, or `rm -rf /`.

Use `self.create_node` to create a node. Use `self.create_edge` to create an edge between two ports.

## Command Execution

Commands can be executed in the GUI by typing `/command_name` in the command bar.