# 装箱问题的动态规划算法和渐进ptas

instance：给定n个物体，大小分别为$s_1,s_2,\dots,s_n$,将其放进大小为m的箱子里

问，需要的最少的箱子的数目是多少。

可以将这个问题抽象成，给定n个物体都在0，1之间，将其放在大小为1的箱子里，球最少的数目。

1.firstfit

这个算法和内存分配算法类似。

把物体放在第一个能够放下的箱子里，如果前面的箱子都放不下，那么重开一个箱子。显然对于这种算法，有：除了最后一个箱子，其他的箱子都应该是超过半满的。否则，假设有两个箱子都没有半满，那么将这两个箱子倒在一起就行了。

so,we get:

$opt>\sum_{i}s_i>\frac {(sol-1)}{2}$

then:

$sol\le2 opt$

这是对于所有情况都成立的估计。

那么如果所有的箱子都很小呢？假设每个箱子都比$\gamma$小，那么只可能有一个箱子里面的东西是小于等于$1-\gamma$的，其余的所有箱子都应该比$1-\gamma$大，那么就有以下的估计：

$opt\ge\sum_{i}s_i>(1-\gamma)\times (sol-1)$

So:

$sol<\frac{opt}{1-\gamma}+1$

装箱问题可以由一个NP问题：partition problem归约来，因此这个问题不存在近似度小于3/2的算法。

2.动规

assume:

$|\{S_ 1,S_2,\dots,s_n\}$=k

which is: there are k groups of the items.

let: $s_{(1)},s_{(2)}\dots,s_{(k)}$的数量分别为$n_1,n_2,\dots,n_k$,let:$\overrightarrow{N}=(n_1,n_2,\dots,n_k)$,$\overrightarrow{I}=(i_1,i_2,\dots,i_k)$

$bin(\overrightarrow{I})=1+min_{bin(\overrightarrow{X})=1}bin(\overrightarrow{I}-\overrightarrow{X})$

那么就有一个rounding的算法，将整个n个item分为K组，每组选它最大的元素作为新的问题的一个元素。

显然，如果箱子能够放下新的问题的item，它肯定能放下原来的item，因为新问题的每个item都比原问题的大。

对于新问题利用动态规划的思想可以得到一个解。

we have:

$opt<sol$

构造另一个问题，将item向下取，而不是向上取，这个是利用数列的知识，需要推导，最终可以得到：

$sol\le opt+\lceil\frac{n}{k}\rceil$

如果原问题中整个集合中item最大的元素是$\gamma$,那么我们有：

$opt\ge n\times \gamma$

So, we have:

$\frac{sol}{opt}\le 1+\frac{\lceil\frac{n}{k}\rceil}{opt}\le 1+\frac{2}{k\times \gamma}$

3.渐进ptas

我们可以观察到firstfit对于物体小的话效果很好，动态规划对于item大效果很好。

那么一个很自然的算法就是：

1.将大小小于$\gamma$的item，denoted as $I^{small }$, $I^{big}=I-I^{small}$

2.$I^{big}$rounding to $I^{finite}$

3.动态规划

4.使用fitstfit来装小的item

assume the solution we get is $m^{'}$, the  big item is m,then we have:

$m^{'}\le \max\{m,\frac{opt}{1-\gamma}+1\}$

这个问题可以这样理解，我们将大的item利用动态规划求出了一个solution m，还有一些小的东西没有放，如果放这些小的item不需要重新开新的箱子，那么最后的结果就是m，如果重新开了新的箱子，那显然这m个箱子都不能放这些小的item了，那么就换另一个角度来看，也就是最多有一个箱子的东西是小于$1-\gamma$的，其余所有都是大的，那么这个估计就回到了之前的firstfit估计了。

then we have：

$\frac{m}{opt}\le1+\frac{2}{k\times \gamma}$

and we want:

$m^{''}\le1+(1+\epsilon )opt$

choose γ = ε/2 and k = 4/ε2.

