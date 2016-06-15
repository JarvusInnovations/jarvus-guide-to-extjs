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

