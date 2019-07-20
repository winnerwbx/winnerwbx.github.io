---
title: ARTS-WEEK-1-Tip
date: 2019-07-20 23:26:32
tags: ARTS Tip
---
## Tip

### Git使用小技巧：如何忽略已经提交的文件

使用Git的时候，经常遇到这样的问题，就是不小心提交了不想要的文件，想要让Git忽略这些文件

---

我们知道，git有一个 **.gitignore** 文件，加入到 **.gitignore** 的文件，就不会被git跟踪。

比如我们在干净的仓库新建一个 `test.txt` 的文件，执行`git status` ，在Untracked files里会看到有 `test.txt` 文件待提交

此时如果创建`.gitignore`文件，并把`test.txt`加入到 **.gitignore** 中，再执行`git status`，就看不到该文件在未跟踪区了，也就是被git忽略掉了。

---

但是，如果我们在创建`test.txt` 的文件后，先输入了 `git add test.txt` ，再把它加入到`.gitignore`文件中，会发生什么事呢？

```
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   test.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   .gitignore
```

看到了么？git并没有把这个文件忽略，仍然在暂存区。

#### 为什么呢？

因为`.gitignore`文件仅仅是对**未被git跟踪过的**文件生效，一旦文件被git跟踪记录过，就不好使了。

#### 该怎么办？

执行`git rm --cached test.txt`（如果要忽略的是文件夹，则要加`-r`参数）, 或者`git reset HEAD text.txt`，将该文件从暂存区移除（仅仅是从git的暂存区移除，不是从本地移除），这样，再执行`git status`，就看不到该文件在未跟踪区了，也就是被git忽略掉了。

---

更进一步，假如我们已经把想要忽略的文件已经commit甚至push了，该怎么办呢？

别着急，也是有办法的

1. 我们先把要忽略的文件加入到`.gitignore`文件中
2. 然后执行`git rm --cached [要忽略的文件]`（如`git rm --cached test.txt`或者`git rm -r --cached target/*`)
   
   此时看`git status`时，会是如下显示：
    ```
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
            deleted:    test.txt
    
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   .gitignore
    ```
    deleted表示会从git的库里删除
3. 执行`git add .gitigonre`，把`.gitignore`文件也加入到暂存区
4. 提交代码`git commit -m "some message"`，这样，以后再有对该文件的改动，也不会被git进行追踪了
5. 执行`git push`，提交到远程版本库，确保其他人拉取代码后，这个文件也会被忽略到。

**Bingo！大功告成~**
