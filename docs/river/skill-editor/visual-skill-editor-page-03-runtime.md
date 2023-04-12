---
tags:
  - 游戏
  - 工具
comments: true
---

**使用xml格式来保存技能逻辑，同时提供一套标准化的解析和执行运行时(额外提供)**

**技能运行时(Runtime) 指一整套的技能加载和运行逻辑结构。作为技能系统的核心，对代码的运行效率、稳健性和扩展能力都有较高的要求。**

实现一套技能运行时往往有几个问题需要提前考虑：
> 1. 你希望技能运行时是运行在服务器or客户端?
> 2. 运行环境是否支持从xml中快速加载数据？是否有高效的xml加载库？
> 3. 运行环境中，内存和Cpu哪个是更稀缺资源？是否需要开启缓存机制？
> 4. 是否需要支持技能文件的热更新和热加载？
> 5. 针对这样的核心代码，是否需要搭建一个单元测试环境？

## 内容列表
- [静态逻辑实现](#静态逻辑实现)
- [获取源码](#获取源码)
- [一种新的技能运行时实现](#一种新的技能运行时实现)

## 静态逻辑实现
以下为一套完整实现的UML类图，如图所示，整个结构划分为3个部分，分别是：

**加载技能数据**
1. 从xml文件中加载技能指令数据，执行语法检查和基础语义分析，得到一个树型的层次结构（有点类似编译系统中的抽象语法树）;                     
2. 这个阶段将确保技能数据配置正确，且无明显的语义错误；             
3. 所有已加载的技能数据将缓存在SkillInstFactory中，提升执行效率；

**技能运行时**
1. 技能的核心部分，以SkillInst为中心，关联技能指令节点（SkillNode）和上下文数据（SkillInstContext）。                    
2. 所有指令有4个执行阶段，分别是：Load（加载）, Prepare（初始化）, Execute（执行）, Finish（结束）                    
3. 上下文数据中保存技能执行过程中的中间数据，也可镜像存储外部环境数据或数据传递的中间过渡区间；                    
4. 提供2个扩展位，分别是继承SkillNode自定义事件指令，继承SkillCond自定义条件指令。继承时只需实现对于回调接口即可(Template Method模式)。

**调用技能**
1. 外部环境只需调用SkillInst上的接口即可驱动所有技能                

![静态逻辑实现](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/runtime.png){ loading=lazy }

## 获取源码
is coming...

## 一种新的技能运行时实现

以上运行时环境适用于多数情况，但某些环境下可能会有特殊的需求和限制，如：
>1.加载或解析XML的耗时严重，或者操作系统载入文件到虚拟内存耗时严重，如Android下加载放置在StreamingAssets中的文件，需要到jar包中异步提取；
>2.内存极度稀缺，无法缓存大量的技能数据；
>3.CPU极度稀缺，以上标准的运行时库仍觉得复杂，想要一套更简单的库；

#### 1.一种类似处理器指令集的实现方式：
在处理器执行CPU指令前，编译器针对抽象语法树，展平其控制语句为顺序指令队列，并在其上完成符号解析和重定位，使得执行引擎只需逐条顺序执行即可，同时无需解析数据（类似汇编代码）。使得后续技能运行时只需逐条顺序执行指令即可，无需管理配置数据和递归执行语法树。将xml文件转换为txt格式的二维顺序指令队列，如下图所示：

![顺序指令文件](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/sequences.png){ loading=lazy }

> 关于CPU指令集可[参考](https://en.wikipedia.org/wiki/Instruction_set_architecture)
> 关于编译器链接过程可[参考](https://en.wikipedia.org/wiki/Linker_(computing)#Relocation)
> 关于CPU流水线可[参考](https://en.wikipedia.org/wiki/Instruction_pipelining)

#### 2.实现过程（所有过程通过技能编辑器工具完成）
**2.1 展平控制语句**

所有if/while/for/switch控制语句都可用goto实现，以下分别是if, do-while, while, for, switch的跳转指令版本：
![控制语句](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/control-flow.png){ loading=lazy }

实现算法可参考编辑器源码 
``` cpp
bool serializeNode(SkillScriptNodePtr nodePtr, SkillScriptNodePtr  nextNodePtr, QList<SkillScriptNodePtr>& list);
```

**2.2 符号解析和重定位**

符号解析：解析所有指令参数和技能动态参数，抽取其值作为指令参数；
重定位：为所有指令生成唯一id（类似绝对地址），同时将跳转指令的目的地址（指令的唯一id）替换为绝对地址（唯一id）；
最终结果如上图txt文件实现算法可参考编辑器源码 
```cpp
void syncCurSkillAction(SkillInstDataPtr instDataPtr);
```

**2.3 技能运行时（Runtime）**

实现方式类似cpu执行指令过程，可将其划分成以下几个阶段：

**取指fetch：**
> 1.根据程序计数器(PC)的值，从指令队列中读取指令行；    
> 2.计算下一指令的地址

**译码decode：**   
> 1.解析指令行数据，定位到具体指令执行体执行execute：    

**执行指令体：**
> 1.更新状态数据到上下文结构中；    
> 2.对于传送指令和跳转指令，可能更新下一指令地址；

**更新 update：**
> 1.计算下一条指令的地址

此算法非常简单，代码不超过200行，以下是截取的取指过程(C#实现)
![runtime实现](https://river-li-tech.github.io/mkdocs/river/skill-editor/visualskilleditor/runtime-code.png){ loading=lazy }

**2.4 更进一步的优化**
将顺序指令队列保存为二进制格式，可使得指令的加载速率更高，同时，可避免文件中大量不必要的空数据。
