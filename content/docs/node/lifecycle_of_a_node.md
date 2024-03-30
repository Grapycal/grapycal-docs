---
weight: 999
title: "Lifecycle of a Node"
description: ""
icon: "article"
date: "2024-03-30T23:30:38+08:00"
lastmod: "2024-03-30T23:30:38+08:00"
draft: false
toc: true
---

This document describes how nodes are created, deleted, restored, updated, saved, and loaded in Grapycal.

In case you're overwhelmed by reading this page, just read the [conclusion](#conclusion).

![Image](https://i.imgur.com/deOzDTd.png)

https://excalidraw.com/#json=UUIVcXygC4-7GRovE2Uxw,8WvXvCo43xdHG8PScqLmmA

## Creation Deletion and Restoration
When a node is directly created by the user (by dragging it from the node library or using a command), the following steps will happen:

> **Node Creation Process**
> 1. An `Node` object is instantiated.
> 1. `build_node()` is called on the `Node` object.
> 1. `init_node()` is called on the `Node` object.
> 1. The node is ready to work and is displayed on the GUI.

The `build_node()` method is used to set up the node's structure, such as creating ports and controls. The `init_node()` method is used to set up the node's runtime state, such as initializing variables.

When the user deletes the node, the Node object will be serialized before its destruction.

> **Node Deletion Process**
> 1. The serialized data of the `Node` object is saved in the history.
> 1. The `Node` object is destroyed.

If the user undoes the deletion, the following steps will happen to restore the node:

> **Node Restoration Process**
> 1. An `Node` object is instantiated. (Not the same object as the one that was destroyed)
> 1. The serialized data is loaded into the `Node` object.
> 1. `init_node()` is called on the `Node` object.
> 1. The node is ready to work and is displayed on the GUI.

Note that the second step are different between the **creation** and **restoration** processes. When **restoring**, instead of calling `build_node()` again, the serialized data is automatically loaded into the node object so attributes, ports, and controls are restored.

Above also applies to these scenarios: 
-   the user undoes the creation of a node then redoes it
-   the user redoes the creation of a node then undoes it again

## Updating the Extension

When the user updates an extension, the following steps will happen:

> **Extension Update Process**
> 1. All nodes created by the extension are serialized and destroyed.
> 1. The extension is unimported.
> 1. The extension is reimported.
> 1. `Node` objects are instantiated again.
> 1. `build_node()` is called on the `Node` objects.
> 1. `init_node()` is called on the `Node` objects.
> 1. The nodes are ready to work and are displayed on the GUI.

The nodes are destroyed and recreated because the extension may have changed the structure of the nodes. The `build_node()` method is called again to apply the node's new structure, and the `init_node()` method is called to set up the node's new runtime state.

In the `build_node()` step, for each attribute added to the node, Grapycal will try to restore the value of the attribute from the serialized data. If an attribute of the same name is found in the serialized data, the value will be restored.

Similarly, Grapycal will try to restore the controls' values from the serialized data. 

## Saving and Loading

When the user saves the workspace, nodes are serialized and saved to a file. When the user loads the workspace, the serialized data is loaded and used to restore the nodes.

The restoration process is identical to the **Extension Update Process** described above.

## Conclusion

When defining a node types, you should follow these guidelines:
-   Set up the node's structure only in `build_node()`
-   Set up the node's runtime state only in `init_node()`
-   Never set up the node's runtime state in `build_node()`, as it will not be called when the node is restored
-   Never add attributes, ports, or controls in `init_node()`. `init_node()` is called every time the node is instantiated, so doing so will cause duplicate attributes, ports, or controls to be added to the node.
