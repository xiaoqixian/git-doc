## Rebase

Rebase 可以重放另一个基础分支上的提交. 
例如, 当你从 master 分支中分出一个 topic 分支, 
而后, 在两个分支上分别有一些提交, 
正常状态下两个分支上的提交互不影响. 
```
          A---B---C topic
         /
    D---E---F---G master
```

若你想在 topic 分支上应用 master 分支上的提交, 
则需要通过 rebase 实现. 
假定当前位于 topic 分支上,
```git
git rebase master
```
则 git 历史提交会变成
```
                  A'--B'--C' topic
                 /
    D---E---F---G master
```
Originally, the base of the topic branch is commit E; 
now the base of topic branch is commit G, so it's called a **rebase**.

这种 rebase 操作在 master 分支需要 merge topic 分支的时候尤其有用,
因为 master 分支已经有新的提交, 直接 merge 将因为冲突导致失败. 
此时, 则需要将 topic rebase 到与 master 相同的提交上, 然后进行 merge.

### Conflicts

由于两个分支上均有修改, 有时不可避免会产生冲突. 
此时, 需要手动解决冲突或跳过产生冲突的提交或放弃这次rebase
```git
git rebase --continue
git rebase --skip
git rebase --abort
```

### Transplant

有时, 需要将分支结构进行移植. 譬如,
```
                            H---I---J topicB
                           /
                  E---F---G  topicA
                 /
    A---B---C---D  master
```
此时, topicB 是 topicA 的子分支. 
若实际上 **topicB 并不依赖于 topicA 的提交**, 
则可将 topicB 从 topicA 子分支移植到 master 的子分支,
这样可以精简项目的管理结构.

```git
git rebase --onto master topicA topicB
```

### Remove range of commits with rebase

before
```
    E---F---G---H---I---J  topicA
```
command
```git
git rebase --onto topicA~5 topicA~3 topicA
```
after
```
    E---H'---I'---J'  topicA
```

## Undo

### Discard changes to a file

```git
git checkout v1.2.3 -- file         # tag v1.2.3
git checkout stable -- file         # stable branch
git checkout origin/master -- file  # upstream master
git checkout HEAD -- file           # the version from the most recent commit
git checkout HEAD^ -- file          # the version before the most recent commit
```

## Delete

### Delete a local merged branch

```git
git branch -d branch_name
```

### Delete a remote branch

```git
git push origin --delete branch_name
```

## Submodule

Add a submodule to the current directory.
```git
git submodule add https://github.com/user/repo
```

### Cloning a Project with Submodules

After cloning, you have to run the following 2 commands:
```git
git submodule init   # generate a config file in local
git submodule update # fetch remote submodule repos
```

Or you can just clone these submodules when you clone the original repo.
```git
git clone --recurse-submodules https://github.com/user/repo
```
