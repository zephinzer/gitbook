# Git Commands

## Initialisation <a id="initialisation"></a>

### Clone a repository <a id="clone-a-repository"></a>

{% hint style="info" %}
I would like to make a local copy of code from a public repository I found online.
{% endhint %}

```text
# for http-based
git clone https://zephinzer@github.com/zephinzer/blog.joeir.net

# for ssh-based
git clone ssh://git@github.com/zephinzer/blog.joeir.net
```

### Create a new repository <a id="create-a-new-repository"></a>

{% hint style="info" %}
I would like to initialise a new Git repository on my computer.
{% endhint %}

```text
git init
```

## Remote management <a id="remote-management"></a>

### Add a Git remote <a id="add-a-git-remote"></a>

{% hint style="info" %}
I would like to add a new remote named `origin` to my repository.
{% endhint %}

```text
git remote add origin ssh://git@github.com/zephinzer/blog.joeir.net
```

### Update the Git remote <a id="update-the-git-remote"></a>

{% hint style="info" %}
I would like to update the URL for my remote named `origin` in my repository.
{% endhint %}

```text
git remote set-url origin ssh://git@github.com/zephinzer/blog.joeir.net
```

## Retrieving changes <a id="retrieving-changes"></a>

### Fetch changes <a id="fetch-changes"></a>

Fetching retrieves the changes but **does not** merge the changes with your local copy.

{% hint style="info" %}
I would like to get updates from the remote but I don't want to update my code yet.
{% endhint %}

```text
git fetch
```

### Merge changes <a id="merge-changes"></a>

Merging takes the remote changes that have been fetched from the remote and merges them with your local copy.

{% hint style="info" %}
I have reviewed the changes I retrieved from the remote and I want to update my local code to match the remote's copy now.
{% endhint %}

```text
git merge HEAD
```

### Pull changes <a id="pull-changes"></a>

Pulling basically does a fetch and merge.

{% hint style="info" %}
I would like to update my code so that it is the same as the remote's.
{% endhint %}

```text
git pull
```

### Pull with Rebase <a id="pull-with-rebase"></a>

Pulling with rebase does a fetch, but before merging in the remote changes, it rolls back to a state before all remote changes were made, applies the remote changes, and then applies your local changes.

{% hint style="info" %}
I would like to update my code by placing whatever's from the remote before my current changes so that it is the same as the remote's and I don't have a merge commit.
{% endhint %}

```text
git pull -r
```

### Rebase <a id="rebase"></a>

{% hint style="info" %}
I would like to pull in changes from another branch that's available locally and place those changes before the changes I've committed.
{% endhint %}

```text
# assuming we are on branch feature_x pulling in updates from master
git rebase master
```

## Saving changes <a id="saving-changes"></a>

### Stashing <a id="stashing"></a>

{% hint style="info" %}
I would like to temporarily store my unstaged changes so that I can pull in the latest updates from the remote.
{% endhint %}

```text
# put all unstaged changes into a stash
git stash

# checking stashed changes
git stash list

# popping the last stashed change
git stash pop
```

### Staging <a id="staging"></a>

{% hint style="info" %}
I would like to add file\(s\) that will be 'saved' during a commit.
{% endhint %}

```text
# to stage all changes, run this from project root
git add .

# to stage only one file
git add ./path/to/changed_file
```

### Commiting <a id="commiting"></a>

{% hint style="info" %}
I would like to save my changes to my local Git repository.
{% endhint %}

```text
git commit -m 'some message'
```

### Commiting without any changes <a id="commiting-without-any-changes"></a>

{% hint style="info" %}
* I would like to add a commit to my local repository without adding any files
* I would like to have a commit that can trigger a pipeline in the remote source control
{% endhint %}

```text
git commit --allow-empty 'some message'
```

## Modifying changes <a id="modifying-changes"></a>

### Adding a file to a previous commit <a id="adding-a-file-to-a-previous-commit"></a>

{% hint style="info" %}
I forgot to run `git add` on a file that should be in the previous commit.
{% endhint %}

```text
# stage the missing file first
git add ./path/to/missed/file;

# this will add the staged file to the previous commit
git commit --amend
```

### Squash commit <a id="squash-commit"></a>

{% hint style="info" %}
I have made 5 commits and I would like to compress them into a single commit so my Git history is cleaner.
{% endhint %}

```text
# indicate `p` or `pick` for the head commit, and `s` or `squash` for the rest
git rebase -i HEAD~5
```

#### Squashing till origin/master/HEAD <a id="squashing-till-originmasterhead"></a>

{% hint style="info" %}
I have made X number of commits to my branch and want to squash/rebase my commits within my branch so that a rebase with master will be cleaner
{% endhint %}

```text
git rebase -i HEAD~$(git log --oneline master..HEAD | wc -l);
```

### Uncommit last commit <a id="uncommit-last-commit"></a>

{% hint style="info" %}
I would like to reverse the last commit but leave changes I made intact
{% endhint %}

```text
# this will leave the committed files as staged
git reset --soft HEAD^

# this will also unstage all changes
git reset HEAD^
```

### Reverting a commit <a id="reverting-a-commit"></a>

{% hint style="info" %}
I would like to create a commit that reverses the changes in a certain commit with hash `${COMMIT_HASH}`
{% endhint %}

```text
# use `git log` to find the commit hash of the commit you wish to undo the effects of
git revert ${COMMIT_HASH}
```

## Submitting changes <a id="submitting-changes"></a>

### Pushing <a id="pushing"></a>

{% hint style="info" %}
I would like push all committed changes from my computer to the remote
{% endhint %}

```text
git push
```

### Force Pushing <a id="force-pushing"></a>

{% hint style="info" %}
I have modified a commit locally and am unable to push normally to the remote since I rewrote history \(**WARNING**: this will erase any changes others may have made between when the original commit was made, and your current commits\)
{% endhint %}

```text
git push -f
```

## Assessing changes <a id="assessing-changes"></a>

### View all current changes <a id="view-all-current-changes"></a>

{% hint style="info" %}
I would like to see what files have been staged
{% endhint %}

```text
git status
```

### View commit history <a id="view-commit-history"></a>

{% hint style="info" %}
* I want to do an interactive rebase \(squashing\) and I would like to see which commit I should rebase up till
* I want to see what changes have been made by other team members/developers
{% endhint %}

```text
# interactive browsing of git commits
git log

# output only the last 5 logs
git log -n 5
```

### View difference between commits <a id="view-difference-between-commits"></a>

{% hint style="info" %}
I would like to check out what changes have been made between two commits
{% endhint %}

```text
# view file changes from HEAD to ${COMMIT_HASH}
git diff HEAD ${COMMIT_HASH}
```

## Viewing repository information <a id="viewing-repository-information"></a>

### View the Git configuration <a id="view-the-git-configuration"></a>

{% hint style="info" %}
I would like to see who am I committing code as
{% endhint %}

```text
git config -l
```

### Check which branch you're on <a id="check-which-branch-youre-on"></a>

{% hint style="info" %}
I would like to confirm which branch I am on
{% endhint %}

```text
git branch
```

### See all remotes <a id="see-all-remotes"></a>

Why

* I would like to see which remotes I am pushing to

```text
git remote -v
```

### Checking which `.gitignore` is ignoring a file <a id="checking-which-gitignore-is-ignoring-a-file"></a>

Why

* I would like to know which `.gitignore` is causing a file to be ignored without any obvious reason

```text
cd `./path/to`;
git check-ignore -v *;
```

## Repository adminstration <a id="repository-adminstration"></a>

### Creating a new branch from an existing one <a id="creating-a-new-branch-from-an-existing-one"></a>

{% hint style="info" %}
I would like to create a new branch based on the current one I'm on
{% endhint %}

```text
git checkout $SOURCE_BRANCH_SLUG;
git checkout -b $NEW_BRANCH_SLUG;
```

### Deleting a local branch <a id="deleting-a-local-branch"></a>

{% hint style="info" %}
I would like to delete a local branch so that `git branch -a` is less messy
{% endhint %}

```text
git branch -D $BRANCH_SLUG;
```

### Deleting a remote branch <a id="deleting-a-remote-branch"></a>

{% hint style="info" %}
I have deleted a local branch and want to delete the pushed branch too
{% endhint %}

```text
git push origin --delete $BRANCH_SLUG;
```

### Deleting local remote branches that have been deleted remotely <a id="deleting-local-remote-branches-that-have-been-deleted-remotely"></a>

{% hint style="info" %}
Someone else deleted a remote branch and I don't want it locally
{% endhint %}

```text
git remote update origin --prune;
```

