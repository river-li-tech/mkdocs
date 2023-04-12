---
tags:
  - 游戏
  - 工具
comments: true
---

**提供30+条指令，全部为瞬发指令，基本满足正交性，各自独立且便于扩展**

## 内容列表

- [内容列表](#内容列表)
- [技能段落](#技能段落)
- [技能指令](#技能指令)
- [参数说明](#参数说明)
- [关键设计：](#关键设计)
  - [如何控制时间](#如何控制时间)
  - [如何表达条件控制语句](#如何表达条件控制语句)
  - [如何实现goto到自定义的section](#如何实现goto到自定义的section)
  - [如何实现并行逻辑](#如何实现并行逻辑)

## 技能段落

技能的逻辑划分成7种段落(section)，分别是：
* 段落-开始：onstart，表示技能起始，只执行一次，完成后才开始执行其它段落；
* 段落-帧触发：onframe，每帧触发，可设置最长总持续时间；
* 段落-间隔触发：ontick，每间隔固定时长触发，可设置最长总持续时间；
* 段落-结束：onfinish，技能结束触发，基本被中断也仍旧会触发结束段落；
* 段落-中断：onbreak，技能被中断时触发，中断后会触发结束段落；
* 段落-自定义段落：id可自定义，通过gotosection触发调用，相当于过程调用；
* 段落-自定义事件：id可自定义，通过sendevent触发，与其他段落并发执行；

![段落结构](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/guild-sections.png){ loading=lazy }

## 技能指令

**事件指令**抽象出技能逻辑中需要执行的原子事件，基本满足正交性。在指令粒度和便利性上做了一定程度的折中，以后者作为第一原则，但整体上已避免冗余。
将所有事件指令划分为7个指令组，分别是：
* 指令-控制：用于实现控制逻辑，如流程控制，时间控制，逻辑跳转等；
* 指令-表现：客户端表现相关指令，涉及动画、特效、相机、声音、子弹时间效果等；
* 指令-移动：位置和朝向相关指令，既包括移动和转向指令，也包括选点和计算朝向；
* 指令-扫描：各种扫描指令，提供多种范围搜索指令；
* 指令-角色：角色操作指令；
* 指令-效果：操作效果(impact)；
* 指令-工具：辅助指令，只用于编辑器；

![事件指令列表1](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/guild-actions1.png){ loading=lazy }
![事件指令列表1](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/guild-actions2.png){ loading=lazy }

**条件指令**抽象出逻辑控制中可能的一些判断条件，用于运行时修改技能的执行轨迹。
>条件指令只返回true/false，供事件指令中的控制指令使用；
>只对个别必需的条件才独立为指令，因此条件指令的数目需要严格控制。
在项目实践中发现，多数策划难以驾驭过于复杂的控制逻辑，所以在设计中倾向于将条件内置到事件指令中（作为其一个参数）。

![条件指令列表](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/guild-conds.png){ loading=lazy }

## 参数说明

每个指令都具有多个参数，通过key-value的方式描述。以scancircle(圆形扫描)为例：
![参数说明](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/guild-params.png){ loading=lazy }
如图所示，参数中规定了此指令的数据类型和范围，以及是否需要强制配置等信息

以上截图来自[指令集描述文件](https://github.com/river-li-tech/VisualSkillEditor/blob/master/Bin/Config/SkillSpec.xml)
>此文件既作为指令参数说明文档，也作为编辑器的界面配置文件，开发者需要增加指令时可自由编辑。


## 关键设计：

### 如何控制时间
> 可使用绝对时间（相对与技能起始时间），也可以使用相对时间（相对于前一个指令）。
两者没有本质区别，但个人推荐后者，虽然绝对时间在执行指令时逻辑简单，但在存在条件分支时后续技能无法自动提前后延后。
更重要的，一旦需要实现子弹时间（慢速特写）功能时，相对时间是唯一的选择。只需提供一条wait(等待一段时间)指令即可实现时间控制功能，当然也提供waitfinish(等待上一条指令完成)用于提供动态效果。

### 如何表达条件控制语句
> 条件语句本身就是一个xml中的一个节点，但如何表示条件却又两种方式：一是将条件保存为节点的属性，这种方式可行且简洁，但条件参数含义难以自描述（只能使用通用属性）；另一种方式是将条件保存为节点的子节点，我这在这选择此方式，因为子节点的方式更有利于扩展和组织。
> 一个if/elseif/else的结构表示为（对应为select指令）:
> ![select语句](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/select.png){ loading=lazy }

### 如何实现goto到自定义的section
> 自定义section放入section节点，如上图“技能逻辑结构”所示。通过gotosection(跳转到段落)指令实现执行流的跳转（原执行流的逻辑不再执行）。
通过以下指令可跳转到自定义逻辑；
``` xml
<action id="gotosection" eventid="sec_name"/>
```
> 自定义段落有多种用途，相当于语言中中的函数或过程，即可表达跳转的逻辑，也可复用局部逻辑；

### 如何实现并行逻辑
> 有时候希望技能以多条时间轴并行方式运行，如下图所示：
> ![并行](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/concurrent.png){ loading=lazy }
> sendevent(触发事件)指令即可实现此功能，将需要并发运行的逻辑组织为一个on节点，如上图“技能逻辑结构”所示。此指令也可用于向游戏发送各种指令，触发其它游戏系统，如场景效果、剧情逻辑等；通过以下指令可并行地触发并行逻辑；
```xml
<action id="sendevent" eventid="evt_name"/>
```