# Controllers

## `refs`

From [github.com/JarvusInnovations/emergence-console/commit/b37d7869af73db68f06e2f07d320353c3fa3fbed#commitcomment-18657022](https://github.com/JarvusInnovations/emergence-console/commit/b37d7869af73db68f06e2f07d320353c3fa3fbed#commitcomment-18657022):

> ref names should be suffixed with an indication of the component's type. This provides some inline documentation, as well as helping avoid naming conflicts with all the many other getters that might exist on a controller.

> It's best to always have an xtype specified in a selector. xtype's match hierarchically so you can use this xtype to guarantee the API you want. For example in this controller you want to call `.getValue()` and `.setValue()` on it, which are parts of the field base component that all field components are based on. So by using the selector `'field[name=Host]'` you both make the query faster but also guarantee that you get back a field API, and this code continues working even if the UI gets worked on and different field types are used.

