# Life Cycle of an Application

## File Layout

In this example, we'll be looking at the simpler project structure that results when the `-modern` argument is passed to `sencha generate app`. Without passing `-modern` or `-classic`, a more complex "universal" app structure is generated to accommodate providing alternate UIs based on each in the same application.

```
workspace/MyApplication
└───.sencha
│   └───app
└───app
│   └───controller
│   └───model
│   └───store
│   └───view
│   └───Application.js
└───resources
└───sass
│   └───etc
│   └───src
│   └───var
│   └───config.rb
└───app.js
└───app.json
└───bootstrap.css
└───bootstrap.js
└───bootstrap.json
└───build.xml
└───index.html
```

## Phase 1: Loading

## Phase 2: Initialization

## Phase 3: Launch

## Phase 4: Interactive