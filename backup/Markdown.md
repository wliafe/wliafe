# Markdown编写数学公式

## 简介

在 Markdown 中，您可以使用 LaTeX 语法编辑数学公式。要在 Markdown 中插入数学公式，您需要使用一对美元符号（$）将公式包围起来。例如：

$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

这将显示一个二次方程的解，其中 `a`、`b` 和 `c` 是系数，`x` 是未知数。

如果您想在行内显示公式，只需在公式前后加上一个美元符号即可，例如：

$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$

注意，Markdown 中的数学公式不支持像物理符号（如→）或复杂的数学标记（如矩阵）这样的特殊符号。对于这些特殊情况，您可能需要使用专门的数学编辑器或 LaTeX 文档。

## 常见数学符号和公式

### 上下标

|显示|代码|描述|
|:--:|:--:|:--:|
|$x^{2}$|`$x^{2}$`|上标，例如：x 的平方|
|$x_{i}$|`$x_{i}$`|下标，例如：第 i 个 x|

### 括号和分隔符

()、[] 和 | 可以直接输入

|显示|代码|显示|代码|描述|
|:--:|:--:|:--:|:--:|:--:|
|$\langle$|`$\langle$`|$\rangle$|`$\rangle$`|尖括号|
|$\{$|`$\{$`|$\}$|`$\}$`|花括号|

### 分数

分数通常使用 `\frac{分子}{分母}` ，如：

|显示|代码|
|:--:|:--:|
|$\frac{x}{y}$|`$\frac{x}{y}$`|

分式较为复杂时，可以使用 `分子 \over 分母` 表示，如：

|显示|代码|
|:--:|:--:|
|$a+b+c+d+e+f \over g+h+i+j+k+l$|`$a+b+c+d+e+f \over g+h+i+j+k+l$`|​

当分式仅有两个字符时，可以使用 `\frac ab` 来快速生成$\frac ab$

### 开方

使用 `\sqrt[根指数，默认为2]{被开方数}`，如：

|显示|代码|
|:--:|:--:|
|$\sqrt{2}$|`$\sqrt{2}$`|
|$\sqrt[n]{5}$|`$\sqrt[n]{5}$`|

### 向量和箭头

|显示|代码|描述|
|:--:|:--:|:--:|
|$\vec{x}$|`$\vec{x}$`|向量 x|
|$\overleftarrow{x}$|`$\overleftarrow{x}$`|x 上方左箭头|
|$\overrightarrow{x}$|`$\overrightarrow{x}$`|x 上方向右箭头|
|$\leftarrow$|`$\leftarrow$`|左箭头|
|$\rightarrow$|`$\rightarrow$`|右箭头|
|$\uparrow$|`$\uparrow$`|向上的箭头|
|$\downarrow$|`$\downarrow$`|向下的箭头|
|$\Leftarrow$|`$\Leftarrow$`||
|$\Rightarrow$|`$\Rightarrow$`||
|$\Uparrow$|`$\Uparrow$`||
|$\Downarrow$|`$\Downarrow$`||
	
### 积分

使用 `\int_{积分下限}^{积分上限} {被积表达式}`

例如：`$$ \int_{0}^{1}{x^{3}}dx $$`

显示：$$ \int_{0}^{1}{x^{3}}dx $$

### 极限运算

使用 `\lim{变量 \to 表达式}` 表达式

例如：`$$ \lim_{x \to \infty} \frac{1}{n(n+1)} $$`

显示：$$ \lim_{x \to \infty} \frac{1}{n(n+1)} $$

### 累加、累乘

使用 `\sum_{下标表达式}^{上标表达式} {累加表达式}`

例如：`$$ \sum_{i=0}^{n} x_i $$`

显示：$$ \sum_{i=0}^{n} x_i $$

与累加类似，

+ 累乘使用 `$\prod$`

+ 并集使用 `$\bigcup$`

+ 交集使用 `$\bigcap$`

例如：`$$ \sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} \Bbb{R} $$`

显示：$$ \sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} \Bbb{R} $$

### 希腊字母

输入 `\小写希腊字母英文全称` 和 `\首字母大写希腊字母英文全称` 来分别输入小写和大写希腊字母

|显示(小写)|代码(小写)|显示(大写)|代码(大写)|
|:--:|:--:|:--:|:--:|
|$\alpha$|`$\alpha$`|$A$|`$A$`|
|$\beta$|`$\beta$`|$B$|`$B$`|
|$\gamma$|`$\gamma$`|$\Gamma$|`$\Gamma$`|
|$\delta$|`$\delta$`|$\Delta$|`$\Delta$`|
|$\epsilon$|`$\epsilon$`|$\Epsilon$|`$\Epsilon$`|
|$\zeta$|`$\zeta$`|$\Zeta$|`$\Zeta$`|
|$\eta$|`$\eta$`|$\Eta$|`$\Eta$`|
|$\theta$|`$\theta$`|$\Theta$|`$\Theta$`|
|$\iota$|`$\iota$`|$\Iota$|`$\Iota$`|
|$\kappa$|`$\kappa$`|$\Kappa$|`$\Kappa$`|
|$\lambda$|`$\lambda$`|$\Lambda$|`$\Lambda$`|
|$\mu$|`$\mu$`|$\Mu$|`$\Mu$`|
|$\nu$|`$\nu$`|$\Nu$|`$\Nu$`|
|$\xi$|`$\xi$`|$\Xi$|`$\Xi$`|
|$o$|`$o$`|$O$|`$O$`|
|$\pi$|`$\pi$`|$\Pi$|`$\Pi$`|
|$\rho$|`$\rho$`|$\Rho$|`$\Rho$`|
|$\sigma$|`$\sigma$`|$\Sigma$|`$\Sigma$`|
|$\tau$|`$\tau$`|$\Tau$|`$\Tau$`|
|$\upsilon$|`$\upsilon$`|$\Upsilon$|`$\Upsilon$`|
|$\phi$|`$\phi$`|$\Phi$|`$\Phi$`|
|$\chi$|`$\chi$`|$\Chi$|`$\Chi$`|
|$\psi$|`$\psi$`|$\Psi$|`$\Psi$`|
|$\omega$|`$\omega$`|$\Omega$|`$\Omega$`|

### 关系运算符

|显示|代码|
|:--:|:--:|
|$\pm$|`$\pm$`|
|$\times$|`$\times$`|
|$\div$|`$\div$`|
|$\mid$|`$\mid$`|
|$\nmid$|`$\nmid$`|
|$\cdot$|`$\cdot$`|
|$\circ$|`$\circ$`|
|$\ast$|`$\ast$`|
|$\bigodot$|`$\bigodot$`|
|$\bigotimes$|`$\bigotimes$`|
|$\bigoplus$|`$\bigoplus$`|
|$\leq$|`$\leq$`|
|$\geq$|`$\geq$`|
|$\neq$|`$\neq$`|
|$\approx$|`$\approx$`|
|$\equiv$|`$\equiv$`|
|$\sum$|`$\sum$`|
|$\prod$|`$\prod$`|
|$\coprod$|`$\coprod$`|
|$\backslash$|`$\backslash$`|

### 集合运算

|显示|代码|描述|
|:--:|:--:|:--:|
|$\emptyset$|`$\emptyset$`|空集|
|$\in$|`$\in$`|属于|
|$\notin$|`$\notin$`|不属于|
|$\subset$|`$\subset$`|真包含于| 
|$\supset$|`$\supset$`|真包含|
|$\subseteq$|`$\subseteq$`|包含|
|$\supseteq$|`$\supseteq$`|包含|
|$\cap$|`$\cap$`|交集|
|$\cup$|`$\cup$`|并集|
|$\vee$|`$\vee$`||
|$\wedge$|`$\wedge$`||
|$\uplus$|`$\uplus$`||
|$\top$|`$\top$`||
|$\bot$|`$\bot$`||
|$\complement$|`$\complement$`||

### 对数运算

|显示|代码|
|:--:|:--:|
|$\log$|`$\log$`|
|$\lg$|`$\lg$`|
|$\ln$|`$\ln$`|

### 三角运算

|显示|代码|
|:--:|:--:|
|$\backsim$|`$\backsim$`|
|$\cong$|`$\cong$`|
|$\angle A$|`$\angle A$`|
|$\sin$|`$\sin$`|
|$\cos$|`$\cos$`|
|$\tan$|`$\tan$`|
|$\csc$|`$\csc$`|
|$\sec$|`$\sec$`|
|$\cot$|`$\cot$`|

### 微积分运算

|显示|代码|
|:--:|:--:|
|$\int$|`$\int$`|
|$\iint$|`$\iint$`|
|$\iiint$|`$\iiint$`|
|$\partial$|`$\partial$`|
|$\oint$|`$\oint$`|
|$\prime$|`$\prime$`|
|$\lim$|`$\lim$`|
|$\infty$|`$\infty$`|
|$\nabla$|`$\nabla$`|

### 逻辑运算

|显示|代码|描述|
|:--:|:--:|:--:|
|$\because$|`$\because$`|因为|
|$\therefore$|`$\therefore$`|所以|
|$\neg$|`$\neg$`|非|
|$\forall$|`$\forall$`|任意|
|$\exists$|`$\exists$`|存在|
|$\not\subset$|`$\not\subset$`|不包含于|
|$\not<$|`$\not<$`|不小于|
|$\not>$|`$\not>$`|不大于|
|$\not=$|`$\not=$`|不等于|

### 戴帽运算

|显示|代码|
|:--:|:--:|
|$\hat{xy}$|`$\hat{xy}$`|
|$\widehat{xyz}$|`$\widehat{xyz}$`|
|$\bar{y}$|`$\bar{y}$`|
|$\tilde{xy}$|`$\tilde{xy}$`|
|$\widetilde{xyz}$|`$\widetilde{xyz}$`|
|$\acute{y}$|`$\acute{y}$`|
|$\breve{y}$|`$\breve{y}$`|
|$\check{y}$|`$\check{y}$`|
|$\grave{y}$|`$\grave{y}$`|
|$\dot{x}$|`$\dot{x}$`|
|$\ddot{x}$|`$\ddot{x}$`|

### 为公式加注释

使用 `\text{注释内容}`

|显示|代码|
|:--:|:--:|
|$f(x)= \begin{cases} 0,& \text{if x is even} \\ 1, & \text{if x is odd} \end{cases}$|`$f(x)= \begin{cases} 0,& \text{if x is even} \\ 1, & \text{if x is odd} \end{cases}$`|

### 为公式加序号

使用 `\tag{公式序号}`

|显示|代码|
|:--:|:--:|
|$$y=ax+b \tag{公式1}$$|`$$y=ax+b \tag{公式1}$$`|

### 加粗

数字加粗：

使用`$\mathbf{数学符号}$`

|显示|代码|
|:--:|:--:|
|$\mathbf{0123456789}$|`$\mathbf{0123456789}$`|

希腊字母加粗：

使用`$\pmb{希腊字母}$`

|显示|代码|
|:--:|:--:|
|$\pmb{\alpha\beta}$|`$\pmb{\alpha\beta}$`|

斜体加粗：

使用`$\boldsymbol{希腊字母}$`

|显示|代码|
|:--:|:--:|
|$\boldsymbol{\alpha\beta}$|`$\boldsymbol{\alpha\beta}$`|

### 矩阵

使用`$$\begin{matrix}…\end{matrix}$$`来实现矩阵。

不带括号的矩阵

代码：

```md
$$
  \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix} \tag{1}
$$
```

显示效果：

$$
  \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix} \tag{1}
$$

带{ }的矩阵

代码：

```md
$$
  \left\{
  \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right\} \tag{2}
$$
```

显示效果：

​$$
 \left\{
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right\} \tag{2}
$$

带[ ]的矩阵

代码：

```md
$$
 \left[
 \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right] \tag{3}
$$
```

显示效果：

$$
  \left[
  \begin{matrix}
   1 & 2 & 3 \\
   4 & 5 & 6 \\
   7 & 8 & 9
  \end{matrix}
  \right] \tag{3}
$$

带省略号的矩阵

代码：

```md
$$
  \left[
  \begin{matrix}
   1      & 2      & \cdots & 4       \\
   7      & 6      & \cdots & 5       \\
   \vdots & \vdots & \ddots & \vdots  \\
   8      & 9      & \cdots & 0       \\
  \end{matrix}
  \right] \tag{4}
$$
```

显示效果：

$$
  \left[
  \begin{matrix}
   1      & 2      & \cdots & 4       \\
   7      & 6      & \cdots & 5       \\
   \vdots & \vdots & \ddots & \vdots  \\
   8      & 9      & \cdots & 0       \\
  \end{matrix}
  \right] \tag{4}
$$

### 乘积

叉乘：

|显示|代码|
|:--:|:--:|
|$a \times b$|`$a \times b$`|

点乘：

|显示|代码|
|:--:|:--:|
|$a \cdot b$|`$a \cdot b$`|

内积：

|显示|代码|
|:--:|:--:|
|$\langle a , b \rangle$|`$\langle a , b \rangle$`|

外积：

|显示|代码|
|:--:|:--:|
|$a \otimes b$|`$a \otimes b$`|

### 实数R

|显示|代码|
|:--:|:--:|
|$\mathbb{R}$|`$\mathbb{R}$`|