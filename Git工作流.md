## GitFlow

由于 Git 上分支使用的便捷性，产生了很多的 `Git工作流`

指的就是，在整个项目开发周期的不同阶段，你可以同时拥有多个开放的分支，你可以定期地把某些主题分支合并入其它分支中。

### 工作流一

![image](https://github.com/user-attachments/assets/04459d47-69c2-4b16-9f5f-779a975a1975)


master(main) 作为主分支，在这个工作流里面，是不在这个默认分支里面进行开发的，而是在一个专门的 `develop` 分支里面进行开发。master 分支当然也是有其存在的价值的，当 develop 分支里面有一个稳定的版本的时候，是会将其合并到 master 分支的。这样我们在 git log 的时候，就只会看到 master 分支里的一些重要的版本在进行一些合并的记录，就不会像 devlop 分支里面有各种各样的开发记录。

`develop` 作为开发分支，在这个分支里面不断去开发一些小的功能，当有稳定版本时，再将稳定版本合并到 master 分支当中。

topic 作为某一主题或者功能或者特效的分支进行开发，如果这个功能开发地很好，那么可以考虑合并到 devlop 分支里面，如果这个模块开发地不好，那么会导致既没有开发出来，也把 devlop 分支的提交记录弄得很乱。因此往往在开发一个模块的功能的时候，会在 devlop 分支下新开一个 topic 分支来进行开发，开发得好，就合并进去。

### 工作流二

![image](https://github.com/user-attachments/assets/b12e09e3-ce6d-4a41-a46e-a039c46d277f)

项目刚刚起步，项目的一开始的起点在 Master Develop 都是可以的，当在develop 里有一个重要的版本的时候，就可以在 Master 打上tag 标记，然后再继续在 devlop 分支里面继续开发，此时，发现 v1.0 出现了 bug，这个时候就可以在 v1.0 新建分支 Hotfix，然后修复对应 bug，修复完成后，再合并到 master 中，同时再将其合并到 devlop 分支里面，然后继续进行开发。如何在开发过程中，有一些新的功能模块，就可以开一个 Feature 进行开发，如果开发完了，可以将其集成到 develop 分支。这里有一个 Release 分支，他的意思是项目发布部署的 Release 版本，也就是说当 Develop 分支开发完了之后，并不会直接进行发布，而是在这个时候开一个 Release 分支，在这个 Release 分支里面进行测试，如果出现了 Bug ，签出 hotfix 分支，在这里进行 bug 修复，修改完了之后，合并至 release 分支，这就是我们最终要发布的版本了，然后将其合并到 master 分支，同时也合并到该版本对应的 Develop 分支里。

### 协同合作进行多分支开发时

合并代码的时候, 你需要先提交自己分支的代码, 然后将目标分支的代码先合并到自己的分支, 如果存在冲突, 就本地解决代码冲突, 合并代码之后, 再将自己的分支合并到目标分支, 多这一步操作的原因就是在自己本地先解决完冲突, 如果你直接合并过去, 就会覆盖了同事的代码。
