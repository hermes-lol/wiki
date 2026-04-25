---
crawl_time: '2026-01-17 14:25:14'
framework: sphinx
title: 15 随机服务系统模拟 | 统计计算
url: https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-des.html
---

# [统计计算](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/)

# 15 随机服务系统模拟

## 15.1 概述

我们在第[10](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-intro.html#sim-intro)章中讲到，
较为复杂的随机模型往往难以进行彻底的理论分析，
这时常常使用随机模拟方法产生模型的大量数据，
从产生的数据对模型进行统计推断。
随机服务系统就是这样的一种模型，
经常需要利用随机模拟方法进行研究。

随机服务系统在我们日常生活、工业生产、科学技术、军事领域中是经常遇到的随机模型，
比如，研究银行、理发店、商店、海关通道、高速路收费口等服务人员个数的设置和排队规则，
研究计算机网络网关、移动网络的调度规则，等等。

在概率统计理论中排队论用来研究随机服务系统的数学模型，
可以用来设计适当的服务机构数目和排队规则。
如下面的M/M/1排队系统。

**例15.1** 设某银行仅有一个柜员，
并简单假设银行不休息。
顾客到来间隔的时间服从独立的指数分布Exp(\(\lambda\))
(\(1/\lambda\)为间隔时间的期望值),
如果柜员正在为先前的顾客服务,
新到顾客就排队等待，
柜员为顾客服务的时间服从均值为\(1/\mu\)的指数分布，
设\(u = \lambda / \mu < 1\)。
设\(X\_t\)表示\(t\)时刻在银行内的顾客数（包括正在服务的和正在排队的），
则\(X\_t\)是一个连续时马氏链。

这是一个生灭过程马氏链，
有理论结果表明当系统处于稳定状态时
\[\begin{aligned}
P(X\_t = i) = u^i (1-u),
\ i=0,1,2,\dots.
\end{aligned}\]
设随机变量\(N\)服从\(X\_t\)的平稳分布。
于是银行中平均顾客数为
\[\begin{aligned}
E N = \frac{u}{1-u},
\end{aligned}\]
平均队列长度\(EQ\)等于\(E N\)减去平均正在服务人数，
正在服务人数\(Y\_t\)为
\[\begin{aligned}
Y\_t = \begin{cases}
1, & \text{当 } X\_t > 0, \\
0, & \text{当 } X\_t = 0
\end{cases}
\end{aligned}\]
所以\(E Y\_t = P(Y\_t = 1) = 1 - P(N = 0) = u\),
于是平均队列长度为
\[\begin{aligned}
E Q = E N - E Y\_t = \frac{u}{1-u} - u = \frac{u^2}{1-u},
\end{aligned}\]
设顾客平均滞留时间为\(E R\)，
由关系式
\[\begin{aligned}
E N = \lambda \cdot E R
\end{aligned}\]
可知平均滞留时间为
\[\begin{aligned}
E R = \frac{u}{\lambda(1-u)} = \frac{1}{\mu - \lambda},
\end{aligned}\]
进一步分析还可以知道顾客滞留时间\(R\)服从均值为\(1/(\mu-\lambda)\)的指数分布。

※※※※※

从上面的例子可以发现，
一个随机服务系统的模型应该包括如下三个要素：

* 输入过程: 比如，银行的顾客到来的规律。
* 排队规则: 比如，银行有多个柜员时，
  顾客是选最短一队，
  还是随机选一队，
  还是统一排队等候叫号，
  顾客等得时间过长后是否会以一定概率放弃排队。
* 服务机构：有多少个柜员，
  服务时间的分布等。

虽然某些随机服务系统可以进行严格理论分析得到各种问题的理论解，
但是，随机服务系统中存在大量随机因素，
使得理论分析变得很困难以至于不可能。
例如，即使是上面的银行服务问题，
可能的变化因素就包括：
顾客到来用齐次泊松过程还是非齐次泊松过程，
柜员有多少个，
是否不同时间段柜员个数有变化，
柜员服务时间服从什么样的分布，
顾客排队按照什么规则，
是否VIP顾客提前服务，
顾客等候过长时会不会放弃排队，
等等。
包含了这么多复杂因素的随机服务系统的理论分析会变得异常复杂，
完全靠理论分析无法解决问题，
这时，可以用随机模拟方法给出答案。

在模拟随机服务系统时，
可以按时间顺序记录发生的事件，
如顾客到来、顾客接受服务、顾客结束服务等，
这样的系统的模拟也叫做**离散事件模拟**(Discrete Event Simulation, DES)。

离散事件模拟算法可以分为三类：

* 活动模拟，把连续时间离散化为很小的单位，
  比如平均几秒发生一个新事件时可以把时间精确到毫秒，
  然后时钟每隔一个小的时间单位前进一步并查看所有活动并据此更新系统状态。
  缺点是速度太慢。
* 事件模拟。仅在事件发生时更新时钟和待发生的事件集合。
  这样的方法不受编程语言功能的限制，
  运行速度很快，也比较灵活，可以实现复杂逻辑，
  但是需要自己管理的数据结构和逻辑结构比较复杂，
  算法编制相对较难。
* 过程模拟。
  把将要到来的各种事件看成是不同的过程，
  让不同的过程并行地发生,
  过程之间可以交换消息，
  并在特殊的软件包或编程语言支持下自动更新系统状态。
  在计算机实现中需要借助于线程或与线程相似的程序功能。
  这类软件包有C++语言软件包C++SIM和Python语言软件包SimPy,
  R的simmer包，Julia的SimJulia包。
  优点是系统逻辑的编码很直观，程序模块化，
  需要用户自己管理的数据结构和逻辑结构少。
  过程模拟是现在更受欢迎的离散事件模拟方式。

事件模拟算法必须考虑的变量包括：

* 当前时刻\(t\);
* 随时间而变化的计数变量，
  如\(t\)时刻时到来顾客人数、已离开人数；
* 系统状态，
  比如是否有顾客正在接受服务、排队人数、队列中顾客序号。

这些变量仅在有事件发生时（如顾客到来、顾客离开）才需要记录并更新。
为了持续模拟后续事件，
需要维护一个将要发生的事件的列表（下一个到来时刻、下一个离开时刻），
列表仅需要在事件发生时进行更新。
其它变量可以从这三种变量中推算出来，比如，
顾客\(i\)在时间\(t\_1\)时到达并排队，
在时间\(t\_2\)时开始接受服务，
在时间\(t\_4\)时结束服务离开，
则顾客\(i\)的滞留时间为\(t\_4 - t\_1\)。

**例15.2** 用事件模拟的方法来模拟例[15.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-des.html#exm:sim-des-mm1)。
目的是估计平均滞留时间\(E R\)。

想法是，模拟生成各种事件发生的时间，
模拟很长时间，
丢弃开始的一段时间\(T\_0\)后，
用\(T\_0\)后到达的顾客的总滞留时间除以\(T\_0\)
后到达的顾客人数来估计平均滞留时间。
设\(T\_0\)时间后每位顾客滞留时间的模拟值为
\(R\_i, i=1,2,\dots,m\)，
可以用\(\{ R\_i \}\)作为随机变量\(R\)的样本来检验\(R\)的分布是否指数分布。

用事件模拟方法进行离散事件模拟的算法关键在于计算系统状态改变的时间，
即各个事件的发生时间，
这个例子中就是顾客到来、顾客开始接受服务、顾客离开这样三种事件,
由此还可以得到每个顾客排队的时间和服务的时间。

在没有明确算法构思时，
可以从时间0开始在纸上按照模型规定人为地生成一些事件并人为地找到下一事件。
这样可以找到要更新的数据结构和更新的程序逻辑。

下面的算法保持了一个将要发生的事件的集合，
在每次事件发生时更新时钟，
更新时钟时从事件集合中找到最早发生的事件进行处理，
并生成下一事件到事件集合中，
如此重复直到需要模拟的时间长度。

| M/M/1 事件模拟算法 |
| --- |
| {初始化当前时钟\(t \leftarrow 0\), 柜员忙标志\(B \leftarrow 0\), 当前排队人数\(L \leftarrow 0\), |
| 最新到来顾客序号\(i \leftarrow 0\), 正在服务顾客序号\(j \leftarrow 0\), 已服务顾客数\(n \leftarrow 0\) } |
| 从Exp(\(\lambda\))抽取\(X\), 设置下一顾客来到时间\(A \leftarrow X\) |
| **repeat** { |
| \(\quad\) **if**(\(B = 0\) **or** (\(B = 1\) **and** \(A < E\))) { # \(E\)是正在服务的顾客结束时刻 |
| \(\qquad\) \(t \leftarrow A\) |
| \(\quad\) } **else** { |
| \(\qquad\) \(t \leftarrow E\) |
| \(\quad\) } |
| \(\quad\) **if** (\(t > T\_1\)) **break**  # \(T\_1\)是预先确定的模拟时长 |
| \(\quad\) |
| \(\quad\) **if**(\(t == A\)) { # 待处理到达事件 |
| \(\qquad\) \(L \leftarrow L+1\) |
| \(\qquad\) \(i \leftarrow i+1\), 记录第\(i\)位顾客到来时间\(a\_i \leftarrow t\) |
| \(\qquad\) 从Exp(\(\lambda\))抽取\(X\), \(A \leftarrow t + X\) |
| \(\qquad\) **if**(\(B==0\) ) { # 不用排队，直接服务 |
| \(\qquad\quad\) \(B \leftarrow 1\), \(L \leftarrow L-1\) |
| \(\qquad\quad\) \(j \leftarrow j+1\), 置第\(j\)位顾客开始服务时间\(s\_j \leftarrow t\) |
| \(\qquad\quad\) 从Exp(\(\mu\))抽取\(Y\), 置\(E \leftarrow t + Y\) |
| \(\qquad\) } |
| \(\quad\) } **else** { # 待处理结束服务事件 |
| \(\qquad\) \(B \leftarrow 0\) |
| \(\qquad\) \(n \leftarrow n+1\), 记录第\(n\)个顾客结束服务时间\(e\_n \leftarrow t\) |
| \(\qquad\) **if**(\(L>0\)) { # 排队顾客开始服务 |
| \(\qquad\quad\) \(L \leftarrow L-1\) |
| \(\qquad\quad\) \(B \leftarrow 1\) |
| \(\qquad\quad\) \(j \leftarrow j+1\), \(s\_j \leftarrow t\) |
| \(\qquad\quad\) 从Exp(\(\mu\))抽取\(Y\), 置\(E \leftarrow t + Y\) |
| \(\qquad\) } |
| \(\quad\) } |
| } |
| 令\(I = \{ i:\ T\_0 \leq s\_i \leq T\_1 \}\), 求\(\{ e\_i - a\_i, \ i \in I \}\)的平均值作为\(ER\)估计 |

从这个算法可以看出，
事件模拟方法需要用户自己管理待处理事件集合与时钟,
算法设计难度较大。

※※※※※

用随机服务系统进行建模和模拟研究的一般步骤如下：

* 提出问题。比如，银行中顾客平均滞留时间与相关参数的关系。
* 建立模型。比如，设顾客到来服从泊松过程，
  设每个顾客服务时间服从独立的指数分布。
* 数据收集与处理。
  比如，估计银行顾客到来速率，
  每个顾客平均服务时间，等等。
* 建立模拟程序。一般使用专用的模拟软件包或专用模拟语言编程。
* 模拟模型的正确性确认。
* 模拟试验。
* 模拟结果分析。

离散事件模拟问题一般都比较复杂，
即使借助于专用模拟软件或软件包，
也很难确保实现的模拟算法与问题的实际设定是一致的。
为此，需要遵循一些提高算法可信度的一般规则。
算法程序一定要仔细检查，
避免出现参数错误、逻辑错误。
在开始阶段，
可以利用较详尽的输出跟踪模拟运行一段时间，
人工检查系统运行符合原始设定。
尽可能利用模块化程序设计，
比如，
在M/M/1问题模拟中，
顾客到来可能遵循不同的规律，
比如时齐泊松过程、非时齐泊松过程，
把产生顾客到来时刻的程序片段模块化并单独检查验证，
就可以避免在这部分出错。
问题实际设定可能比较复杂，
在程序模块化以后，
如果有一种较简单的设定可以得到理论结果，
就可以用理论结果验证算法输出，
保证程序框架正确后，
再利用模块化设计修改各个模块适应实际复杂设定。

## 15.2 用R的simmer包进行随机服务系统模拟(`*`)

R扩展包simmer是仿照python语言的simpy包编写的一个用于离散事件模拟的软件包，
属于过程模拟算法，
利用这样的软件包可以使得离散事件模拟程序变得比较简单。

**例15.3** 用R的simmer包模拟M/M/1随机服务系统。
比较简单的版本，进行短时间的模拟，
用详细过程输出验证算法的合理性。

程序：

```
demo.mm1.simmer1 <- function(T1=20, lambda=1, mu=1.2){
  library(simmer)
  
  ## 建立模拟环境
  sim <- simmer("M/M/1模拟")

  ## 顾客的轨迹
  customer <- trajectory("顾客") |>
    ## 进入系统
    log_("进入银行") |>
    ## 排队或直接服务
    seize("柜员", 1) |>
    log_("开始接受服务") |>
    timeout(function() rexp(1, rate=mu)) |>
    log_("结束服务") |>
    release("柜员", 1)
  
  ## 指定资源和顾客到来
  sim |>
    add_resource("柜员", 1) |>
    add_generator("顾客", customer, function() rexp(1, rate=lambda))
  
  ## 运行
  sim |>
    run(until=T1)
}
```

测试运行：

```
set.seed(1)
sim1 <- demo.mm1.simmer1()
## 0.755182: 顾客0: 进入银行
## 0.755182: 顾客0: 开始接受服务
## 0.876604: 顾客0: 结束服务
## 1.93682: 顾客1: 进入银行
## 1.93682: 顾客1: 开始接受服务
## 2.07662: 顾客2: 进入银行
## 2.30022: 顾客1: 结束服务
## 2.30022: 顾客2: 开始接受服务
## 3.32485: 顾客2: 结束服务
## 4.97159: 顾客3: 进入银行
## 4.97159: 顾客3: 开始接受服务
## 5.51127: 顾客4: 进入银行
## 5.65832: 顾客5: 进入银行
## 5.76873: 顾客3: 结束服务
## 5.76873: 顾客4: 开始接受服务
## 6.40375: 顾客4: 结束服务
## 6.40375: 顾客5: 开始接受服务
## 7.04905: 顾客6: 进入银行
## 7.43509: 顾客5: 结束服务
## 7.43509: 顾客6: 开始接受服务
## 8.31388: 顾客6: 结束服务
## 11.473: 顾客7: 进入银行
## 11.473: 顾客7: 开始接受服务
## 12.5082: 顾客8: 进入银行
## 13.0363: 顾客7: 结束服务
## 13.0363: 顾客8: 开始接受服务
## 13.163: 顾客9: 进入银行
## 13.3171: 顾客8: 结束服务
## 13.3171: 顾客9: 开始接受服务
## 13.7515: 顾客10: 进入银行
## 14.3933: 顾客11: 进入银行
## 14.6875: 顾客12: 进入银行
## 15.2533: 顾客13: 进入银行
## 15.2876: 顾客9: 结束服务
## 15.2876: 顾客10: 开始接受服务
## 15.3371: 顾客10: 结束服务
## 15.3371: 顾客11: 开始接受服务
## 15.3594: 顾客14: 进入银行
## 15.8193: 顾客11: 结束服务
## 15.8193: 顾客12: 开始接受服务
## 16.7971: 顾客12: 结束服务
## 16.7971: 顾客13: 开始接受服务
## 17.6278: 顾客13: 结束服务
## 17.6278: 顾客14: 开始接受服务
## 18.8239: 顾客14: 结束服务
## 19.3183: 顾客15: 进入银行
## 19.3183: 顾客15: 开始接受服务
## 19.3556: 顾客16: 进入银行
## 19.5883: 顾客15: 结束服务
## 19.5883: 顾客16: 开始接受服务
## 19.7579: 顾客16: 结束服务
```

用`get_mon_arrivals()`获取系统监测的事件发生和持续时间，
结果是数据框：

```
sim1 |>
  get_mon_arrivals() |>
  knitr::kable(digits=2)
```

| name | start\_time | end\_time | activity\_time | finished | replication |
| --- | --- | --- | --- | --- | --- |
| 顾客0 | 0.76 | 0.88 | 0.12 | TRUE | 1 |
| 顾客1 | 1.94 | 2.30 | 0.36 | TRUE | 1 |
| 顾客2 | 2.08 | 3.32 | 1.02 | TRUE | 1 |
| 顾客3 | 4.97 | 5.77 | 0.80 | TRUE | 1 |
| 顾客4 | 5.51 | 6.40 | 0.64 | TRUE | 1 |
| 顾客5 | 5.66 | 7.44 | 1.03 | TRUE | 1 |
| 顾客6 | 7.05 | 8.31 | 0.88 | TRUE | 1 |
| 顾客7 | 11.47 | 13.04 | 1.56 | TRUE | 1 |
| 顾客8 | 12.51 | 13.32 | 0.28 | TRUE | 1 |
| 顾客9 | 13.16 | 15.29 | 1.97 | TRUE | 1 |
| 顾客10 | 13.75 | 15.34 | 0.05 | TRUE | 1 |
| 顾客11 | 14.39 | 15.82 | 0.48 | TRUE | 1 |
| 顾客12 | 14.69 | 16.80 | 0.98 | TRUE | 1 |
| 顾客13 | 15.25 | 17.63 | 0.83 | TRUE | 1 |
| 顾客14 | 15.36 | 18.82 | 1.20 | TRUE | 1 |
| 顾客15 | 19.32 | 19.59 | 0.27 | TRUE | 1 |
| 顾客16 | 19.36 | 19.76 | 0.17 | TRUE | 1 |

用`get_mon_resources()`获取系统监测的资源占用情况：

```
sim1 |>
  get_mon_resources() |>
  knitr::kable(digits=2)
```

| resource | time | server | queue | capacity | queue\_size | system | limit | replication |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 柜员 | 0.76 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 0.88 | 0 | 0 | 1 | Inf | 0 | Inf | 1 |
| 柜员 | 1.94 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 2.08 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 2.30 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 3.32 | 0 | 0 | 1 | Inf | 0 | Inf | 1 |
| 柜员 | 4.97 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 5.51 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 5.66 | 1 | 2 | 1 | Inf | 3 | Inf | 1 |
| 柜员 | 5.77 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 6.40 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 7.05 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 7.44 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 8.31 | 0 | 0 | 1 | Inf | 0 | Inf | 1 |
| 柜员 | 11.47 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 12.51 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 13.04 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 13.16 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 13.32 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 13.75 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 14.39 | 1 | 2 | 1 | Inf | 3 | Inf | 1 |
| 柜员 | 14.69 | 1 | 3 | 1 | Inf | 4 | Inf | 1 |
| 柜员 | 15.25 | 1 | 4 | 1 | Inf | 5 | Inf | 1 |
| 柜员 | 15.29 | 1 | 3 | 1 | Inf | 4 | Inf | 1 |
| 柜员 | 15.34 | 1 | 2 | 1 | Inf | 3 | Inf | 1 |
| 柜员 | 15.36 | 1 | 3 | 1 | Inf | 4 | Inf | 1 |
| 柜员 | 15.82 | 1 | 2 | 1 | Inf | 3 | Inf | 1 |
| 柜员 | 16.80 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 17.63 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 18.82 | 0 | 0 | 1 | Inf | 0 | Inf | 1 |
| 柜员 | 19.32 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 19.36 | 1 | 1 | 1 | Inf | 2 | Inf | 1 |
| 柜员 | 19.59 | 1 | 0 | 1 | Inf | 1 | Inf | 1 |
| 柜员 | 19.76 | 0 | 0 | 1 | Inf | 0 | Inf | 1 |

※※※※※

**例15.4** 上一例子的改进。
用R的simmer包模拟一个银行，多名顾客以指数间隔时间到来(速率`lambda`)
有一个服务柜台仅一名柜员，只排一队，
服务时间是速率为`mu`的指数分布随机数。
程序更容易改成其它模拟问题，还增加了一些额外的输出。

R程序：

```
demo.mm1.simmer2 <- function(T1=20, lambda=1.0, mu=1.2){
  library(simmer)

  ## 建立模拟环境
  bank <- simmer("bank")
  
  ## 用trajectory()建立顾客，并指定顾客的一系列活动
  ## seize()获取柜员服务资源，如果正在忙，就进入排队
  ## 服务时间用timeout指定，为了生成多个随机服务时间，
  ## timeout的参数是返回随机服务时间的而函数而不是时间值本身
  customer <- 
    trajectory("顾客") |>
    log_("到达") |>
    set_attribute("到达时刻", function() {now(bank)}) |>
    seize("柜员") |>
    log_(function() paste("开始接受服务，排队时间: ", 
      now(bank) - get_attribute(bank, "到达时刻"))) |>
    timeout( function() rexp(1, mu)) |> 
    release("柜员") |>
    log_(function(attr) {paste("离开: ", now(bank))})
  
  ## 用add_resource生成柜员资源
  ## 用add_generator()生成顾客到来列
  bank |>
    add_resource("柜员") |>
    add_generator("顾客", customer, 
      function() {rexp(1, lambda)} )
  
  ## 用run()执行模拟到指定结束时刻  
  bank |>
    run(until=T1)
}
```

测试:

```
set.seed(1)
sim2 <- demo.mm1.simmer2()
## 0.755182: 顾客0: 到达
## 0.755182: 顾客0: 开始接受服务，排队时间:  0
## 0.876604: 顾客0: 离开:  0.876604105381506
## 1.93682: 顾客1: 到达
## 1.93682: 顾客1: 开始接受服务，排队时间:  0
## 2.07662: 顾客2: 到达
## 2.30022: 顾客1: 离开:  2.3002151337181
## 2.30022: 顾客2: 开始接受服务，排队时间:  0.223595259614148
## 3.32485: 顾客2: 离开:  3.32485017823051
## 4.97159: 顾客3: 到达
## 4.97159: 顾客3: 开始接受服务，排队时间:  0
## 5.51127: 顾客4: 到达
## 5.65832: 顾客5: 到达
## 5.76873: 顾客3: 离开:  5.76872798965007
## 5.76873: 顾客4: 开始接受服务，排队时间:  0.257456738084932
## 6.40375: 顾客4: 离开:  6.40375286920846
## 6.40375: 顾客5: 开始接受服务，排队时间:  0.745435627139374
## 7.04905: 顾客6: 到达
## 7.43509: 顾客5: 离开:  7.43508916148749
## 7.43509: 顾客6: 开始接受服务，排队时间:  0.386036790607969
## 8.31388: 顾客6: 离开:  8.31387513424638
## 11.473: 顾客7: 到达
## 11.473: 顾客7: 开始接受服务，排队时间:  0
## 12.5082: 顾客8: 到达
## 13.0363: 顾客7: 离开:  13.0363492322359
## 13.0363: 顾客8: 开始接受服务，排队时间:  0.528118697642798
## 13.163: 顾客9: 到达
## 13.3171: 顾客8: 离开:  13.3171271292232
## 13.3171: 顾客9: 开始接受服务，排队时间:  0.154149957416097
## 13.7515: 顾客10: 到达
## 14.3933: 顾客11: 到达
## 14.6875: 顾客12: 到达
## 15.2533: 顾客13: 到达
## 15.2876: 顾客9: 离开:  15.2875565067429
## 15.2876: 顾客10: 开始接受服务，排队时间:  1.53609961348276
## 15.3371: 顾客10: 离开:  15.3370891404057
## 15.3371: 顾客11: 开始接受服务，排队时间:  0.943739658913893
## 15.3594: 顾客14: 到达
## 15.8193: 顾客11: 离开:  15.8193495265548
## 15.8193: 顾客12: 开始接受服务，排队时间:  1.13187965742313
## 16.7971: 顾客12: 离开:  16.7971096147388
## 16.7971: 顾客13: 开始接受服务，排队时间:  1.54377422102827
## 17.6278: 顾客13: 离开:  17.627787077442
## 17.6278: 顾客14: 开始接受服务，排队时间:  2.2683790604488
## 18.8239: 顾客14: 离开:  18.8238581969195
## 19.3183: 顾客15: 到达
## 19.3183: 顾客15: 开始接受服务，排队时间:  0
## 19.3556: 顾客16: 到达
## 19.5883: 顾客15: 离开:  19.5883493298518
## 19.5883: 顾客16: 开始接受服务，排队时间:  0.232739934309695
## 19.7579: 顾客16: 离开:  19.7579412882115
```

用`get_mon_arrivals()`获取各个顾客到来的时间、离开时间、活动时间等：

```
sim2 |>
  get_mon_arrivals() |>
  dplyr::mutate(waiting_time = end_time - start_time - activity_time) |>
  knitr::kable(digits=2)
```

| name | start\_time | end\_time | activity\_time | finished | replication | waiting\_time |
| --- | --- | --- | --- | --- | --- | --- |
| 顾客0 | 0.76 | 0.88 | 0.12 | TRUE | 1 | 0.00 |
| 顾客1 | 1.94 | 2.30 | 0.36 | TRUE | 1 | 0.00 |
| 顾客2 | 2.08 | 3.32 | 1.02 | TRUE | 1 | 0.22 |
| 顾客3 | 4.97 | 5.77 | 0.80 | TRUE | 1 | 0.00 |
| 顾客4 | 5.51 | 6.40 | 0.64 | TRUE | 1 | 0.26 |
| 顾客5 | 5.66 | 7.44 | 1.03 | TRUE | 1 | 0.75 |
| 顾客6 | 7.05 | 8.31 | 0.88 | TRUE | 1 | 0.39 |
| 顾客7 | 11.47 | 13.04 | 1.56 | TRUE | 1 | 0.00 |
| 顾客8 | 12.51 | 13.32 | 0.28 | TRUE | 1 | 0.53 |
| 顾客9 | 13.16 | 15.29 | 1.97 | TRUE | 1 | 0.15 |
| 顾客10 | 13.75 | 15.34 | 0.05 | TRUE | 1 | 1.54 |
| 顾客11 | 14.39 | 15.82 | 0.48 | TRUE | 1 | 0.94 |
| 顾客12 | 14.69 | 16.80 | 0.98 | TRUE | 1 | 1.13 |
| 顾客13 | 15.25 | 17.63 | 0.83 | TRUE | 1 | 1.54 |
| 顾客14 | 15.36 | 18.82 | 1.20 | TRUE | 1 | 2.27 |
| 顾客15 | 19.32 | 19.59 | 0.27 | TRUE | 1 | 0.00 |
| 顾客16 | 19.36 | 19.76 | 0.17 | TRUE | 1 | 0.23 |

※※※※※

**例15.5** 用R的simmer包模拟例[15.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-des.html#exm:sim-des-mm1)。
一个银行，多名顾客以指数间隔时间到来(速率`lambda`),
有一个服务柜台仅一名柜员，只排一队,
服务时间是速率为mu的指数分布随机数,
比较平均等待时间与理论值。

```
demo.mm1.simmer3 <- function(T0=0, T1=10000, lambda=1.0, mu=1.2){
  library(simmer)

  ## 建立模拟环境
  bank <- simmer("bank")
  
  ## 用trajectory()建立顾客，并指定顾客的一系列活动
  ## seize()获取柜员服务资源，如果正在忙，就进入排队
  ## 服务时间用timeout指定，为了生成多个随机服务时间，
  ## timeout的参数是返回随机服务时间的而函数而不是时间值本身
  customer <- 
    trajectory("顾客") |>
    seize("柜员") |>
    timeout( function() rexp(1, mu)) |> 
    release("柜员")

  ## 用add_resource生成柜员资源
  ## 用add_generator()生成顾客到来列
  bank |>
    add_resource("柜员") |>
    add_generator("顾客", customer, function() {rexp(1, lambda)} )
  
  ## 用run()执行模拟到指定结束时刻  
  bank |>
    run(until=T1)
  
  ## 用get_mon_arrivals()获取各个顾客到来的时间、离开时间、活动时间等，
  ## 结果是数据框
  resd <- bank |>
    get_mon_arrivals() |>
    dplyr::mutate(
      waiting_time = end_time - start_time - activity_time,
      stay_time = end_time - start_time)
  
  stay_times <- resd |>
    dplyr::filter(start_time >= T0, end_time < T1) |>
    dplyr::select(stay_time) |>
    dplyr::pull()
  
  ER <- mean(stay_times)
  ER.true <- 1/(mu-lambda)
  cat('估计的平均滞留时间ER=', ER,
      ' 期望值=', ER.true, '\n')
  invisible(c(ER.hat=ER, ER.true=ER.true))
}
```

测试:

```
set.seed(1)
demo.mm1.simmer3()
## 估计的平均滞留时间ER= 4.790878  期望值= 5
```

※※※※※

## 15.3 用Julia语言的SimJulia包进行离散事件模拟(`*`)

Julia的[SimJulia](https://benlauwens.github.io/SimJulia.jl/stable/)库是一个模仿Python语言SimPy库的软件包，采用过程模拟方法实现了离散事件模拟功能。

将SimJulia的离散事件模拟看作一场戏剧表演，
需要一个“环境”作为舞台，
演员作为“过程”，用Julia的可恢复函数实现，
用`@resumable`前缀说明，
演员与环境和演员之间的交互用“事件”实现。

过程是一种特殊的函数，
它可以在中间搁置起来，
并在某种信号下恢复运行。
过程可以`@yield`一个事件，
这时过程被暂时搁置，
当它提交的事件被激发时过程从搁置的位置恢复运行。
多个过程可以提交同一事件，
则该事件激发时按序恢复各个过程。
常见的事件是“超时”(timeout)事件，
过程将等待或者保持当前状态指定的时间。
用`@process`命令开始一个过程的事件激发。

过程可以要求某种资源，
并在资源被其它过程占用时搁置起来等待资源，
一旦资源空闲就自动占用资源并恢复运行。

作为例子，
下面是M/M/1问题的SimJulia模拟实现。

```
using ResumableFunctions, SimJulia, Distributions 

# 顾客行为，定义一个过程。
@resumable function customer(
    env::Environment, # 模拟环境
    arrival_time,     # 实际到达时间
    server::Resource, # 要求的柜员 
    id::Integer,      # 编号
    dist_serve::Distribution) # 服务时间分布
    
    # 声明保存统计结果的全局变量
    global time_arr, time_ser, time_lea 

    # 顾客到达事件被触发，显示到达信息
    t0 = now(env)
    DEBUG && println("Customer $id arrived: ", t0)
    
    # 请求柜员资源，如果没有空闲就搁置并等待
    @yield request(server) 
    
    # 获得空闲柜员，恢复运行
    t1 = now(env)
    DEBUG && println("Customer $id entered service: ", t1)
    # 占用柜员随机的服务时间
    @yield timeout(env, rand(dist_serve)) 
    
    # 释放柜员资源，终止服务，离开银行
    @yield release(server) 
    t2 = now(env)
    DEBUG && println("Customer $id exited service: ", t2)

    # 将统计分析需要的数据保存到全局变量数组
    push!(time_arr, t0)
    push!(time_ser, t1)
    push!(time_lea, t2)
end

# 这也是一个过程，用来不停地生成顾客
@resumable function customer_generator(
    env::Environment, 
    T::Number, # 结束时间
    server::Resource, 
    arrival_dist,
    service_dist)

    i = 0 # 顾客编号初值
    arrival_time =  0.0 # 实际到达时间，累计计算
    while true # 不停地生成顾客
        i += 1
        # 生成一个到达间隔时间
        tint = rand(arrival_dist)
        # 注意arrival_time是各个顾客的实际到达时间而不是间隔时间
        arrival_time += tint 
        # 提交一个等待事件，等待过后才添加一个新顾客
        @yield timeout(env, tint)
        # 添加一个新顾客和此新顾客的服务事件
        proc = @process customer(env, 
          arrival_time, server, i, service_dist)
    end
end

# 模拟主控程序。
function mm1_sim(λ = 1.0, μ = 1.2, T=1_000)
    # 随机数分布 
    arrival_dist = Exponential(1 / λ)
    service_dist = Exponential(1 / μ)

    sim = Simulation() # 生成一个模拟控制环境
    server = Resource(sim, 1) # 柜员资源
    
    # 顾客生成来源也是一个过程，不停地生成顾客
    @process customer_generator(sim, T, 
        server, arrival_dist, service_dist)

    # 实际运行模拟直至时间T
    run(sim, T)
end

# 比较模拟的平均等待时间与理论期望等待时间
function mm1_stat(λ = 1.0, μ = 1.2)
    mwait = 1 / (μ - λ)
    arrs = [time_arr, time_ser, time_lea]
    n = minimum(length, arrs)
    arrs = [x[1:n] for x in arrs]
    time_stay = arrs[3] .- arrs[1]
    sampavg = mean(time_stay)
    println("理论期望等待时间=$(mwait)")
    println("模拟估计=$(sampavg)")
end

# 利用全局变量保存结果
time_arr = []
time_ser = []
time_lea = []
DEBUG = false 

mm1_sim(0.2, 0.25, 100_000)
mm1_stat(0.2, 0.25)
## 理论期望等待时间=20.000000000000004 
## 模拟估计=20.850519547614372
```

## 15.4 附录：M/M/1系统事件模拟算法的R语言程序(`*`)

例[15.2](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-des.html#exm:sim-des-mm1sim)可以用R语言直接按照事件模拟法实现。

这是单服务员、泊松到来、指数服务时间、先进先出服务系统。
设`T0`为开始计入统计的时间，
`T1`为结束模拟的时间，
`lambda`为顾客到来速率，
`mu`服务结束速率。

```
demo.mm1 <- function(T0=0, T1=10000, lambda=1, mu=1.2, DEBUG=FALSE){
  ## 初始化
  t <- 0     # 时钟
  B <- FALSE # 柜员忙标志
  L <- 0     # 排队等候人数
  i <- 0     # 当前到达顾客号
  j <- 0     # 正在接受服务顾客号
  n <- 0     # 已结束服务顾客数
  max_events <- T1/lambda*2 # 设置一个较大的时间集合存储空间
  a <- numeric(max_events) # 顾客到来时间的集合
  s <- numeric(max_events) # 顾客开始服务时间的集合
  e <- numeric(max_events) # 顾客结束服务时间的集合
  X <- rexp(1, lambda); A <- X # 下一顾客到来时间
  E <- NA         # 下一个服务结束时间
  
  ## 时钟反复跳到下一事件发生的时间，并处理事件，更新待发生事件集合
  repeat{
    if(DEBUG){
      cat('\n')
      cat('t=', round(t,2), ' B=', as.integer(B), ' L=', L,
          ' i=', i, ' A=', round(A,2),
          ' n=', n, ' E=', round(E,2), 
          ' j=', j, 
          '\n', sep='')
      cat('到达序列', round(a,2), '\n')
      cat('开始序列', round(s,2), '\n')
      cat('结束序列', round(e,2), '\n')
    }
    if(!B || (B && A<E)){ # 不忙，或待处理下一到来事件
      t <- A
    } else {
      t <- E
    }
    if(t > T1) break
    
    if(t == A){ # 待处理到达事件
      L <- L+1
      i <- i+1; a[i] <- t
      X <- rexp(1, lambda); A <- t + X
      if(!B){
        B <- TRUE
        L <- L-1
        j <- j+1; s[j] <- t
        Y <- rexp(1, mu); E <- t + Y
      } # if !B
    } else { # 待处理结束服务事件
      B <- FALSE
      n <- n+1; e[n] <- t
      if(L > 0){
        L <- L-1
        B <- 1
        j <- j+1; s[j] <- t
        Y <- rexp(1, mu); E <- t + Y
      }
    } # end of 待处理结束服务事件
  } # end of repeat
  
  nn <- min(c(i, j, n))
  a <- a[seq(nn)]
  s <- s[seq(nn)]
  e <- e[seq(nn)]
  I <- a >= T0 & a <= T1
  ER <- mean(e[I] - a[I])
  ER.true <- 1/(mu-lambda)
  cat('估计的平均滞留时间ER=', ER,
      ' 期望值=', ER.true, '\n')
  c(ER.hat=ER, ER.true=ER.true)
}
```

测试运行：

```
set.seed(1)
demo.mm1(T1=100000, DEBUG=FALSE)
## 估计的平均滞留时间ER= 4.783766 期望值= 5
## ER.hat ER.true
## 4.783766 5.000000
```

※※※※※

## 15.5 附录：M/M/1系统事件模拟算法的Julia程序(`*`)

```
using Random, Distributions, Statistics

# 定义一个专用的数据类型表示模拟
abstract type DESSim end 
abstract type DESState end

struct DESMM1 <: DESSim
    λ::Real # 顾客到来速率
    μ::Real # 服务结束速率
    make_arrival # 抽取一个到来间隔时间
    make_service # 抽取一个服务持续时间

    function DESMM1(λ, μ)
        ma = () -> rand(Exponential(1/λ))
        ms = () -> rand(Exponential(1/μ))
        new(λ, μ, ma, ms)
    end
end

mutable struct MM1State <: DESState
    model::DESMM1
    t::Real # 当前时钟
    B::Bool # 柜员忙
    L::Integer # 当前排队人数
    i::Integer  # 最新到来顾客序号
    j::Integer  # 正在服务顾客序号
    n::Integer  # 已服务顾客人数
    A::Real  # 下一顾客到来时间
    E::Real  # 下一顾客结束时间
    arr_arrival::Vector{Real} # 各位顾客的到来时间数组
    arr_service::Vector{Real} # 各位顾客的服务开始时间数组
    arr_depart::Vector{Real} # 各位顾客的离开时间数组

    function MM1State(λ, μ) 
        model = DESMM1(λ, μ)
        A = model.make_arrival()
        E = A + model.make_service()

        new(model,
            0, # t
            false, # B
            0, # L 
            0, # i
            0, # j
            0, # n 
            A, 
            E,
            [], # 到来时间数组
            [], # 服务开始时间数组
            []  # 离开时间数组
             )
    end
end 

# 处理到达事件
function arrival_event!(state::MM1State)
    state.L += 1 # 排队人数
    state.i += 1 # 最新到来顾客编号
    # 记录当前新到来顾客的到来时间
    push!(state.arr_arrival, state.t)
    # 生成下一个顾客，但暂不处理此顾客
    state.A = state.t + state.model.make_arrival()

    # 处理与i对应的顾客
    if !state.B # 可以直接服务第i顾客
        state.B = true # 正在服务
        state.L -= 1 # 排队人数
        state.j += 1 # 正在接受服务顾客编号
        # 记录当前服务顾客服务开始时间
        push!(state.arr_service, state.t)
        # 生成正在服务顾客的离开时间
        state.E = state.t + state.model.make_service()
    end 
    # 可以直接服务时处理服务问题，
    # 如果不可以直接服务则等待下一事件

    return state 
end # function arrival_event!

# 处理离开事件
function departure_event!(state::MM1State)
    state.B = false  # 非正在服务
    state.n += 1 # 已服务顾客人数
    # 记录离开顾客的离开时间
    push!(state.arr_depart, state.t)

    # 如果有排队顾客则服务队首顾客
    if state.L > 0 
        state.L -= 1 
        state.B = true 
        state.j += 1 # 正在服务顾客编号
        # 记录正在服务顾客服务开始时间
        push!(state.arr_service, state.t)

        # 生成正在服务顾客的离开时间，但作为下一个事件
        state.E = state.t + state.model.make_service()
    end # if state.L > 0

    return state 
end # function departure_event!


# 从当前状态向前模拟一步的函数
function step!(state::MM1State)
    model = state.model
    λ = model.λ
    μ = model.μ

    if !state.B || (state.B && state.A < state.E)
        # 如果没有顾客，或者有顾客且下一事件是到来 
        state.t = state.A # 下一改变状态的时间
        arrival_event!(state)
    else
        # 下一事件是结束服务 
        state.t = state.E 
        departure_event!(state)
    end 

    return state 
end # function step!


# 函数，输入一个MM1系统参数和要求的时间，进行模拟
# T是要模拟的时间终点
# T0是要抛弃的部分
function MM1_sim(λ=2, μ=4, T=10_000; T0 = 0)
    state = MM1State(λ, μ) # 初始化
    while state.t < T 
        step!(state)
    end

    # 将到来时间、开始服务时间、离开时间的顾客编号对齐，
    # 删去多余内容
    arrs = [state.arr_arrival,
        state.arr_service, state.arr_depart]
    ncommon = minimum(length, arrs)
    for i in eachindex(arrs)
        arrs[i] = arrs[i][1:ncommon]
    end
    sele = fill(true, ncommon)
    for i in eachindex(arrs)
        sele .= sele .& (T0 .<= arrs[i] .<= T)
    end
    for i in eachindex(arrs)
        arrs[i] = arrs[i][sele]
    end

    return arrs 
end # function MM1_sim

# 测试M/M/1模拟，与理论结果比较
function test_mm1()
    λ = 0.2
    μ = 0.4
    T = 100_000
    arrs = MM1_sim(λ, μ, T; T0=1_000)

    # 滞留时间的数组
    t_in = arrs[3] .- arrs[1]
    mt_in = mean(t_in)
    # 理论值
    par_t_in = 1/(μ-λ)
    println("平均滞留时间：$(mt_in), 理论值：$(par_t_in)")

    arrs 
end # function test_mm1
test_mm1();
```

## 习题

#### 习题1

选择适当的编程语言实现例[15.1](https://www.math.pku.edu.cn/teachers/lidf/docs/statcomp/html/_statcompbook/sim-des.html#exm:sim-des-mm1)的算法。
考虑如下的变化情形:

1. 银行8:00开门，17:00关门，关门后不再允许顾客进入但已进入的顾客会服务完;
2. 顾客到来服从非齐次的泊松分布，其速率函数\(\lambda(t)\)为阶梯函数；
3. 如果顾客到来后发现队太长，有一定概率离开； 如果顾客等待了太长时间，有一定概率离开， 这时需要求这些离开顾客的平均人数。

#### 习题2

设计模拟如下离散事件的算法。
设某商场每天开放\(L\_0\)时间，
有扒手在该商城出没，
扒手的出现服从强度\(\lambda\_1\)的齐次泊松过程，
出现后作案\(X\)时间后离开，
设\(X\)服从对数正态分布, \(\ln X \sim \text{N}(\mu\_2, \sigma\_2^2)\)。
设有一个警察每隔\(L\_3\)时间在商城巡逻\(Y\)时间，
\(Y\)服从Exp\((\lambda\_2)\)分布，
只要扒手和警察同时出现在商城内扒手就会被抓获。
模拟估计一天时间内有扒手被抓获的概率。

#### 习题3

设计模拟M/M/c随机服务系统的算法。
设服务机构共有\(c\)个服务窗口，
顾客到来服从齐次泊松过程，速率为\(\lambda\)，
顾客排成一队，
一旦某个窗口空闲则队头的顾客接受服务，
每个窗口的服务时间均服从期望为\(1/\mu\)的指数分布。

1. 编程模拟上述随机服务系统；
2. 模拟估计顾客在这个服务机构的平均滞留时间\(ER\);
3. 在什么条件下这个服务机构的平均滞留时间能够稳定不变？