# Establish Testing Directory

Navigate to the application's sencha-workspace folder and make a directory called tests:
```
cd /path/to/sencha-workspace/
mkdir tests
```

You should now have a tests folder in your app's sencha-workspace. This folder will serve as the root of all our testing code and the location of the siesta framework. Download the latest version of Siesta-lite library here: [www.bryntum.com/products/siesta/download-lite/](www.bryntum.com/products/siesta/download-lite/)

Once your download finishes, extract the zip file and move the siesta folder into the tests directory you just made. In this folder there are some examples we don't need. Go ahead and delete them:
```bash
rm -rf examples/
rm -rf examples-touch/
rm Changes 
# IF Changes file exists
```

Now we want to create our Testing Harness. Create a index.html & index.js files in the tests/ folder:
```bash
touch index.html
touch index.js
```

The index.html file will contain the markup for our testing harness & the index.js file will contain the configuration for the test scripts.

Copy & paste the following into your index.html file, interpolating values where necessary:
```html
<!DOCTYPE html>
<html>
    <head>
        <!-- Siesta UI must use ExtJS 6.0.1 (you can specify any other ExtJS version in your "preload" config) -->
        <link rel="stylesheet" type="text/css" href="../ext-6.0.1.250/build/classic/theme-triton/resources/theme-triton-all.css" />
        <link rel="stylesheet" type="text/css" href="../tests/siesta-4.0.6-lite/resources/css/siesta-all.css">

        <!-- Siesta UI must use ExtJS 6.0.1 (you can target any other ExtJS version in your "preload" config) -->
        <script type="text/javascript" src="../ext-6.0.1.250/build/ext-all.js"></script>
        <script type="text/javascript" src="../ext-6.0.1.250/build/classic/theme-triton/theme-triton.js"></script>


        <script type="text/javascript" src="../tests/siesta-4.0.6-lite/siesta-all.js"></script>

        <script type="text/javascript" src="index.js"></script>
    </head>

    <body>
    </body>
</html>
```

Now we can configure our harness in our index.js file:
```js
var harness = new Siesta.Harness.Browser.ExtJS();

harness.configure({
    title: 'Spark Classroom Student Tests'
});

harness.start(
    {
        group: 'SparkClassroomStudent tests',
        preload: [
            '../ext-6.0.0.640/build/ext-all.js',
        ],
        items: [
          // array of your test scripts
          // '../SparkClassroomStudent/010_sanity.t.js'
        ]
    }
);
```

That's it! Simply navigate to your tests/ folder on your virtual host and you'll see the list of your test scripts on the left. If you wrote any ui tests you can view them in action by opening the DOM panel on the right.
