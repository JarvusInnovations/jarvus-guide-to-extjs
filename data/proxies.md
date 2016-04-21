# Proxies

## Built-in types
Sencha provides multiple general-purpose proxy implementations that are highly configurable. You can use these out-of-the-box without declaring any new classes, or you can declare an extending class to encapsulate common configuration options.

### `Ext.data.proxy.Ajax`

### `Ext.data.proxy.Rest`

### `Ext.data.proxy.Direct`

### `Ext.data.proxy.Memory`
A memory proxy is useful if you have an ephemeral client-side store but still want to configure a reader for loading models from an input data structure.

### `Ext.data.proxy.LocalStorage`
See also `Ext.data.proxy.SessionStorage`


## Building your own proxy

### Reuse configuration
You can simply extend one of the existing configurable types 

### Custom workflow
The `jarvus-apikit` package was created to make this job easier, it provides a base proxy class that is optimized around extendability whereas the framework's are optimized around configurability. Rather than have tons of overlapping config options you can set without writing any code, they have clear methods you can override to implement an arbitrary configuration scheme and workflow.