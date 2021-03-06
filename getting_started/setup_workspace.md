# Setup Workspace

## Step 1: Initialize git repository

If you haven't already, initialize a new git repository to track your changes. Using git and keeping a good account of your changes is essential to a well-built and maintained Sencha project.

```bash
mkdir -p ~/Repositories/my-project
cd ~/Repositories/my-project
git init
```

## Step 2: Initialize Sencha workspace

The first thing to add to your repository is the scaffolding for a sencha workspace. This provides spaces for project-level configuration shared across applications.

```bash
sencha generate workspace ./sencha-workspace
git add --all
git commit -m 'Generate sencha workspace with CMD

Used command:

    sencha generate workspace ./sencha-workspace'
```

Every time you modify your project with a shell command, start from a clean working tree. If the command is successful, commit all changes and document the command you ran at once. Follow [best-practices for all commit messages](http://chris.beams.io/posts/git-commit/) and make use of [markdown code fencing](https://guides.github.com/features/mastering-markdown/#example-code).

## Step 3: Add Sencha framework to repository

[Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) are the best way to add frameworks to your project workspace. Using a submodule ensures that every clone of your repository uses the exact same code top-to-bottom. Without this you will encounter different machines and different developers getting different results from the *same code*.

In this example we are copying the latest GPL release of the ext framework as of this writing, but you may use any git-based source. If you use a commercially-licensed version of the framework, commit the extracted framework to a private repository of your own. Alternatively you may commit the framework directly to your project repository and control its access accordingly.

```bash
git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git ./sencha-workspace/ext-6.0.1.250
git commit -m 'Add ext-6.0.1.250 as submodule

Used command:

    git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git ./sencha-workspace/ext-6.0.1.250'
```

Again, you should commit the results of the command alongside documentation of the command.

## Step 4: Add hotfixes package *(optional)*

As with adding a framework to the project's workspace, Git Submodules are a great way to add packages to the workspace as well. In this example we will add the [community hotfixes package](https://github.com/JarvusInnovations/sencha-hotfixes) maintained by [Jarvus Innovations](http://jarv.us). This package (when required by your app) will apply community-reviewed hotfixes for known issues in your current framework version — saving you time and headaches fighting battles that have already been won.

```bash
git submodule add -b ext/6/0/1/250 --name jarvus-hotfixes https://github.com/JarvusInnovations/sencha-hotfixes.git ./sencha-workspace/packages/jarvus-hotfixes
git commit -m 'Add jarvus-hotfixes package as submodule

Used command:

    git submodule add -b ext/6/0/1/250 --name jarvus-hotfixes https://github.com/JarvusInnovations/sencha-hotfixes.git ./sencha-workspace/packages/jarvus-hotfixes'
```

## Step 5: Add `.eslintrc.js` file *(optional)*

[A file named `.eslintrc.js`](examples/.eslintrc.js) placed at the root of your Git repository or customized at the level of your `sencha-workspace` directory can document the coding style guidelines for the project. Editors and tools powered by ESLint can find and use these automatically on any machine the code is checked out to. This can help your project maintain consistent styling by ensuring all developers using capable editors see the same warning and error messages live while they work.

Placing and maintaining these options in a central `.eslintrc.js` file deprecates the old style of inserting `/*jshint...*/` configuration snippets at the top of each source file. Those should be purged en masse from your source files once a `.eslintrc.js` file is in place.

### Editors knowns to support .eslintrc.js

- [Visual Studio Code](https://code.visualstudio.com/) with the [ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### Sample file for Sencha workspace

```json
{
    "globals": {
        "Ext": true
    },

    "undef": true,
    "unused": true,
    "quotmark": "single",
    "curly": true,
    "noempty": true,
    "latedef": true,
    "immed": true,
    "freeze": true,
    "forin": true,
    "maxlen": 120,

    "browser": true
}
```
