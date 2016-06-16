# Setup workspace

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

Git Submodules are the best way to add frameworks to your project workspace. Using a submodule ensures that every clone of your repository uses the exact same code top-to-bottom. Without this you will encounter different machines and different developers getting different results from the *same code*.

In this example we are copying the latest GPL release of the ext framework as of this writing, but you may use any git-based source. If you use a commercially-licensed version of the framework, commit the extracted framework to a private repository of your own. Alternatively you may commit the framework directly to your project repository and control its access accordingly.

```bash
git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git sencha-workspace/ext-6.0.1.250
git commit -m 'Add ext-6.0.1.250 as submodule

Used command:

    git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git sencha-workspace/ext-6.0.1.250'
```

Again, you should commit the results of the command alongside documentation of the command.