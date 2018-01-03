# vertex cover问题的简单LP算法

vertex cover:

instance: 给定一个图G(V，E)，找到一个顶点集使得所有的边都被覆盖。并使得这个集合最小。

它的ILP问题是：

$minimize: \sum_{v\in V}X_v$

$s.t: \sum_{v \in e}x_v\ge 1\ \ \ \ \ \ e\in E$

$\ \ \ \ \ \ \ \ x_v\in \{0,1\}\ \ \ \ \ v\in V$

整数规划问题也是NP-hard的

因此我们可以求它的LP问题。

即：

$minimize: \sum_{v\in V}X_v$

$s.t: \sum_{v \in e}x_v\ge 1\ \ \ \ \ \ e\in E$

$\ \ \ \ \ \ \ \ x_v\in [0,1]\ \ \ \ \ v\in V$

假设已经找到了这个LP问题的解，显然有：

$sol\le opt$

也就是线性规划的最优解是一定好于整数规划的最优解的。

记这个LP问题的最优解为$x^{*}_{v}$

那么接下来需要找到一个可行解使得整数规划问题的条件满足。

给出这样的round。

$\hat x_{v}=1$ if $x^{*}_{v}>0.5$

$\hat x_{v}=0$ otherwise

验证其是可行解：

 $\sum_{v \in e}x_v\ge 1$

$ \sum_{uv \in e}x_v+x_u\ge 1$

rounding之后不满足当且仅当原来的两个值都小于0.5，如果这种情况出现，原来的解也不满足条件。

同时

$\hat x_{v}=1$ if $x^{*}_{v}>0.5$ 可得 $\hat x_{v}\le 2\times x^{*}_{v}$

$\hat x_{v}=0$ otherwise 可得 $\hat x_{v}\le 2\times x^{*}_{v}$

因此：

$\hat x_{v}\le 2\times x^{*}_{v}$

所以sol<2opt

总结，解决ILP问题的一般步骤是：

1.将问题抽象成ILP问题

2.relax ilp to lp

3.求解LP

4.将LP的解rounding到ILP，找到可行解

5.证明这个可行解的误差bound。