# Upgrading an Application's Framework

## Step 1: Use Submodules for Frameworks

If your project is not already using Git Submodules to add Sencha frameworks to your workspace, [switch to using submodules](../best_practices/git_submodules.md) with your **current framework version** before proceeding and ensure that doesn't break anything.

## Step 2: Upgrade Build Scaffolding

In addition to your source code, styles, and resources, each Sencha application also has embedded within it configuration and code for the build process. Most of this is contained within the hidden `.sencha/app` tree, but a few files in the root of the application tree play a role too. You typically do not modify any of these files, and new versions of Sencha CMD ship with new versions of them. Before upgrading what framework an application uses, you first want to upgrade these build files with the latest version of Sencha CMD:

```bash
cd ./sencha-workspace/MyApplication/
sencha app upgrade --noframework
```

The `--noframework` option tells Sencha CMD to only upgrade the application's build files. You will be upgrading the framework by reconfiguring the submodule instead of letting CMD download or copy it.

Review all changes made by the upgrade command, paying close attention to any made to `app.json` or `sencha.cfg` while mostly glossing over the rest. Commit all the changes made by the upgrade, documenting how you ran it:

```bash
git add --all
git commit -m 'Upgrade MyApplication app scaffolding with Sencha CMD

Used commands:

    cd ./sencha-workspace/MyApplication/
    sencha app upgrade --noframework'
```