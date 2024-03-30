---
title: "Write an Extension"
date: 2024-03-30T16:33:47+08:00
draft: false
weight: 10
---

Here is a brief walkthrough of how to create an extension. For more details about defining nodes, see [Define a Node Type](./define_a_node_type).

## Create an Extension

Create `extensions/grapycal_uwu.py`:

```python
# extensions/grapycal_uwu.py
from grapycal import Node

class TestNode(Node):
    category = 'test'
```

That's it! You have created an extension with a node type `TestNode` in the category `test`.

## Import the Extension

To use the extension, type `/import grapycal_uwu` in the GUI. This will import the extension and make the `TestNode` available in the node library. Then, type `Test` to summon a `TestNode`.

The node is empty because we haven't defined any ports and controls yet.

## Edit the Extension

Add some ports and logic to the `TestNode`:

```python
    from grapycal import Node, Edge, InputPort

    class TestNode(Node):
        category = 'test'
        def build_node(self):
            self.label.set('Test')
            self.add_in_port('number')
            self.out_port = self.add_out_port('isEven')

        def port_activated(self, port: InputPort):

            # Determine whether the input number is even
            result = port.get() % 2 == 0

            # Feed the result to edges connected to the output port
            self.out_port.push(result)
```

## Reload the Extension

To apply the changes, type `/reload grapycal_uwu` in the GUI. This will reload the extension and update the `TestNode` we created earlier.

Now, the `TestNode` has an input port `number` and an output port `isEven`. When a number is fed into the input port, the node will output `True` if the number is even and `False` otherwise.

![Image](https://i.imgur.com/nN4g9o1.png)

## Unimport the Extension

To unimport the extension from the workspace, there must be no nodes from the extension in the workspace. Delete the `TestNode` from the workspace and type `/unimport grapycal_uwu` in the GUI.