# 程序简介

这是一个简单的人工智能单机斗地主游戏

# 程序的实现

## 程序中使用到的类

#### Card：一副扑克牌。

    接口：洗牌、抹牌及剩余牌数。
    
#### CardGroup：一组牌型，如：单张、对子、顺子、炸弹等等。

    属性：包含牌序号集合、对应牌的权值集合、该组牌的类型、权值、牌的数量；
    接口：添加/删除序号牌、重置结构内属性、静态序号到权值转换。
    
#### Player：玩家

    属性：手牌集合、手牌牌型集合、选牌集合、出牌集合、是否不出牌、玩家总分数；
    接口：包括分析叫地主分数、分析手牌、选牌、分析是否出牌（或跟牌）等。
    
#### Game：游戏主程序

    属性：玩家、地主方、当前出牌方、本局基本分、倍率、地主专属牌集合等；
    接口：相关控制游戏进行函数，及没个步骤通知界面更新。
    
#### Scene：游戏界面

    包含游戏界面元素及游戏主界面缓冲去生成及窗口绘制等功能。
    
## 人工智能部分实现

#### 分析选牌牌型

1. 先分析选牌是否为王炸，如不是则进行第二步；
2. 找出选牌中相同权值的最大数量；
3. 根据最大数量判断是否为相应的有效牌型。

#### 分析手牌并拆分成有效牌型

1. 判断牌型集合是否为空，不为空就不需要分析直接返回，否则进行第二步；
2. 由当前玩家手牌（0-53格式）生成权值牌集合（3-17格式）；
3. 先拆分出权值牌集合中的基本牌型，分析的顺序为：王炸 → 炸弹 → 单顺 → 双顺 → 三顺、三条 → 一对 → 单张；
4. 然后又基本牌型尝试组合成更复杂的牌型：三带一、四带二、飞机等。

#### 出牌/跟牌分析

1. 是否需要重新分析手牌，分析完成后进行第2步；
2. 直接出牌（最后出牌方为自己）；

    *	如手牌数为2，则先出王炸、炸弹、数量最多、权值最大的牌；
    *	如下家为我方，其手牌数为一，则出最小单牌；如下家为敌方，其手牌数为一，尽量不出单牌，或出最大单牌；
    *	其他正常出牌顺序为：单牌(A以上的牌尽量不直接出)→对子→双顺→单顺→三条、三带一、飞机

3.	跟友方牌（最后出牌方为我方）

    *	手牌把数≤2，应出对应牌或炸弹，否则过牌
    *	上家为地主且未跟牌，过牌
    *	有对应牌，且权值小于14，则跟，炸弹不跟；
    
4.	跟敌方牌（最后出牌方为敌方）
    *	有对应牌就跟，
    *	没有就拆，
    *   再着用炸弹，否则就过牌

    ##### 拆牌原则：
```
    单牌时：
    
        1.拆单顺数量大于5的
        2.拆三条
        3.拆对
        
    对子时：
    
        1.拆三条
        2.拆三顺数量大于3的
    
    单顺：
    
        1.拆更长单顺
    
    三条或三带一：
    
        1.拆三顺（先判断数量大于3的）
    
    飞机：
    
        1.拆三顺数量大、权值更大的
```
`拆牌后一定要清空牌型集合`

# License
[MIT](https://github.com/songbaoming/DouDiZhu/blob/master/LICENSE)
