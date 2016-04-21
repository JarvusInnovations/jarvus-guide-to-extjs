# Batch All The Things

Take care that sequences of operations don't trigger more downstream operations than they need to.

Examples:
 - Setting multiple fields on a model
 - Changing multiple models in a store
 - Making multiple layout/sizing changes to a container
   - See https://www.sencha.com/blog/performance-optimization-for-layout-runs/
 - A custom component with multiple inputs