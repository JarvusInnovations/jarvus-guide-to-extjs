# Building Components

## When to use a component
It is usually best to start with plain HTML (via `Ext.Component`'s `html` or `tpl`+`data` config), and turn things into components only when you find yourself about to start replicating the functionality they provide.

For example, if you have an array of data to render, just start with one big `tpl` with a `<tpl for="...">` loop over the array to repeat elements. Once you want to have that array being added to, removed from, having properties within items changed, etc continuously after initially being rendered, then it's time to use a `DataView` or `List` component that lets you abstract the data into a `Store` and just provide a template or component for each individual item in the list. A DataView just repeats the given HTML for each item in the store while a `List` builds upon `DataView` with a more rigid vertical structure that supports richer features like scrolling, paging, grouping, etc.

If you need to include some minor interactions within the repeated HTML you can use links and button elements in your `itemTpl` markup and just listen to the `itemtap` event provided by `DataView`/`List`. The `itemtap` event fires when any part of an item gets tapped and does the work of figuring out which record it corresponds to, which is provided as an argument to the event handler alongside the `Event` object. The handler will get called for any and all taps inside the list, but you can use something like `var button = ev.getTarget('button'); if (!button) { return; } if (button.hasCls('confirm')) { ... };`

If you end up having a lot of internal state changes and interactions within each item in these lists though, you might want to consider using List's `useComponents: true` config to provide a custom component xtype instead of an itemTpl, which lets you break up and manage the pieces inside each row a lot more naturally.
