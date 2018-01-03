# 背包问题

instance：给定n个item，$i=1,2,\dots,n$; weights $w_1,w_2,\dots,w_n\in Z^{+}$; values $v_1,v_2,\dots,v_n\in Z^{+}$, 给定背包容量$B\in Z^+$; 

需要找到一个集合$S\in\{1,2,\dots,n\}$ 让背包里面的东西value最大。

1.贪心算法

根据$\frac{v_i}{w_i}$的大小排序，每次放进去性价比高的item，直到不能放为止。

2.动态规划

定义：

$A(i,v)$代表从$s\in\{1,2,\dots,i\}$里找到价值为v的最小的w和；如果这个集合s找不到，那么$+\infty$.

显然我们有：

$A(i,v)=min\{A(i-1,v),A(i-1,v-v_i)+w_i\}$

接下来我们就可以填表格似的求出max v such that $A(n,v)\le B$.

接下来分析这个算法，这个算法需要填的表的大小是$n\times V$,其中$V=\sum_{i=1}^{n}v_i$,因此这个算法时间复杂度是$O(n\times V)$.

问题的规模是n的，V不是n的多项式，所以这个算法不是polynomial time的。这个是伪多项式时间的算法。

3.fptas（Fully Polynomial-Time Approximation Scheme）

完全多项式时间近似方案。就是无论多小的$\epsilon$,都能够找到一个多项式时间的算法使得它的approximation ratio$>1- \epsilon$,(或者小于1+)。

对于这个背包问题，我们可以观察到伪多项式算法只是由于V不是多项式的，因此可以把V化成n的多项式相关的，令：

$\hat v_i=\lfloor\frac{v_i}{K}\rfloor$

然后利用动态规划算法，求出这个新$\hat v$集合的收益最大的集合$\hat S$. 假设opt最优的集合为$O$

显然有：

$\sum_{i\in \hat S}\hat v_i\ge \sum_{i\in  O}\hat v_i$

so：

$K\times \sum_{i\in \hat S}\hat v_i\ge K\times \sum_{i\in  O}\hat v_i$

又有：

$\lfloor\frac{v_i}{K}\rfloor>\frac{v_i}{K}-1$

So:

$K\times \hat v_i>v_i-K$

So:

$K\times \sum_{i\in  O}\hat v_i=\sum_{i\in O}K\times \hat v_i\ge \sum_{i\in O}(v_i-K)\ge opt-nK$

if we want:

$\frac{sol}{opt}>1-\epsilon $

then we want:

$\frac{opt-nK}{opt}\ge 1-\epsilon $

then:

$\frac{n\times K}{opt}\le \epsilon$

我们只需要找到opt的最小值即可。

那么opt显然是大于能放进这个背包中的物体中最大的那一个价值的。

记为P

Then:

$K=\frac {\epsilon \times P} {n}$

分析一下它的时间复杂度：

$\sum_{i\in S}\hat v_i=\sum_{i\in S}\lfloor v_i/K\rfloor< \lfloor \sum_{i\in S}v_i/k\rfloor$

And:

$\sum_{i\in S}v_i/P<n$

Then:

$\hat v<n\times \frac{n}{\epsilon }$

因此时间复杂度为：$O(n^3/\epsilon )$

