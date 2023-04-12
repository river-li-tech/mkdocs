---
tags:
  - 游戏
  - 工具
comments: true
---

**一种基于指令集的技能编辑器：将技能抽象为基于时间轴的主线（也支持控制逻辑和跳转），通过在时间轴上挂接技能指令的方式来表达技能逻辑。**

工程[地址](https://github.com/river-li-tech/VisualSkillEditor)
工程[文档wiki](https://github.com/river-li-tech/VisualSkillEditor/wiki)

## 内容列表

- [背景](#背景)
- [文档](#文档)
    - [编辑器帮助](https://github.com/river-li-tech/VisualSkillEditor/wiki/编辑器帮助)
    - [技能结构和指令集](https://github.com/river-li-tech/VisualSkillEditor/wiki/技能结构和指令集)
    - [技能运行时](https://github.com/river-li-tech/VisualSkillEditor/wiki/技能运行时)
- [编译](#编译)

## 背景

**为什么需要这样一款编辑器**
在许多类型的游戏中，良好的技能表现都是游戏的一大卖点。技能系统结构复杂，牵扯到游戏中几乎所有的基础子系统，同时为为上层逻辑提供支持。策划团队往往期望能够以一种直观、独立的方式组织技能逻辑，他们既希望可以自由组合技能逻辑，同时不希望配置过于复杂。这样的矛盾往往会让程序设计走向两个极端：
1. 一种方式是提供一种可平行扩展的方式，不断特写逻辑以满足个性化需求。这种方式在前期往往表现良好，但当项目规模膨胀到一定程度时，极有可能出现复杂度暴增导致失控的情况；
2. 另一种方式是把策划需求拆分成尽可能独立的一些子逻辑。这种方式对程序设计人员的业务抽象和代码设计能力要求极高，代码需要持续、谨慎地维护，同时解决冗余和复杂的配置方式也是一大挑战。

针对以上问题，我们引入一种基于指令集的技能系统：将技能抽象为基于时间轴的主线（也支持控制逻辑和跳转），通过在时间轴上挂接技能指令的方式来表达技能逻辑。

**提供以下特性：**

1.  提供30+条指令，全部为瞬发指令，基本满足正交性，各自独立且便于扩展；
2.  使用xml格式来保存技能逻辑，同时提供一套标准化的解析和执行运行时(额外提供)；
3.  可视化编辑，在编辑器中拖拽指令和控制结构，一目了然；

## 文档

- [编辑器帮助](https://github.com/river-li-tech/VisualSkillEditor/wiki/编辑器帮助)
> 简易的编辑器手册

- [技能结构和指令集](https://github.com/river-li-tech/VisualSkillEditor/wiki/技能结构和指令集)
> 介绍技能段落、指令集和相关的关键设计

- [技能运行时](https://github.com/river-li-tech/VisualSkillEditor/wiki/技能运行时)
> 技能解析器和执行引擎


## 编译

下载已生成的版本：
[Win32版本](https://github.com/river-li-tech/VisualSkillEditor/tree/master/Versions)

*****
Visual Studio编译：
1. 安装环境，参考[Qt环境搭建(Visual Studio)](https://blog.csdn.net/liang19890820/article/details/49874033)
2. Visual Studio打开工程文件 SkillEditor.sln。
3. 编译工程，输出到 Bin 目录。
4. (可选)如果需要迁移到其他环境运行，执行部署文件 Bin/Deploy.cmd (需指定Qt安装目录)。

*****
Qt Creator编译：
1. 安装Qt，所有Qt版本[下载](http://download.qt.io/archive/qt/)。
2. Qt打开工程文件 SkillEditor.pro。
3. 编译工程，输出到Bin目录。
4. (可选)如果需要迁移到其他环境运行，执行部署文件 Bin/Deploy.cmd(需指定Qt安装目录)。
