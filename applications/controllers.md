# Controllers

## Properties of a good controller

- With the exception of one or two "global" controllers for each app that manage components that are always on the screen (for example one for the viewport and maybe a separate one for the main navigation system), each controller should handle a distinct and well-defined section of the app
- The controller should be the cause of all that sections dependencies being pulled into the app runtime and the app builds. Commenting out the controller line should erase nearly all of that section's assets from being loaded or included in builds
- The controller's only coupling with the outside world should be using `listen` to pick up events from the app or global controllers. Again it should be possible to comment out the controller and entirely suppress its section from the app without the app breaking beyond maybe some routes or buttons for entering its section becoming duds
- Has minimal state -- in most cases views should be the holders of state and controllers just act on them. For example, if a user management section has a "currently selected user", store that as a config option on the highest container in which it makes sense (like the overall container for the user manager section), attach firing an event to the update handler in the view, and have the controller listen to that event and call the getter when needed. A controller designed like this won't care when suddenly the app allows for two instances of the user manager to be open.
- Doesn't keep the same state in multiple places and have to deal with synchronizing them. Store state at its highest point of relevance, preferably in the view component hierarchy.
- Declares all dependencies explicitely, using higher-order dependency structures like `models` and `stores` when available. Use the generated getters exclusively instead of calls like `Ext.getStore(...)` or `Ext.create(...)` -- these calls should stand out as implicit dependencies that need to be eliminated.

## Formatting and Organization

- Two empty lines between sections
- One empty line between methods or multiline properties
- Has comments header that at least outlines its responsibilities
- Follow the preferred sections and order as closely as possible
- Within the entry points section, try to order things roughly in the order that a typical user would encounter them. This makes it easy to read through the controller and test out each piece comprehensively.
- Config handlers, event handlers, and route handlers are in the same order as their declarations

## Preferred sections and order

- `Ext.define` options
  - `extend`, `requires`, `alias`, etc
- Custom `config` options for mutable controller state
- Controller dependencies
  - `views`, `stores`, `models`
- Component references: `refs`
- Entry points
  - `routes`, `listen`, and `control` in that order
- Controller template methods
  - `init` / `onLaunch`
- Config handlers
  - use same order as in `config` object
- Route handlers
  - name like `showUser: function(userId) { ... }`
  - use same order as in `routes` object
- Event handlers
  - name like `onHelpButtonClick`
  - use same order as in entry points section
- Custom controller methods
  - Avoid method names that could unintentionally conflict in the future with getters/setters/updaters/appliers added by new configs. Start nothing with `get`/`update`/`apply`/`set`/`on`
  - Some common conventions for naming methods down here:
    - `doSomething()` -- carry out a complex operation that other parts of the controller preprocess or filter happening
    - `syncSomething()` -- idempotent methods that can be called belligerently to ensure some state is pushed from its point of authority to other places

## Controller example/template

```javascript
/*jslint browser: true, undef: true, laxcomma:true *//*global Ext*/
/**
 * The People controller manages the Pople section of the application where staff can
 * browse, search, create, and edit users
 *
 * ## Responsibilities
 * - Add People section to navigation menu
 * - Handle people/* routes
 */
Ext.define('MyApp.controller.People', {
    extend: 'Ext.app.Controller',
    requires: [
        'Ext.util.DelayedTask'
    ],



    // mutable state
    config: {
        /**
         * @private
         * Tracks the currently selected {@link MyApp.model.Person}
         */
        selectedPerson: null
    },


    // dependencies
    views: [
        'people.Container'
    ],

    stores: [
        'People'
    ],



    // component references
    refs: {
        navBar: 'myapp-navbar',
        peopleCt: {
            selector: 'myapp-people-container',
            autoCreate: true,

            xtype: 'myapp-people-container'
        }
    },


    // entry points
    routes: {
        'people': 'showPeople'
    },

    listen: {
        controller: {
            '#': {
                logout: 'onLogout'
            }
        },
        store: {
            '#People': {
                load: 'onPeopleStoreLoad'
            }
        }
    },

    control: {
        'myapp-navbar button[action=people]': {
            tap: 'onNavBarPeopleTap'
        },
        peopleCt: {
            activate: 'onPeopleCtActivate'
        }
    },


    // controller templates method overrides
    init: function() {
        // initialize any local controller properties
        // apply dynamic controller configurations before app launches
        // instantiate and configure any non-user utilities like task runners
        // DO NOT instantiate views, render views, or trigger data loading here
    },

    onLaunch: function() {
        // controllers are all initialized
        // instantiate and render top-level views if this controller manages any
    },


    // config handlers
    updateSelectedPerson: function(person, oldPerson) {
        this.syncSelectedPerson()
        this.getApplication().fireEvent('personselect', person, oldPerson);
    },


    // route handlers
    showPeople: function() {
        // transition app to viewing people screen from any prior state
        // DO NOT trigger data loading here, only apply the information in the route to the screen
        // if the route has any dynamic params, apply them as state to the UI or to store params/filters
        // route handlers should be idempotent
    },


    // event handlers
    onLogout: function() {
        // ...
    },

    onStudentsStoreLoad: function(peopleStore, people, success) {
        // ...
    },

    onNavBarPeopleTap: function() {
        // no transitioning UI directly from user intent! go through a route
        this.redirectTo('people');
    },

    onPeopleCtActivate: function() {
        // trigger loading any needed data for the screen here
        // show visual indicator of loading (if not using a component that automatically provides this)
        // DO NOT trigger loading if data already loaded and loading parameters have not changed
        // activate handlers should be idempotent
    },


    // custom controller methods
    syncSelectedPerson: function() {
        // apply selected person to UI state idempotently
    }
});
```

## Miscellaneous Best Practices

- ref names should be suffixed with an indication of the component's type. This provides some inline documentation on what methods are available, as well as helping avoid naming conflicts with all the many other getters that might exist on a controller.
- It's best to always have an xtype specified in a component selector. xtypes match hierarchically so you can use this xtype to guarantee the API you want. For example, in a controller that needs to call `.getValue()` and `.setValue()` on a `ref`ed component, you would use a `control` or `refs` selector like `'field[name=Host]'` because both those methods are parts of the  [`field` base component](http://docs.sencha.com/extjs/6.0.2-classic/Ext.form.field.Base.html) that all field components are based on. So by using the xtype selector before the `name` attribute selector, you both make the query faster but also guarantee that you get back a field API. Because you weren't overly specific with what type of field, this code can continue working unmodified if the UI configuration evolves to a different field type down the line.

