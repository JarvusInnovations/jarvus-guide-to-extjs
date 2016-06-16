# Upgrading to a new Framework Version

When migrating an application to a new framework version, the branch of the hotfixes package used must change too.

## Checkout Remote Branch

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

If a branch is not available, fork the most recent framework branch that is available and review each hotfix.

## Reviewing Hotfixes for a New Branch
Every hotfix should include a link to a Sencha Fiddle that demonstrates the issue. First verify that you can see the original bug demonstrated by the Fiddle. Then switch the Fiddle to the new framework version and see if you can still reproduce it.

The hotfix should also include a link to a forum post reporting the issue — check that too to see if it is marked as fixed as-of a given framework release. If the bug is fixed