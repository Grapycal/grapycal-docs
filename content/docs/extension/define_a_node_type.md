---
weight: 200
title: "Define a Node Type"
date: 2024-03-30T15:34:16+08:00
draft: false
toc: true
---

To define a node type, you need to create a class that extends the
`Node`, `FunctionNode`, or `SourceNode` class. `FunctionNode` and
`SourceNode` are both subclasses of `Node`, providing higher-level
interfaces to define nodes.

>💡 The source code of
[grapycal_builtin](https://github.com/Grapycal/Grapycal/tree/main/extensions/grapycal_builtin)
contains various concrete examples of node definitions, which would be
helpful for understanding how to define a node.


## Inheriting Node

To give a node type a custom behavior, inherit from the `Node` class and implement the methods you need.

This guide walks through all methods/settings that can be implemented.

### Specify the Category {#specify_the_category}

Each node type must has a category. For example:

```python
class CounterNode(Node):
    category = 'demo'
```

Categories are used to group nodes in the node list UI.

### Implement `build_node()`

The `build_node()` method is called once when a node is created. This is
where you define the structure of the node, i.e.
[controls](../node/node#controls),
[ports](../node/node#ports), and
[attributes](../node/node#attributes). Here's an example:

```python
class CounterNode(Node):
    category = 'demo'

    def build_node(self):
        self.text = self.add_text_control('0')
        self.button = self.add_button_control('Add')
```

The above code results in a node with a text field and a button.

Things you can do in `build_node()`:

-   `self.add_text_control`: Add a Text Control to the node.
-   `self.add_button_control` : Add a Button Control to the node.
-   `self.add_image_control` : Add an Image Control to the node.
-   `self.add_in_port` : Add an Port to the node.
-   `self.add_out_port` : Add an Port to the node.
-   `self.add_attribute` : Add an Attribute to the node.
-   `self.expose_attribute` : Expose an attribute to the inspector
    panel, so it can be edited by the user when the node is selected.

- Set values of the node's inherent attributes to customize the node's appearance.
    - `self.shape`: can be `'normal'`, `'simple'`, or `'round'`. The default is `'normal'`.

    - `self.label`: the node's label.

    - `self.label_offset`: the vertical offset of the label from the node's
    center. It's primarily used to adjust the label's position when the node's shape is `'round'`.

### Implement `init_node()`

The `init_node()` method is called every time a node object is
instantiated. It's used for initialization tasks other than adding
controls, ports, and attributes. Here's an example:

```python
class CounterNode(Node):
    category = 'demo'

    def build_node(self):
        self.text = self.add_text_control('0')
        self.button = self.add_button_control('Add')

    def init_node(self):
        self.i=0
        self.button.on_click += self.button_clicked

    def button_clicked(self):
        self.i += 1
        self.text.set(str(self.i))
```

With the code above, the number increases every time you press the "Add"
button:

Things you can do in `init_node()`:

-   initialize variables that will be used in the node
-   add callbacks to controls or attributes

>⚠️ Do not confuse `init_node()` with `build_node()`. See [Lifecycle of a
Node](../node/Lifecycle_of_a_Node) for more details.

### Implement `destroy()`

`destroy()` is called when a node is being deleted. Override this method
to do cleanup tasks such as closing a file or releasing a resource. It's
mandatory to return `super().destroy()` at the end of the method.

Example usage:

```python
def destroy(self):
    self.file.close()
    return super().destroy()
```

---

Above are methods related to node creation and deletion.
Next, let's see the "node event" methods. These methods determins the runtime behavior of the node.

Things you can do in these methods:

-   Get data from input ports.
-   Push data to output ports.
-   Do any calculations or operations.
-   `self.print()`: Print a message to the inspector panel.
-   `self.run()`: Run a complex custom task. There are 2 advantages to use
    `self.run(task)` instead of just `task()`:
    -   If the task raises an exception, the exception will be caught and
        printed to the inspector panel instead of possibly crashing the
        whole program.
    -   The node event methods are possibly called from the UI thread. If
        the task takes a long time to run, the UI will freeze. `self.run()`
        will run the task in the background thread (i.e. the runner) to avoid freezing the UI.


An example to run a task in the background thread:

```python
class CnnNode(Node):
    ...

    def port_activated(self, port: Port): # could be called from UI thread or background thread
        self.run(self.task)

    def task(self): # will be executed in background thread (runner)
        x = self.in_port.get_one_data()
        y = self.cnn.forward(x) # this line may take a long time or raise an exception
        self.out_port.push_data(y)
```

### Implement `port_activated()`

Called when an edge on an input port is activated.

### Implement `input_edge_added()`

Called when an edge is added to an input port.

### Implement `input_edge_removed()`

Called when an edge is removed from an input port.

### Implement `output_edge_added()`

Called when an edge is added to an output port.

### Implement `output_edge_removed()`

Called when an edge is removed from an output port.

### Implement `double_click()`

Called when the node is double clicked by an user.

## Inheriting FunctionNode {#inheriting_functionnode}

The `FunctionNode` class is a node that takes inputs from one or more
ports, performs a calculation on the data, and outputs the result to one
or more ports. It is passive i.e. it only activates when there is data
on all of its input ports.

To define a `FunctionNode`, simply define the following:

-   `category`: The category of the node. This is used to group nodes
    together in the node library.
-   `inputs`: A list of the names of the input ports.
-   `outputs`: A list of the names of the output ports.
-   `calculate()`: A function that takes the input data and returns the
    output data.
    -   Arguments: For each input port, one argument will be passed to
        the function. Each argument will be a list of data from all
        edges connected to that port.
    -   Return: A dictionary of the output data to send to every output
        port. The keys of the dictionary should be the names of the
        output ports. For each output port, all its edges will receive
        the same data. If there is only one output port, then the
        dictionary is omitted and the data should be returned directly.
-   `max_in_degree` (Optional): A list of the maximum number of edges
    that can be connected to each input port. If `None` is specified,
    then there is no limit.
-   `display_port_names` (Optional): A boolean that determines whether
    the port names are displayed on the node. Defaults to `True`.
-   Since `FunctionNode` is a subclass of `Node`, you can also do
    everything in [Inheriting Node](#inheriting-node) to
    further customize the node.

For example:

```python
class AdditionNode(FunctionNode):
    '''
    Adds a set of numbers together.
    '''
    category = 'function/math'
    inputs = ['numbers']
    max_in_degree = [None]
    outputs = ['sum']

    def calculate(self, numbers:List[Any]):
        return sum(data)
```

## Inheriting SourceNode {#inheriting_sourcenode}

Unlike the passive `FunctionNode`, the `SourceNode` class actively sends
data to its output ports when dobule-clicked or receives a signal from
from its run port. It is usually used to start a graph running.

To define a `SourceNode`, simply define the following:

-   `task()`: The task to perform when the node runs. Usually pushs data
    to its output ports.
-   Since `SourceNode` is a subclass of `Node`, you can also do
    everything in [Inheriting Node](#inheriting-node) to
    further customize the node.