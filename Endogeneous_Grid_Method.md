# 内生网格法求解个体消费者最大化问题
## 
&emsp;  &emsp;   本文介绍内生网格法以解决值函数问题。其基本思想是对下一期的持有资产$a'$构建网格，而不是标准方法中对本期的$a$构建网格。考虑以下最大化问题，其状态变量是本期持有的资产和收入$(a,y)$:

$$
\begin{array}{l}
V(a,y) = \max \left\{ {u\left( c \right) + \beta E\left[ {V\left( {a'y'} \right)} \right]} \right\}\\
s.t.\\
c + a' \le R \cdot a + y\\
a' \ge  - b
\end{array}
$$

&emsp; &emsp;假定我们已经将收入序列离散化了，因此我们可以将上述问题的欧拉方程写为：
$${
u_c}(c(a,y)) \ge \beta R\sum\limits_{{y^\prime } \in Y} \pi  \left( {{y^\prime }|y} \right){u_c}\left( {c\left( {{a^\prime },{y^\prime }} \right)} \right)
$$


&emsp; &emsp;  当满足$a'\ge -b$时，欧拉方程取等号。上述问题的解是找一个消费的政策方程同时满足欧拉方程和资产下限约束。首先猜测消费的政策方程为${{\hat c}_0}(a,y)$,通过之后的不断迭代，使得最终的消费政策方程稳定不变。具体步骤如下：
&emsp; &emsp; 1、 构建$a'$和$y$的网格，使得：$a' \in {G_A} = \{ {a_1},...{a_M}\} ,{a_1} =  - b$，且  $y \in {G_y} = \{ {y_1},...{y_N}\} $ 。此处值得注意的是，应对下一期的资产 $y'$ 而不是本期的 $y$网格化。


&emsp; &emsp; 2、 猜测消费的政策方程为：$c_0 (a_i ,y_i)$.可以设 $c_0 (a_i ,y_i)=R a_i+y_i$。

&emsp; &emsp; 3、对于网格空间${G_A} \times {G_y}$ 上的点对$\\{ a_i,y_i \\}$ , 计算欧拉方程右侧的值（称为 $B(a_i^{'},yj)$ :
$$
B\left( {a_i^\prime ,{y_j}} \right) = \beta R\sum\limits_{{y^\prime } \in Y} \pi  \left( {{y^\prime }|{y_j}} \right){u_c}\left( {{{\hat c}_0}\left( {a_i^\prime ,{y^\prime }} \right)} \right)
$$
&emsp; &emsp;  4、利用欧拉方程求解满足下式的${{\tilde c}_0}(a_i^{'},{y_i})$：
$$
u_c\left(\tilde{c}_0\left(a_i^{\prime},y_j\right)\right)=B\left(a_i^{\prime},y_j\right)
$$

&emsp; &emsp;   注意，对于$u_c (c) =c^{-\gamma}$, 可以得到消费的c的解析解：$\tilde{c}_{0}\left(a_{i}^{\prime},y_{j}\right)=[B\left(a_{i}^{\prime},y_{j}\right)]^{-\frac{1}{\gamma}}.$这是使算法比传统算法更高效的关键步骤。因为这样一来就不需要求解非线性方程了，从而节省了大量的计算时间。这里我们只计算了一次第3步的期望。在传统的算法中，这是由非线性方程求解器多次完成的，而且它需要线性插值。

&emsp; &emsp;   5、根据预算约束计算本期最优的资产$a$:
$$
\tilde{c}\left(a_i^{\prime},y_j\right)+a_i^{\prime}=Ra_i^*+y_j
$$

 让$\hat{c}_1\left(a_i^*,y_j\right)=\tilde{c}_0\left(a_i^\prime,y_j\right)$  ，$a^*\left(a_i^{\prime},y_j\right)$是在收入水平等于$y_j$下，让下一期的资产等于$a'$的本期资产需要选择的水平。这就是内生的网格，它的划分会随着每次迭代而变化。注意，$a^*\left(a_i^{\prime},y_j\right)$很可能不是定义在$G_A$上的点。令$a_1^{*}$为使得下一期预算约束紧的本期资产值，即用$a_1^{*}$带入预算约束得到的值。
 

&emsp; &emsp;   6、现在来更新猜测。为得到$a_i\gt a_1^{*}$上消费政策方程$\hat{c}_{1}\left(a_{i},y_{j}\right)$的新猜测，可以用插值法更新得到$G_A$集合内每个点对应的政策方程。为了得到$a_i\lt a_1^{*}$  下消费政策方程的值，我们用预算约束来计算，

$$\hat{c}_1\left(a_i,y_j\right)=Ra_i+y_j-a_1^{\prime}$$
这是因为在借贷约束为紧的情况下，欧拉方程不再成立。

&emsp; &emsp;     7、通过比较${{\hat c}_{n + 1}}({a_i},{y_j})$ 和${{\hat c}_n}({a_i},{y_j})$。例如，对于很小的容忍值$\epsilon$，当第n+1次迭代满足下式就停止：
$$\max_{i,j}\left\{\left|\hat{c}_{n+1}\left(a_{i},y_{j}\right)-\hat{c}_{n}\left(a_{i},y_{j}\right)\right|\right\}<\varepsilon$$





