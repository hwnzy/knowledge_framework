# MarkDown

## 

## MarkDown数学公式

**MarkDown数学公式：使用$，将数学公式写在两个$之间。写在两个$$之间是把公式居中。**

## 1、上下标

^ 表示上标， \_ 表示下标，如果上标或下标内容多于一个字符，则使用 {} 括起来。  
 例：`$(x^2 + x^y )^{x^y}+ x_1^2= y_1 - y_2^{x_1-y_1^2}$`

$$
(x^2 + x^y) ^ {x^y} + x_1^2 = y_1 - y_2 ^ {x_1 - y_1^2}
$$

## 2、分数

公式 \frac{分子}{分母}，或 分子 \over 分母  
 例：`$\ frac{1-x}{y+1}$或$x \over x+y$`

$$
\frac{1-x}{y+1} 或 {x \over x + y}
$$

## 3、开方

公式\sqrt\[n\]{a}  
 例：`$\sqrt[3]{4}$或$\sqrt{9}$`

$$
\sqrt[3]{4} 或 {\sqrt{9}}
$$

## 4、括号

* \(\) \[\] 直接写就行，而 {} 则需要转义。  例：`$f(x, y) = x^2 + y^2, x \epsilon [0, 100], y \epsilon \{1,2,3\}$`  ![f\(x, y\) = x^2 + y^2, x \epsilon \[0, 100\], y \epsilon \{1,2,3\}](https://math.jianshu.com/math?formula=f%28x%2C%20y%29%20%3D%20x%5E2%20%2B%20y%5E2%2C%20x%20%5Cepsilon%20%5B0%2C%20100%5D%2C%20y%20%5Cepsilon%20%5C%7B1%2C2%2C3%5C%7D)
* 大括号，需要括号前加\left和\right。  例：`$(\sqrt{1 \over 2})^2$加大括号后$\left(\sqrt{1 \over 2}\right)^2$`  ![\(\sqrt{1 \over 2}\)^2](https://math.jianshu.com/math?formula=%28%5Csqrt%7B1%20%5Cover%202%7D%29%5E2)加大括号后![\left\(\sqrt{1 \over 2}\right\)^2](https://math.jianshu.com/math?formula=%5Cleft%28%5Csqrt%7B1%20%5Cover%202%7D%5Cright%29%5E2)
* \left 和 \right必须成对出现，对于不显示的一边可以使用 . 代替。  例：`$\frac{du}{dx} | _{x=0}$加大后$\left. \frac{du}{dx} \right| _{x=0}$`  ![\frac{du}{dx} \| \_{x=0}](https://math.jianshu.com/math?formula=%5Cfrac%7Bdu%7D%7Bdx%7D%20%7C%20_%7Bx%3D0%7D)加大后![\left. \frac{du}{dx} \right\| \_{x=0}](https://math.jianshu.com/math?formula=%5Cleft.%20%5Cfrac%7Bdu%7D%7Bdx%7D%20%5Cright%7C%20_%7Bx%3D0%7D)
* 大括号  例：`$y :\begin{cases} x+y=1\\ x-y = 0 \end{cases}$`  ![y :\begin{cases} x+y=1\\ x-y = 0 \end{cases}](https://math.jianshu.com/math?formula=y%20%3A%5Cbegin%7Bcases%7D%20x%2By%3D1%5C%5C%20x-y%20%3D%200%20%5Cend%7Bcases%7D)

## 5、向量

公式\vec{a}  
 例：`$\vec a \cdot \vec b = 1$`  
 ![\vec a \cdot \vec b = 1](https://math.jianshu.com/math?formula=%5Cvec%20a%20%5Ccdot%20%5Cvec%20b%20%3D%201)

## 6、定积分

公式\int  
 例：`符号：$\int$，示例公式：$\int_0^1x^2dx$`  
 符号：![\int](https://math.jianshu.com/math?formula=%5Cint)，示例公式：![\int\_0^1x^2dx](https://math.jianshu.com/math?formula=%5Cint_0%5E1x%5E2dx)

## 7、极限

公式\lim\_{n\rightarrow+\infty}  
 例：`符号：$\lim_{n\rightarrow+\infty}$，示例公式：$\lim_{n\rightarrow+\infty}\frac{1}{n}$`  
 符号：![\lim\_{n\rightarrow+\infty}](https://math.jianshu.com/math?formula=%5Clim_%7Bn%5Crightarrow%2B%5Cinfty%7D)，示例公式：![\lim\_{n\rightarrow+\infty}\frac{1}{n}](https://math.jianshu.com/math?formula=%5Clim_%7Bn%5Crightarrow%2B%5Cinfty%7D%5Cfrac%7B1%7D%7Bn%7D)

## 8、累加/累乘

公式累加\sum\_1^n, 累乘\prod\_{i=0}^n  
 例：`累加$\sum_1^n$, 累乘$\prod_{i=0}^n$`  
 累加![\sum\_1^n](https://math.jianshu.com/math?formula=%5Csum_1%5En), 累乘![\prod\_{i=0}^n](https://math.jianshu.com/math?formula=%5Cprod_%7Bi%3D0%7D%5En)

## 9、省略号

公式\ldots 表示底线对其的省略号，\cdots 表示中线对其的省略号，\cdot点乘号。  
 例：`$f(x_1,x_2,\ldots,x_n) = \left({1 \over x_1}\right)^2+\left({1 \over x_2}\right)^2+\cdots+\left({1 \over x_n}\right)^2$`  
 ![f\(x\_1,x\_2,\ldots,x\_n\) = \left\({1 \over x\_1}\right\)^2+\left\({1 \over x\_2}\right\)^2+\cdots+\left\({1 \over x\_n}\right\)^2](https://math.jianshu.com/math?formula=f%28x_1%2Cx_2%2C%5Cldots%2Cx_n%29%20%3D%20%5Cleft%28%7B1%20%5Cover%20x_1%7D%5Cright%29%5E2%2B%5Cleft%28%7B1%20%5Cover%20x_2%7D%5Cright%29%5E2%2B%5Ccdots%2B%5Cleft%28%7B1%20%5Cover%20x_n%7D%5Cright%29%5E2)

## 10、符号

### 10.1 数学符号

| 代码 | 符号 | 描述 |
| :--- | :--- | :--- |
| `$\not=$` | ![\not=](https://math.jianshu.com/math?formula=%5Cnot%3D) | 不等于 |
| `$\approx$` | ![\approx](https://math.jianshu.com/math?formula=%5Capprox) | 约等于 |
| `$\leq$` | ![\leq](https://math.jianshu.com/math?formula=%5Cleq) | 小于等于 |
| `$\geq$` | ![\geq](https://math.jianshu.com/math?formula=%5Cgeq) | 大于等于 |
| `$\times$` | ![\times](https://math.jianshu.com/math?formula=%5Ctimes) | 乘号 |
| `$\pm$` | ![\pm](https://math.jianshu.com/math?formula=%5Cpm) | 正负号 |
| `$\div$` | ![\div](https://math.jianshu.com/math?formula=%5Cdiv) | 除号 |
| `$\sum$` | ![\sum](https://math.jianshu.com/math?formula=%5Csum) | 累加 |
| `$\prod$` | ![\prod](https://math.jianshu.com/math?formula=%5Cprod) | 累乘 |
| `$\coprod$` | ![\coprod](https://math.jianshu.com/math?formula=%5Ccoprod) | 累除 |
| `$\overline{a+b+c+d}$` | ![\overline{a+b+c+d}](https://math.jianshu.com/math?formula=%5Coverline%7Ba%2Bb%2Bc%2Bd%7D) | 平均值 |

### 10.2 三角函数

| 代码 | 符号 | 描述 |
| :--- | :--- | :--- |
| `$\bot$` | ![\bot](https://math.jianshu.com/math?formula=%5Cbot) | 垂直 |
| `$\angle$` | ![\angle](https://math.jianshu.com/math?formula=%5Cangle) | 角 |
| `$30^\circ$` | ![30^\circ](https://math.jianshu.com/math?formula=30%5E%5Ccirc) | 30度 |
| `$\sin$` | ![\sin](https://math.jianshu.com/math?formula=%5Csin) | 正弦 |
| `$\cos$` | ![\cos](https://math.jianshu.com/math?formula=%5Ccos) | 余弦 |
| `$\tan$` | ![\tan](https://math.jianshu.com/math?formula=%5Ctan) | 正切 |
| `$\cot$` | ![\cot](https://math.jianshu.com/math?formula=%5Ccot) | 余切 |
| `$\sec$` | ![\sec](https://math.jianshu.com/math?formula=%5Csec) | 正割 |
| `$\csc$` | ![\csc](https://math.jianshu.com/math?formula=%5Ccsc) | 余割 |

### 10.3 定积分

| 代码 | 符号 | 描述 |
| :--- | :--- | :--- |
| `$\infty$` | ![\infty](https://math.jianshu.com/math?formula=%5Cinfty) | 无穷 |
| `$\int$` | ![\int](https://math.jianshu.com/math?formula=%5Cint) | 定积分 |
| `$\iint$` | ![\iint](https://math.jianshu.com/math?formula=%5Ciint) | 双重积分 |
| `$\iiint$` | ![\iiint](https://math.jianshu.com/math?formula=%5Ciiint) | 三重积分 |
| `$\oint$` | ![\oint](https://math.jianshu.com/math?formula=%5Coint) | 曲线积分 |
| `$y\prime$` | ![y\prime](https://math.jianshu.com/math?formula=y%5Cprime) | 求导 |
| `$y\prime$` | ![\lim](https://math.jianshu.com/math?formula=%5Clim) | 极限 |

### 10.4 集合

| 代码 | 符号 | 描述 |
| :--- | :--- | :--- |
| `$\emptyset$` | ![\emptyset](https://math.jianshu.com/math?formula=%5Cemptyset) | 空集 |
| `$\in$` | ![\in](https://math.jianshu.com/math?formula=%5Cin) | 属于 |
| `$\notin$` | ![\notin](https://math.jianshu.com/math?formula=%5Cnotin) | 不属于 |
| `$\supset$` | ![\supset](https://math.jianshu.com/math?formula=%5Csupset) | 真包含 |
| `$\supseteq$` | ![\supseteq](https://math.jianshu.com/math?formula=%5Csupseteq) | 包含 |
| `$\bigcap$` | ![\bigcap](https://math.jianshu.com/math?formula=%5Cbigcap) | 交集 |
| `$\bigcup$` | ![\bigcup](https://math.jianshu.com/math?formula=%5Cbigcup) | 并集 |
| `$\bigvee$` | ![\bigvee](https://math.jianshu.com/math?formula=%5Cbigvee) | 逻辑或 |
| `$\bigwedge$` | ![\bigwedge](https://math.jianshu.com/math?formula=%5Cbigwedge) | 逻辑与 |

### 10.5 对数符号

| 代码 | `$\log$` | `$\lg$` | `$\ln$` |
| :--- | :--- | :--- | :--- |
| 符号 | ![\log](https://math.jianshu.com/math?formula=%5Clog) | ![\lg](https://math.jianshu.com/math?formula=%5Clg) | ![\ln](https://math.jianshu.com/math?formula=%5Cln) |

### 10.6 希腊字母

| 代码 | 符号 | 代码 | 符号 |
| :--- | :--- | :--- | :--- |
| `$\alpha$` | ![\alpha](https://math.jianshu.com/math?formula=%5Calpha) | `$\beta$` | ![\beta](https://math.jianshu.com/math?formula=%5Cbeta) |
| `$\gamma$` | ![\gamma](https://math.jianshu.com/math?formula=%5Cgamma) | `$\delta$` | ![\delta](https://math.jianshu.com/math?formula=%5Cdelta) |
| `$\epsilon$` | ![\epsilon](https://math.jianshu.com/math?formula=%5Cepsilon) | `$\varepsilon$` | ![\varepsilon](https://math.jianshu.com/math?formula=%5Cvarepsilon) |
| `$\zeta$` | ![\zeta](https://math.jianshu.com/math?formula=%5Czeta) | `$\eta$` | ![\eta](https://math.jianshu.com/math?formula=%5Ceta) |
| `$\theta$` | ![\theta](https://math.jianshu.com/math?formula=%5Ctheta) | `$\Theta$` | ![\Theta](https://math.jianshu.com/math?formula=%5CTheta) |
| `$\vartheta$` | ![\vartheta](https://math.jianshu.com/math?formula=%5Cvartheta) | `$\pi$` | ![\pi](https://math.jianshu.com/math?formula=%5Cpi) |
| `$\phi$` | ![\phi](https://math.jianshu.com/math?formula=%5Cphi) | `$\psi$` | ![\psi](https://math.jianshu.com/math?formula=%5Cpsi) |
| `$\Psi$` | ![\Psi](https://math.jianshu.com/math?formula=%5CPsi) | `$\omega$` | ![\omega](https://math.jianshu.com/math?formula=%5Comega) |
| `$\Omega$` | ![\Omega](https://math.jianshu.com/math?formula=%5COmega) | `$\rho$` | ![\rho](https://math.jianshu.com/math?formula=%5Crho) |
| `$\sigma$` | ![\sigma](https://math.jianshu.com/math?formula=%5Csigma) | `$\xi$` | ![\xi](https://math.jianshu.com/math?formula=%5Cxi) |
| `$\mu$` | ![\mu](https://math.jianshu.com/math?formula=%5Cmu) | `$\partial$` | ![\partial](https://math.jianshu.com/math?formula=%5Cpartial) |

