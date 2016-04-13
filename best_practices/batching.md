# Batch All The Things

Take care that sequences of operations don't trigger more downstream operations than they need to.

Examples:
 - Setting multiple fields on a model
 - Changing multiple models in a store
 - Making multiple layout/sizing changes to a container
 - A custom component with multiple inputs