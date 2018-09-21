---
layout:     post
title:      git-flow VS trunk-based
subtitle:   
date:       2017-01-05
author:     BY annie dong
catalog: true
tags:
    - git
    - git-flow
    - trunk-based
---
以git来说，对比下现在讨论较多的git-flow 和 truncked-base的异同
帮助选择最合适自己team的开发流程

---

## Git-flow & Trunk-based
### Git-flow
1. 从dev分支拉出Release分支
2. 基于dev分支开发
3. Release分支发现的问题，只在Release分支修改
4. 测试完毕版本发布后，Release分支merge回master/dev分支，Release分支删除

### Trunk-based
1. 打tag，从master分支拉出Release分支
2. 我们继续基于master分支开发
3. Release分支发现的问题，先在主线修改,再cherry-pick到Release分支。最后发布时Release分支不需要merge回主线，也不需要删除
4. 测试完毕版本发布后，Release不用回主线。可以不删除，也可以过很久以后再删除


## Git-flow VS Trunk-based
### Git-Flow
·Git-Flow涉及分支 feature/master/develop/release/hotfix·
- 当您运行开源项目，或基于Pull Request的开发方式
- 当你有很多初级开发人员
- 当你有一个非常成熟的产品或者管理一个大型团队
### Trunk-Based
·Trunk-based涉及分支 master/release·
- 当你开始项目没多久
- 当你需要快速迭代，有成熟的CI&CD
- 当你主要与高级开发人员合作时

开发流程只是帮助team更好更快速的工作，没有真正标准，适合最重要，据我所知也有结合git-flow和trunk-based进行开发的team
