# Useful Git commands

## Basics

* show differences between index and working tree that is, changes you haven't staged to commit:
  ``` bash
  git diff [filename]
  ```

* show differences between current commit and index that is, what you're about to commit:
  ``` bash
  git diff --cached [filename]
  ```

* show differences between current commit and working tree:
  ``` bash
  git diff HEAD [filename]
  ```

## Configuration

### You can configure an individual repo to use a specific user / email address which overrides the global configuration. From the root of the repo, run

```bash
git config user.name "Your Name Here"
git config user.email your@email.com
```

#### Whereas the default user / email is configured in your ~/.gitconfig

```bash
git config --global user.name "Your Name Here"
git config --global user.email your@email.com
```

## Commit submodule changes

The submodule is its own repo/work-area, with its own .git directory.

1. So, first commit/push your submodule's changes:
    ```bash
    cd path/to/submodule
    git add
    git commit -m "comment"
    git push
    ```

1. Then tell your main project to track the updated version:
    ``` bash
    cd /main/project
    git add path/to/submodule
    git commit -m "updated my submodule"
    git push
    ```

## Submodule updates and init

For git 1.7.3 or above you can use:

`git submodule update --recursive`

or:

`git pull --recurse-submodules`

if you want to pull your submodules to latest commits intead of what the repo points to.

**Note: If that's the first time you checkout a repo you need to use --init first:**

`git submodule update --init --recursive`

For older, git 1.6.1 or above you can use something similar to (modified to suit):

`git submodule foreach git pull origin master`
