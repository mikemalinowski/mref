# mref

mref is a small library that takes on some of the functionality that pymel
previously held. Allowing a user to get a reference to a node and interact
with it at a higher level. 

Rather than dealing with large hierarchical structures it just binds traits
to the object which then exposes functionality. 

This is very much an early alpha.

Example of use:

```python
import mref
from maya import cmds

node_a = mref.create("transform", name="node_a")
node_b = mref.create("transform", name="node_b", parent=node_a)

# -- We cna mix in cmds too - lets use cmds to create a node
# -- and then get the reference to that using mref
node_c = cmds.createNode("transform", name="node_c")
node_c = mref.get(node_c)

# -- Now lets rename them
node_a.rename("foo_a")
node_b.rename("foo_b")
node_c.rename("foo_c")

# -- As we're dealing with a reference we can still interact with
# -- it. Because its a transform, it will have the DAG trait, allowing
# -- us to manage parenting
node_a.add_child(node_b)

# -- Lets add an attribute, and then print the value
node_a.add_attribute(name="my_name", value="bob", attribute_type="string")
print(node_a.attr("my_name").get())

# -- Finally, lets use maya cmds commands with these objects
cmds.parentConstraint(node_c.name(), node_a.name())
```