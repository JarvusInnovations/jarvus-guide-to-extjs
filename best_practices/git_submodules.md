# Git Submodules

[Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) allow you to clone other git repositories anywhere within your project's repository. You can check out any branch from the submodule's repository, but you get the entire contents of that branch added to your tree as-is -- you can't pick-and-choose what files and directories get pulled.

The benefit of using submodules vs just copying the repository into your tree manually is that with submodules the files don't actually get copied into your repository. Instead, only the configuration for cloning the repository and the hash of the commit you want to use get saved into your repository. This ensures that any developer checking out a given commit within your project's repository will always get the same exact version of the submodule's files too as the author of that commit had them.

From the perspective of your project's git repository, the path in your tree where the submodule is mounted just looks like a text file containing a commit hash. When you `cd` into the submodules directory with your shell or open it with a GUI application, git will behave as if you were in that other repository. If you make changes to any files within the submodule you need to commit and push them to the submodule repository first. Whenever the `HEAD` commit for the submodule changes -- either from making a new commit, checking out a new branch, or pulling updates -- a change with the new commit hash shows up to be staged in the main project repository. The change to the submodule `HEAD` can be committed to your project's repository on its own, or logically bundled with other changes as you would any other edit.

## Using Submodules in your Sencha Workspace

Submodules are a great way to add Sencha frameworks and packages to your project.

### Switching Existing Framework

If you generated your application with Sencha CMD it may have copied a minimal framework tree into your app or workspace. These are safe to replace with the full framework trees provided by any archive distributed by Sencha or mirrors of such checked into git.

As with any change to your project you want to start from a clean working tree. It is best to make the change entirely in one discreet commit so that each commit in your history is fully functional, and the commit for switching the framework to a submodule can be easily merged between branches.

If you've previously used `.gitignore` patterns to exclude frameworks from version control you'll want to start your commit by deleting any such patterns. This command run from the root of your project's repository may help you find them:

```bash
grep -H ext `find . -name ".gitignore"`
```

If you were ignoring the framework and not tracking it in your repository then you now just need to delete the files from your working tree:

```bash
rm -R ./sencha-workspace/ext-6.0.0.640/
```

Otherwise, if you *were* tracking the framework's files in your repository you'll need to use git's `rm` command instead to both remove the files from your working tree **and** mark them for deletion in your commit:

```bash
git rm -r ./sencha-workspace/ext-6.0.0.640/
```