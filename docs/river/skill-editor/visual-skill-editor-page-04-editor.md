---
tags:
  - 游戏
  - 工具
comments: true
---

**可视化编辑，在编辑器中拖拽指令和控制结构，一目了然**

## 内容列表

- [内容列表](#内容列表)
- [主界面](#主界面)
- [指令集描述文件](#指令集描述文件)

## 主界面
![主界面](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/editor-main.png){ loading=lazy }

编辑界面中各部分的作用如下：
- **技能列表：**
从SkillInst.txt表格中载入所有技能行，可快速索引；

- **技能树：**
单个技能的xml文件，表示为树形结构，可增删节点并调整层次关系；

- **指令参数：**
单个技能节点的参数信息，所有参数从配置文件(SkillConfig.xml)中获取其数值范围，并可索引动态参数；

- **技能动态参数：**
独立与技能逻辑的一些数据，用于实现将技能逻辑和数据分离，数据按技能id存储在SkillInst.txt的动态参数区；


## 指令集描述文件
[指令集描述文件](https://github.com/river-li-tech/VisualSkillEditor/blob/master/Bin/Config/SkillSpec.xml)
>此文件既作为指令参数说明文档，也作为编辑器的界面配置文件，开发者需要增加指令时可自由编辑。
