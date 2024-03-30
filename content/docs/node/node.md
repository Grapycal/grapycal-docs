---
weight: 999
title: "Node"
description: ""
icon: "article"
date: "2024-03-31T00:29:17+08:00"
lastmod: "2024-03-31T00:29:17+08:00"
draft: false
toc: true
---

## Inside a Node

In a Node, there are ports, controls, and attributes. Ports are used to connect nodes together, controls are used to interact with the user, and attributes are used to store data.

### Ports

Ports are used to connect nodes together. There are two types of ports: input ports and output ports. Input ports receive data from other nodes, while output ports send data to other nodes.

To create a port, call `add_in_port()` or `add_out_port()` in the `build_node()` method. For example:

```python
class AddNode(Node):
    def build_node(self):
        self.a = self.add_in_port('a')
        self.b = self.add_in_port('b')
        self.sum = self.add_out_port('sum')
```

### Controls

Controls are used to interact with the user. There are different types of controls:

- TextControl: A text field that displays text or allows the user to input text.
- ButtonControl: A button that triggers an action when clicked.
- ImageControl: An image that is displayed in the node or allows the user to upload an image from keyboard.
- OptionsControl: A dropdown menu that allows the user to select an option.
- LinePlotControl: A line plot that displays data as a line chart. Made with 3.js. Very hard to write. QWQ

To create a control, call `add_text_control()`, `add_button_control()`, `add_image_control()`, `add_options_control()`, or `add_line_plot_control()` in the `build_node()` method. For example:

```python
class CounterNode(Node):
    def build_node(self):
        self.add_text_control(name='count', text='0', readonly=True)
        self.add_button_control(name='increment', label='Increment')
        self.add_button_control(name='decrement', label='Decrement')
```

For the exact API, please see the source code or use argument completion while coding.

### Attributes

Attributes are used to either
-   make state persistent when the node is serialized and deserialized
-   expose a value to the user, making it editable in the inspector

Attributes' type is `Topic`. There are different types of topics:

- StringTopic: A string value.
- IntTopic: An integer value.
- FloatTopic: A float value.
- ListTopic: A list of values.
- DictTopic: A dictionary of key-value pairs.
- SetTopic: A set of values.
- EventTopic: An event that can be triggered. It has no value.

To create an attribute, call `add_attribute()` in the `build_node()` method. For example:

```python
class CounterNode(Node):
    def build_node(self):
        self.count_topic: IntTopic = self.add_attribute('count',IntTopic,init_value=0)
```

To get and set the value of an attribute, use the `get()` and `set()` methods. For example:

```python
class CounterNode(Node):
    def build_node(self):
        self.count_topic: IntTopic = self.add_attribute('count',IntTopic,init_value=0)

    def double_click(self):
        self.count_topic.set(self.count_topic.get() + 1)
```

To expose an attribute to the inspector, call `expose_attribute()` in the `build_node()` method. For example:

```python
class CounterNode(Node):
    def build_node(self):
        self.count_topic = self.add_attribute('count',StringTopic, value=0)
        self.expose_attribute(self.count_topic, editor_type='text', label='Count')
```

Avaliable `editor_type` are:
-   `text`: A text field. Compatible with `StringTopic`.
-   `list`: A list of values. Compatible with `ListTopic` (with string entries only).
-   `int`: An integer value. Compatible with `IntTopic`.
-   `float`: A float value. Compatible with `FloatTopic`.
-   `button`: A button. Compatible with `EventTopic`.
-   `options`: A dropdown menu. Compatible with `StringTopic`.
-   `dict`: A dictionary of key-value pairs. Compatible with `DictTopic`.