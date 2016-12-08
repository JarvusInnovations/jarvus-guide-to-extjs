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

Review all changes made by the upgrade command, paying close attention to any made to `app.json` or `sencha.cfg` while mostly glossing over the rest. Unless Sencha CMD reported merge conflicts that need to be resolved, avoid making any changes until a follow-up commit. Commit all the changes made by the upgrade, documenting how you ran it:

```bash
git add --all
git commit -m 'Upgrade MyApplication app scaffolding with Sencha CMD

Used commands:

    cd ./sencha-workspace/MyApplication/
    sencha app upgrade --noframework'
```

## Step 3: Add the New Framework

Before deleting the old framework, add the new one to your workspace and test out the application against it. This workflow is particularly helpful if you have multiple apps in your workspace to gradually migrate and test. If you weren't appending version numbers to your framework's directory name before, now is a good time to start. In this example, the new framework being added to the repository is ext-6.0.1.250 from Jarvus' GitHub mirror of GPL releases:

```bash
git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git ./sencha-workspace/ext-6.0.1.250
git commit -m 'Add ext-6.0.1.250 as submodule

Used command:

    git submodule add --name ext-6.0.1.250 -b 6.0.1.250 https://github.com/JarvusInnovations/extjs.git ./sencha-workspace/ext-6.0.1.250'
```

## Step 4: Reconfigure Application

To finish upgrading an application to a new framework version, two things must be accomplished: 

1. Setting `app.framework.version` to the new framework version
2. Getting `ext.dir` pointing to where that framework is on disk

As setup by Sencha CMD, `ext.dir` is set at the workspace level for all your apps. Going forward you want to set it at the application level within `.sencha/app/sencha.cfg`. Setting `ext.dir` here will supercede any workspace-level setting, so you can leave the workspace's configuration alone and not interfere with any other apps in your workspace.

```bash
cd ./sencha-workspace/MyApplication/

# search for any references to old framework version
grep -H "6.0.0.640" `find . -name "*.cfg" -o -name "*.json" ! -name "codegen.json" ! -name "bootstrap.*"`

# set app.framework.version and ext.dir
vim ./.sencha/app/sencha.cfg
```

To avoid having to write the framework version in two places, you can set `ext.dir` using variables like this and only have to update `app.framework.version` going forward:

```
app.framework.version=6.0.1.250
ext.dir=${workspace.dir}/${app.framework}-${app.framework.version}
```

If you got the idea of setting `ext.dir` like this at the framework level -- good thinking but it won't work all the time since the `app.*` variables it's based on aren't always available at the workspace level.

Finally, attempt to build the application and verify that Sencha CMD reports using the correct version of the new framework near the beginning of the build process output before committing the changes:

```bash
sencha app build
git add --all
git commit -m 'Update MyApplication framework to ext-6.0.1.250'
```

## Step 5: Upgrade Overrides and Packages

If your application contains any overrides for framework classes, review them for compatibility and relevance to the new framework version. Ideally, your application does not contain any framework overrides outside of a supported hotfixes package. If this is the case the main thing you need to worry about is [upgrading your hotfixes package](../framework_bugs/upgrading_framework.md).

Additionally, for any 3rd party packages included in your workspace, check for any updates or framework upgrade directions that might be available. If submodules are used for packages, check if new framework-version-specific branches of any are available or need to be created.

## Step 6: Scrub Old Framework

Once all applications in your workspace have been migrated to and tested with the new framework, you may remove the old framework from your workspace. At this point you want to also update the `ext.dir` setting at the workspace level to keep it having a valid value and for any future use of `sencha generate` commands.

```bash
# update workspace-level ext.dir default value if present
vim ./sencha-workspace/.sencha/workspace/sencha.cfg

# remove submodule for old framework
git rm ./sencha-workspace/ext-6.0.0.640

git commit -m 'Remove ext-6.0.0.640 framework submodule'
```

