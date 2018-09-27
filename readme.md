

![](http://p20tr36iw.bkt.clouddn.com/py_back.jpg)


# LeetCode攀登之旅(3)

## 0.说在前面

学习来源于gitchat王晓华的算法课。

自己实现后面的实例算法(比如：0-1背包问题)

## 1.如何玩算法？

### 1.1 学习要点

玩算法需要做到三点：

【**第一点**】

对遇到的特殊问题能够自己设计出算法实现(可以是智力游戏题目或者工作中的实际问题等)

【**第二点**】

对原理公开的知名算法予以实现(如大整数乘法的Karatsuba算法)

【**第三点**】

对已有具体实现的算法，要能够设计出合适的数学模型，将算法应用到实际问题中(如遗传算法，机器学习算法应用于工业中等)

要掌握这三点，除了掌握基础算法之外，还需要了解算法设计的常用思想和模式，并且要掌握将题目转成数据模型，并进一步用数据结构实现数据模型的一般方法。

### 1.2 数据模型

一个完整的算法实现应该包含三个重要的组成部分，即**数据模型**、**算法逻辑主体**和**输入输出**。

**输入**就是把自然语言描述的问题转化成计算机能存储或处理的数据，并存入数据模型中；

**输出**就是将计算机处理后的结果（也在数据模型中定义）转化成人类能理解的方式输出。

**逻辑主体**就是具体承载数据处理的代码流程，负责对数据模型中的输入数据进行处理、转换，并得到结果。

这三个组成的**核心**是**数据模型**，好的数据模型不仅能准确地描述问题，还能简化算法实现或提高算法的效率，不好的数据模型可能会导致算法的实现困难、效率低下，甚至无法实现算法。

**根据问题的描述建立数据模型的能力是“玩”算法的关键**。

### 1.3 数据模型深入

【**信息数字化**】

信息数字化就是把自然语言描述的信息，转化成方便代码数据模型表达的数字化信息，这是各种问题建模的一个通用思考方向。

来看一个完整的例子：警察抓了 A、B、C、D 四名罪犯，其中一名是小偷，审讯的时候：

```python
A说：“我不是小偷。”    x !=0
B说：“C 是小偷。”     x = 2
C说：“小偷肯定是 D。”  x = 3 
D说：“C 是在冤枉人。”  x != 3
```

现在已经知道四个人中三个人说的是真话，一个人说了假话，请判断一下到底谁是小偷？

对于这个问题转化为数字化的结果则是：

A、B、C、D 四个人分别按照0~3编号，若描述为真，则结果是1，如果是假，则结果是0，假设小偷的编号是x，对于这四个人的描述，数字化的结果是：

```python
a = 1 if x!=0 else 0
b = 1 if x==2 else 0
c = 1 if x==3 else 0
d = 1 - c
```

算法的实现就是：

```python
def who_is_thief():
    for x in range(4):
        a = 1 if x != 0 else 0
        b = 1 if x == 2 else 0
        c = 1 if x == 3 else 0
        d = 1 - c
        if a + b + c + d == 3:
            thief = x
            if thief == 0:
                thief = 'A'
            elif thief == 1:
                thief = 'B'
            elif thief == 2:
                thief = 'C'
            elif thief == 3:
                thief = 'D'
            print("This thief is " + thief)
            break
who_is_thief()
```

输出结果是：

```python
This thief is C
```

很多情况下，信息数字化是建立数据模型的基础。数字化后的数据和数据模型是相辅相成的两个东西，先要知道有什么数据，才能设计相应的数据模型存储和表达这些数据，而好的数据模型不仅有利于数据的存储和访问，也有利于算法的高效实现。

【**类比和转化**】

这里说的类比和转化简言之就是：当我们解决未知的问题时，常常把已知的旧问题当作基础或经验来源。正如艾萨克·牛顿说的那样：“如果我看得比别人远，那是因为我站在巨人的肩膀上。”从根本上讲，把未知的问题转化成已知问题，然后再用已知的方法解决已知问题，是解决未知问题的基础手段。

**以一个项目管理问题为例**

一个工程项目经过层层结构分解最终得到一系列具体的活动，这些活动之间往往存在复杂的依赖关系，如何安排这些活动的开始顺序，使得项目能够顺利完成是个艰巨的任务。

但是如果能把这个问题转化成有向图，图的顶点就是活动，顶点之间的有向边代表活动之间的前后关系，则只需要使用简单的有向图拓扑排序算法就可以解决这个问题。

顶点代表事件，边代表活动，边的权代表活动时间，则可以利用有向图的关键路径算法来解决问题。

## 2. 贪婪法

### 2.1 基本思想

贪婪法（Greedy Algorithm），又称贪心算法，是寻找最优解问题的常用方法，这种方法模式一般将求解过程分成若干个步骤，但每个步骤都应用贪心原则，选取当前状态下最好的或最优的选择（局部最有利的选择），并以此希望最后堆叠出的结果也是最好或最优的解。贪婪法的每次决策都以当前情况为基础并根据某个最优原则进行选择，不从整体上考虑其他各种可能的情况。

贪婪法的基本设计思想有以下三个步骤：

- 建立对问题精确描述的数学模型，包括定义最优解的模型；
- 将问题分解为一系列的子问题，同时定义子问题的最优解结构；
- 应用贪心原则确定每个子问题的局部最优解，并根据最优解的模型，用子问题的局部最优解堆叠出全局最优解。

### 2.2 背包问题

本次例子为0-1背包问题：有 N 件物品和一个承重为 C 的背包（也可定义为体积），每件物品的重量是 wi，价值是 pi，求解将哪几件物品装入背包可使这些物品在重量总和不超过 C 的情况下价值总和最大。这个问题隐含了一个条件，每个物品只有一件，也就是限定每件物品只能选择 0 个或 1 个，因此又被称为 0-1 背包问题。

来看一个具体的例子，有一个背包，最多能承载重量为 C=150 的物品，现在有 7 个物品（物品不能分割成任意大小），编号为 1~7，重量分别是 wi=[35、30、60、50、40、10、25]，价值分别是 pi=[10、40、30、50、35、40、30]，现在从这 7 个物品中选择一个或多个装入背包，要求在物品总重量不超过 C 的前提下，所装入的物品总价值最高。

针对这个问题，有三种贪婪策略的选择问题。

第一：根据最小重量贪心策略，这个策略每次选择最小重量的物品，最终选择装入背包的物品编号依次是 4、2、6、5，此时包中物品总重量是 130，总价值是 165；

第二：根据最大价值贪心策略，这个策略每次都选择重量最轻的物品，根据这个策略最终选择装入背包的物品编号依次是 6、7、2、1、5，此时包中物品总重量是 140，总价值是 155；

第三：根据取最大价值密度贪心策略，这个策略是定义一个价值密度的概念，每次选择都选价值密度最高的物品，物品的价值密度 si 定义为 pi/wi，这 7 件物品的价值密度分别为 si=[0.286、1.333、0.5、1.0、0.875、4.0、1.2]。根据这个策略最终选择装入背包的物品编号依次是 6、2、7、4、1，此时包中物品的总重量是 150，总价值是 170。

【**数据定义**】

```python
# 0-1背包问题的实现
class Goods:
    def __init__(self, weight, value, status):
        # 物品的重量
        self.weight = weight
        # 物品的价值
        self.value = value
        # 0表示未放入包,1表示已经放入包
        self.status = status
```

【**三种策略定义**】

```python
class Greedy(object):
    def greed(self,goods,total,parameter=None):
        result = []
        sum_weight = 0
        sum_value = 0
        while True:
            s = self.strategy(goods, total,parameter)
            if s == -1:
                break
            sum_weight += goods[s].weight
            sum_value += goods[s].value
            if parameter == 'weight':
                result.append(goods[s].weight)
                total = total - goods[s].weight
                goods[s].status = 1
                goods.pop(s)
            elif parameter == 'value':
                result.append(goods[s].value)
                total = total - goods[s].weight
                goods[s].status = 1
                goods.pop(s)
            elif parameter == 'si':
                # 保留两位有效数字
                res = round(goods[s].value / goods[s].weight,2)
                result.append(res)
                total = total - goods[s].weight
                goods[s].status = 1
                goods.pop(s)
        return result,sum_weight,sum_value

    def strategy(self,goods,total,parameter=None):
        index = -1
        minWeight = goods[0].weight
        maxValue = goods[0].value
        maxSi = maxValue / minWeight
        for i in range(0, len(goods)):
            currentGood = goods[i]
            if parameter == 'weight':
                if currentGood.status == 0 and currentGood.weight <= total and currentGood.weight <= minWeight:
                    index = i
                    minWeight = goods[index].weight
            elif parameter == 'value':
                if currentGood.status == 0 and currentGood.weight <= total and currentGood.value > maxValue:
                    index = i
                    maxValue = currentGood.value
            elif parameter == 'si':
                if currentGood.status == 0 and currentGood.weight <= total and currentGood.value / currentGood.weight > maxSi:
                    index = i
                    maxSi = currentGood.value / currentGood.weight
                if currentGood.value / currentGood.weight <= maxSi and currentGood.weight==total:
                    index = i
        return index
```

【**三种策略调用**】

```python
if __name__ == '__main__':
    parameter_list = ['weight','value','si']
    for parameter in parameter_list:
        goods = [Goods(35, 10, 0), Goods(30, 40, 0), Goods(60, 30, 0), Goods(50, 50, 0),
                 Goods(40, 35, 0), Goods(10, 40, 0), Goods(25, 30, 0)]
        g = Greedy()
        result, sum_weight, sum_value = g.greed(goods, 150, parameter=parameter)
        if parameter == 'weight':
            print("--------------按照取最小重量贪心策略--------------")
            print("最终总重量为：" + str(sum_weight))
            print("最终总价值为：" + str(sum_value))
            print("重量选取依此选择为：", end='')
            print(result)
        elif parameter == 'value':
            print("--------------按照取最大价值贪心策略--------------")
            print("最终总重量为：" + str(sum_weight))
            print("最终总价值为：" + str(sum_value))
            print("价值选取依此选择为：", end='')
            print(result)
        elif parameter == 'si':
            print("--------------按照取最大价值密度贪心策略--------------")
            print("最终总重量为：" + str(sum_weight))
            print("最终总价值为：" + str(sum_value))
            print("密度选取依此选择为：", end='')
            print(result)
```

【**三种策略实现结果**】

```python
--------------按照取最小重量贪心策略--------------
最终总重量为：140
最终总价值为：155
重量选取依此选择为：[10, 25, 30, 35, 40]
--------------按照取最大价值贪心策略--------------
最终总重量为：130
最终总价值为：165
价值选取依此选择为：[50, 40, 40, 35]
--------------按照取最大价值密度贪心策略--------------
最终总重量为：150
最终总价值为：170
密度选取依此选择为：[4.0, 1.33, 1.2, 1.0, 0.29]
```

看起来第三种策略取得了最好的结果，和动态规划方法得到的最优结果是一致的，但是实际上，这只是对这组数据的验证结果而已，如果换一组数据，结果可能完全相反。当然，对于一些能够证明贪婪策略得到的就是最优解的问题，应用贪婪法可以高效地求得结果，比如求最小生成树的 Prim 算法和 Kruskal 算法。

在大多数情况下，贪婪法受自身策略模式的限制，通常很难直接求解全局最优解问题，也很难用于多阶段决策问题。贪婪法只能得到比较接近最优解的近似的最优解，但是作为一种启发式辅助方法在很多算法中都得到了广泛的应用，很多常用的算法在解决局部最优决策时，都会应用到贪婪法。比如 Dijkstra 的单源最短路径算法在从 dist 中选择当前最短距离的节点时，就是采用的贪婪法策略。事实上，在任何算法中，只要在某个阶段使用了只考虑局部最优情况的选择策略，都可以理解为使用了贪婪算法。

