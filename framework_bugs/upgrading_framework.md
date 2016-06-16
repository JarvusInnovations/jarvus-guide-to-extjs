# Upgrading to a new Framework Version

When migrating an application to a new framework version, the branch of the hotfixes package used must change too.

First, check if your upstream source for the hotfixes package already has a new branch for this framework version. In this example we're using Jarvus' hotfixes package and upgrading an app from ext-6.0.0.640 to 6.0.1.250:

```bash
cd ./sencha-workspace/packages/jarvus-hotfixes
git fetch --all
git branch -r
```

If a branch is available, check it out and update the submodule configuration to the new branch:

```bash
git checkout ext/6/0/1/250
cd ../../../
git config -f ./.gitmodules submodule.jarvus-hotfixes.branch ext/6/0/1/250
```