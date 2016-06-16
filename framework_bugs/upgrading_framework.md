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

The hotfix should also include a link to a forum post reporting the issue. Check if the issue's thread was marked by Sencha as fixed as-of a given framework release. If the bug is fixed, delete the hotfix and make a commit noting your findings in the extended message:

```bash
cd ./sencha-workspace/packages/jarvus-hotfixes
git rm overrides/data/field/FieldValidate.js
git commit -m 'Remove FieldValidate hotfix

Marked as resolved in the forum as-of ext-6.0.1.250. Verified via Fiddle'
```

If the issue remains in the new framework version, the hotfix may need to be updated. Compare the overridden framework class between the two framework versions:

```bash
cd ./sencha-workspace/ext-6.0.1.250
# use simple command-line diff:
git diff origin/6.0.0.640...origin/6.0.1.250 packages/core/src/data/request/Ajax.js
# use configured diff tool:
git difftool origin/6.0.0.640 origin/6.0.1.250 packages/core/src/data/request/Ajax.js
```

If the hotfix involves copying the body of a framework version to apply some internal changes, it needs to be updated and the fix manually re-applied. First, check the history of the hotfix file. It's originally implementation should have been spread across two commits: one copying the overridden method body as-is from the framework and a second applying the fix for the issue.

```bash
cd ./sencha-workspace/packages/jarvus-hotfixes
git log -p overrides/data/request/AjaxHeaders.js
```

Repeat this process by first committing the new function body from the new framework. Then adapt the changes applied in the original hotfix to the new function body. Test the new hotfix by pasting the contents of the hotfix's .js file at the top of the Sencha Fiddle that demonstrates the bug under the new framework. Commit the new function body in a second commit, explaining any changes that needed to be made in the extended commit message:

```bash
#
```