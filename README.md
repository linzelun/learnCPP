---
文档创建时间: 2024-10-24 15:43
领域: 
关键词: 
作者: 
机构: 
期刊名称: 
tags:
  - 面元法
aliases:
  - A502 theory
---
---
说明
1. *文献笔记标题命名规则：*
	*文献标题 + 主要作者 + 期刊名称（可缩写） + 发表时间*


>[!QUESTION] 问题


---

## 这是什么？


## 这对我有什么用？
理解[[A5021 User's Manual PAN AIR Technology Program for solving problems of potential flow about arbitrary configurations. BOEING. 1992|PANAIR]]程序的理论基础

---

# 1. introduction

PANAIR (an abbreviation for "panel aerodynamics") is a system of computer programs designed to analyze subsonic or supersonic inviscid flows about arbitrary configurations.
PANAIR（“面元空气动力学”的缩写）是一个计算机程序系统，旨在分析任意外形的亚音速或超音速无粘流动。
It is one of a sequence of computer programs developed over the past two decades which fall in the category of "panel methods."
它是过去二十年开发的一系列计算机程序之一，属于“面元法”的范畴。
Generally speaking, a panel method is a program which solves a linear partial differential equation numerically by approximating the configuration surface by a set of panels on which unknown "singularity strengths" are defined, imposing boundary conditions at a discrete set of points, such as panel centers, and thereby generating a system of linear equations relating the unknown singularity strengths.
一般来说，面元法是一种程序，它通过定义未知“[[奇异强度]]”的一组面元来逼近外形表面，在离散的一组点（如面元中心）施加边界条件，从而生成与未知奇异强度相关的线性方程组，从而数值求解线性偏微分方程。
The equations are then solved to obtain the singularity strengths, which, once known, provide complete information about the flow.
然后求解方程以获得奇异强度，一旦知道，就提供了关于流动的完整信息。

PANAIR differs from earlier panel methods in that it is a "higher order" panel method; that is, ==the singularity strengths are not constant on each panel==.
PANAIR不同于早期的面元法，因为它是一种“高阶”面元法；也就是说，奇异强度在每个面板上不是恒定的。
This is necessitated by the more stringent requirements of supersonic flow problems.
这源于超音速流动问题更为严格的要求。
Numerical solution of the differential equation for supersonic flow, the wave equation, is far more sensitive to the numerical idiosyncracies of a panel method than is the solution of Laplace's equation, which governs subsonic flow.
超音速流动的微分方程（如波动方程）的数值解对面元法的数值特性要比控制亚音速流动的拉普拉斯方程更加敏感。
The potential for numerical error is greatly reduced by requiring the doublet singularity strength to be continuous.
通过要求偶极子奇异强度的连续性，可以大大降低数值误差的可能性。

It is this "higher order" attribute which, in turn, allows PANAIR to be used to analyze flow about ==arbitrary configurations==.
正是这种“高阶”特性，反过来使得 PANAIR 能够用于分析**任意构型**周围的流动。
PANAIR can handle the simple configurations considered in preliminary design, and at the same time serves as an "analytical wind tunnel" for the analysis of flow about detailed, complex configurations.
PANAIR可以处理初步设计中考虑的简单配置，同时作为“分析风洞”，用于分析详细、复杂配置的流动。

the basis version 3.0 PANAIR capabilities include:
- (a) the ability to handle, within the limitations of linear potential flow theory, completely arbitrary configurations, using either exact or linearized boundary conditions; 在满足线性势流理论限制的情况下，能够使用精确或线性化边界条件处理完全任意的构型。
- (b) the ability to handle asymmetric configurations as well as those which one or two planes of symmetry; 能够处理不对称构型以及具有一个或两个对称平面的构型。
- (c) the ability to handle symmetric configurations in either symmetric or asymmetric flow; 能够处理对称构型，无论是在对称流动还是不对称流动中。
- (d) the ability to superimpose an incremental velocity on the freestream, either locally or globally, in order to simulate effects such as a rotational motion, differing angles of attack for different portions of a configuration, or a propeller slipstream; 能够在自由流上叠加增量速度，既可以局部叠加，也可以全局叠加，以模拟旋转运动、配置不同部分的攻角差异或螺旋桨滑流等效应。
- (e) the ability to calculate pressures, forces and moments using a variety of pressure formulas (such as isentropic, linear, etc.), including the forces and moments due to momentum flux through the surface; 能够使用多种压力公式（如等熵、线性等）计算压力、力和力矩，包括由于通过表面的动量通量产生的力和力矩。
- (f) the ability to calculate leading edge and side edge thrust forces and moments for thin configurations; 能够计算薄型构型的前缘和侧缘推力及力矩。
- (g) the ability to perform non-iterative design of a configuration, a process in which a desired pressure or tangential velocity distribution is specified. The program then determines the "residual" normal flow through the surface required to obtain the desired pressure distribution; 能够进行构型的非迭代设计，其中指定所需的压力或切向速度分布。然后程序确定获得所需压力分布所需的“残余”法向流动通过表面。
- (h) the ability to calculate streamlines and to evaluate flow properties at user specified off body points. 能够计算流线并在用户指定的物体外点评估流动特性。

This document has been structured to provide an overview of the theory of potential flow in general and PANAIR in particular, with detailed mathematical formulations reserved for the appendices. 本文的结构旨在提供势流理论的概述，特别是关于 PANAIR 的概述，详细的数学公式留待附录部分介绍。

论文结构：
- section 2 contains a brief discussion on fluid dynamics, outlining without proofs the steps from the Navier-Stokes equations to the linear differential equation solved by PANAIR.
- section 3 discusses the general theory of panel methods without discussing PANAIR in particular.
- section 4 is an overview of PANAIR as it compares to older panel methods.
- section 5 is devoted specifically to PANAIR.

# 2. fundamental fluid dynamics

In this section, we will outline the process by which one arrives at a second order linear partial differential equation, called the ==Prandtl-Glauert== equation, which describes steady, irrotational, inviscid flow in a perfect fluid.

Our starting point is the Navier-Stokes equations, which describe flow in a fluid under very general circumstances.

The assumption that viscosity can be neglected permits the Navier-Stokes equations to be replaced by a simpler system of equations including a "==continuity equation==," a "momentum equation," two "energy equations," and "Euler's equation." 

The further assumptions of "irrotationality" and "isentropic flow" lead to the "unsteady potential equation." 

The assumption of steady flow leads to the "steady non-linear potential equation."

Finally, the "small perturbation assumption" leads to the "Prandtl-Glauert equation."

The remainder of this document will deal with the numerical solution of the latter equation.

## 2.5 the Prandtl-Glauert equation

if $M_\infty=0$, the unsteady potential equation reduces to ==Laplace's equation==

(2.5.1)$$\nabla^2\phi=0$$a linear partial differential equation.

if $M_\infty\neq0$, the unsteady potential equation reduces to a linear differential equation provided additional assumptions are made.
suppose (==small perturbation assumptions==)
(2.5.2)$$M_\infty^2|\vec{v}|\ll1-M_\infty^2$$
and
(2.5.3)$$M_\infty^2|\vec{v}|\ll1$$under those assumptions, the steady non-linear potential equation reduces to the ==Prandtl-Glauert== equation:
(2.5.4)$$\left(1-M_\infty^2\right)\phi_{xx}+\phi_{yy}+\phi_{zz}=0$$
equations (2.5.2) and (2.5.3) should be considered carefully by any user of PANAIR, since they best indicate when PANAIR will provide a reasonable analysis of the flow about a configuration.
==equation (2.5.2) clearly cannot be satisfied for $M_\infty\approx1$==, and thus the ==Prandtl-Glauert equation does not describe "transonic" flow==.
equation (2.5.3) does not hold for $M_\infty\gg1$, and so the ==Prandtl-Glauert equation does not describe "hypersonic" flow==.

The remainder of this document will deal with the solution of the Prandtl-Glauert equation. Using ==Green's theorem==, (2.5.4) is used to derive an integral representation formula where the integrals extend over the configuration surface.
本文档的其余部分将讨论 Prandtl-Glauert 方程的解。利用**格林定理**，公式(2.5.4)被用来推导一个积分表示公式，其中积分在构型表面上进行。
Additional assumptions are then brought to bear in order to obtain an integral equation on the configuration surface.

The integral equation is then solved by a =="discretization" process==: 
	the configuration surface is divided into panels,
	"boundary conditions" are imposed at a discrete set of points, 
	and a system of linear equations is generated.
The system of equations is solved, and data of aerodynamic interest is calculated from that solution.

# 3. panel method theory

in this section, we outline the process by which the Prandtl-Glauert equation 在本节中，我们概述了将 P-G 方程转换为积分方程的过程，以及通用面元法如何求解该积分方程。
(3.0.1)$$\left(1-M_\infty^2\right)\phi_{xx}+\phi_{yy}+\phi_{zz}=0$$
is converted to an ==integral equation==, and the way in which a general panel method solves that integral equation.

In section 3.1, we describe the Prandtl-Glauert scale transformation by which equation (3.0.1) is converted to either Laplace's equation ($M_\infty<1$) or the wave equation ($M_\infty>1$).

In section 3.2, we state ==Green's third identity== which provides a representation formula for $\phi$ in the subsonic case ($M_\infty<1$). For the subsonic case, a simple problem is then formulated showing how the integral representation formula leads to an integral equation.
在第3.2节中，我们陈述了格林第三恒等式，它提供了亚音速情况下（$M_\infty<1$）的表示公式。对于亚音速情况，然后提出了一个简单的问题，说明积分表示公式是如何导致积分方程的。

Finally, in section 3.3, we describe the discretization process by which a panel method solves the resulting integral equation.
最后，在第3.3节中，我们将描述面元法求解所得积分方程的离散化过程。
## 3.1 coordinate scaling 坐标缩放
Equation (3.0.1) is further simplified by performing a scaling of the coordinate system.
通过对坐标系进行缩放，方程 (3.0.1) 得到了进一步简化。
If we define the flow type indicator $s$ by:
(3.1.1)$$s=\mathrm{sign}(1-M_\infty^2)$$
and the ==compressibility scale factor== $\beta$ by:
(3.1.2)$$\beta=\sqrt{s(1-M_\infty^2)}$$
then the scaled coordinates we require are given by
(3.1.3)
$$\bar{x}=x$$$$\bar{y}=\beta y$$$$\bar{z}=\beta z$$
In this new, scaled coordinate system, (3.0.1) can be written
（3.1.4）$$s\phi_{\bar{x}\bar{x}}+\phi_{\bar{y}\bar{y}}+\phi_{\bar{z}\bar{z}}=0$$
But equation (3.1.4) is just the same as (3.0.1) with $M_\infty=0$ or $M_\infty=\sqrt{2}$.
Thus, the subsonic case reduces to the $M_\infty=0$ case while the supersonic case reduces to the $M_\infty=\sqrt{2}$ case.
Equation (3.1.4) is called ==Laplace's equation== if $s=1$, and the ==wave equation== if $s=-1$.
These equations occur in other branches of physics (for instance, Laplace's equation occurs in electrostatics), and thus PANAIR potentially has applications in fields other than fluid mechnaics.
但是，方程 (3.1.4) 与 (3.0.1) 相同，只是在 $M_\infty = 0$ 或 $M_\infty = \sqrt{2}$ 的情况下。  
因此，亚音速情况简化为$M_\infty = 0$的情况，而超音速情况简化为 $M_\infty = \sqrt{2}$ 的情况。  
当 $s=1$时，方程 (3.1.4) 被称为 **拉普拉斯方程**，而当 $s=-1$时，则称为 **波动方程**。  
这些方程在物理学的其他领域中也有出现（例如，拉普拉斯方程出现在静电学中），因此 PANAIR 程序有可能在流体力学以外的领域中得到应用。

For the rest of section 3, we will assume $M_\infty=0$ (note, incidentally, that this does not mean $|\vec{v_\infty}|=0$; rather, $|\vec{v_\infty}|=1$ and the freestream speed of sound $a_\infty$ is infinite).
A similar discussion, for the case $M_\infty=\sqrt{2}$, is given in *Ward*.
The integral representation formula (3.2.7) which results may be generalized to arbitrary subsonic and supersonic Mach number, as discussed by Ward in section 2.8 and 2.10.
在第3节的其余部分，我们将假设 $M_\infty=0$（顺便提一下，这并不意味着$|\vec{v_\infty}|=0$；相反，$|\vec{v_\infty}|=1$，且自由流声速$a_\infty$是无穷大的）。关于$M_\infty=\sqrt{2}$的类似讨论在 *Ward* 中给出。由此产生的积分表示公式 (3.2.7) 可以推广到任意亚音速和超音速马赫数，正如 Ward 在第2.8节和第2.10节中所讨论的那样。

## 3.2 Green's theorems 格林定理

There are a number of theorems, all of them slightly different formulations of the same result, known as Green's theorems.
It is one of these results, often known as ==Green's third identity== which allows us to obtain an integral representation formula for a function $\phi$ satisfying Laplace's equation.
The most fundamental version of these theorems is also known as the "==divergence theorem==," or ==Gauss' Theorem==, which states that if $\vec{F}(\vec{x})$ is a "well-behaved" function (that is, continuously differentiable) on a "nice" region $V$ in space with boundary $S$, then
(3.2.1)$$\iiint\limits_{\mathbf{v}}\nabla\cdot\vec{F}\ \mathrm{dV}\quad=\quad\iint\limits_{\mathbf{S}}\hat{n} \cdot\vec{F}\ \mathrm{dS}$$
where $\hat{n}(\vec{x})$ is an outward-pointing unit normal to the surface.
有许多定理，所有这些定理都是同一结果的略微不同的表述，称为格林定理。其中一个结果，通常被称为**格林的第三恒等式**，使我们能够获得满足拉普拉斯方程的函数 $\phi$ 的积分表示公式。

这些定理中最基本的版本也被称为 **散度定理** 或 **高斯定理**，其表述为：如果 $\vec{F}(\vec{x})$ 是一个在边界为 $S$ 的“良好行为”函数（即连续可微的）且在空间中“好”的区域 $V$ 上定义，那么

$$\iiint\limits_{\mathbf{v}}\nabla\cdot\vec{F}\ \mathrm{dV}\quad=\quad\iint\limits_{\mathbf{S}}\hat{n} \cdot\vec{F}\ \mathrm{dS}$$

其中 $\hat{n}(\vec{x})$ 是指向外的单位法向量。

Green's third identity follows from (3.2.1). We need some notation to state this result, however. Let $U$ be a twice continuously differentiable function in a region $V$ of space. Let $P$ be a point in $V$, $S$ the boundary of $V$, $Q$ an arbitrary point of integration on $S$, and $R=|\vec{P}-\vec{Q}|$. Then
(3.2.2)
$$\begin{aligned}
{U(P)}=& -\frac{1}{4\pi} \iiint\limits_{V}\frac{\nabla^{2}U}{R} \mathrm{d}V_{Q} \\
&-\frac{1}{4\pi}\iint\limits_{S}\frac{\hat{n}\cdot\nabla U}{R} \mathrm{d}S_{Q} \\
&+\frac{1}{4\pi}\iint\limits_{S}U\ \hat{n}\cdot\nabla\frac{1}{R} \mathrm{d}S_{Q}
\end{aligned}$$
>[!question] how equation (3.2.1) -> equation (3.2.2)
>this result is derived in Chapter VIII of *Kellogg*, where opposite signs appear because Kellogg's normal points inward.

Also, (3.2.2a) $$\nabla^2=\nabla\cdot\nabla=\sum^{3}_{i=1}\frac{\partial^2}{\partial x_{i}^2}$$
A number of results follow by substituting into (3.2.2) a function $\phi$ satisfying Laplace's equation 
(3.2.3)$$\nabla^2\phi=0$$

First, letting $P$ approach $S$ we find that $\phi$ is finite as we approach $S$.
首先，当让点 P 逼近边界 S 时，我们发现函数 ϕ 在接近 S 时是有限的。
Thus, $\phi$ is an integrable function over $S$.
因此，ϕ是在边界 S 上的可积函数。
Next, let $V$ be a region consisting of all of space except for a surface $S$, which is thus the boundary of $V$.
接下来，设 V 是一个区域，包含了整个空间，除了一个表面 S，因此 S 就是 V 的边界
![[Pasted image 20241025220306.png]]![[Pasted image 20241025220323.png]]

>[!note] Figure 3.2
>$S$ is a closed surface, and thus $V$ is divided into two regions:
>$V_1$ the "interior" of $S$
>$V_2$ the "exterior"

![[Pasted image 20241026183822.png]]
>[!note] Figure 3.3
>$S$ is not closed, and thus $V$ consists of a single region.
>Let us define the =="upper"== surface of $S$ as the surface bounding that portion of $V$ into which $n$ points, where $\hat{n}$ is the ==outward-pointing normal== for a closed surface, and may be chosen arbitrarily otherwise.

Let us write $\phi_U$ and $\phi_L$ to denote the limiting values of $\phi$ at a point on $S$, approaching from above and below.
Then:
(3.2.4)$$\phi(P)=-\frac{1}{4\pi}\iint\limits_{S}\left[\frac{\hat{n}\cdot(\nabla\phi_U-\nabla\phi_L)}{R}-(\phi_U-\phi_L)\hat{n}\cdot\nabla\frac{1}{R}\right]\mathrm{d}S_Q$$
Equation (3.2.4) is the ==fundamental integral representation formula== which a panel method uses to obtain a solution to the potential flow problem.
公式 (3.2.4) 是**基本积分表示公式**，面元法利用它来求解势流问题。

When combined with approximate "boundary condition"(see below), the formula (3.2.4) can be manipulated to yield an integral equation on the singularity surface $S$.

A panel method then obtains an approximate solution of this integral equation by means of the numerical method of collocation. Two functions defined on $S$ are generally introduced because of their importance in the manipulation of (3.2.4).
面元法随后通过配置数值方法获得该积分方程的近似解。通常会在 S 上定义两个函数，因为它们在操作公式 (3.2.4) 中非常重要。

The first is the "==source strength==", defined by
(3.2.5)$$\sigma(Q)=\hat{n}\cdot[\nabla\phi_U(Q)-\nabla\phi_L(Q)]$$
and the second is the "==doublet strength==", defined by 
(3.2.6)$$\mu(Q)=\phi_U(Q)-\phi_L(Q)$$
==These quantities are often called "**singularity strengths**," because they measure the singular behavior of $\phi$ on $S$==.

Using these quantities, (3.2.4) becomes
(3.2.7)$$\phi(P)=-\frac{1}{4\pi}\iint\limits_{S}\left[\frac{\sigma}{R}-\mu\hat{n}\cdot\nabla\frac{1}{R}\right]\mathrm{d}S$$
As mentioned above, equation (3.2.7) must be supplemented with ==boundary conditions== in order to obtain the integral equation that is solved by PANAIR. 
如上所述，为了得到由 PANAIR 求解的积分方程，公式 (3.2.7) 必须加上**边界条件**。

Generally, these boundary conditions are equations relating $\phi$, $\sigma$, $\mu$ and their derivative on $S$.

The specification of boundary conditions in conjunction with (3.2.7) amounts to a formulation of a "boundary value problem." This problem in turn is called "well-posed" if it has a unique solution, and "ill-posed" otherwise.
结合公式 (3.2.7) 指定边界条件，相当于构建了一个“边界值问题”。如果该问题有唯一解，则称之为“适定问题”；否则，称之为“不适定问题”。

A typical example of a set of boundary conditions leading to boundary value problem formulation might be (see figure 3.2) 
(3.2.8)$$\phi_L=0$$
combined with 
(3.2.9)$$\begin{aligned}\nabla\phi_U\cdot\hat{n}&=b \\ 
&=-\vec{V}_\infty\cdot\hat{n}\end{aligned}$$
It can be shown that the combination of (3.2.7) with the specification of the boundary conditions (3.2.8) and (3.2.9) on the configuration in figure 3.2 is a well-posed boundary value problem.

In fact, the boundary condition (3.2.8) and (3.2.9) constitute the "[[Morino formulation]]" of the potential flow problem.
事实上，边界条件 (3.2.8) 和 (3.2.9) 构成了势流问题的“**Morino 公式**”。

Referring again to figure (3.2), we see that the boundary condition (3.2.8) implies that $\phi=0$ for all points interior to $V_1$. This follows from the general uniqueness result for solutions of Laplace's equation with Dirichlet boundary condition. Consequently we find that 
(3.2.10) $$\nabla\phi_L\cdot\hat{n}=0$$
Substituting this and (3.2.9) into (3.2.5) yield for $\sigma$,
(3.2.11)$$\sigma=-\vec{V}_\infty\cdot\hat{n}$$
Note as well that $\phi_U$ is equal to the doublet strength $\mu$; for, combining (3.2.6) and (3.2.8) we get
(3.2.12) $$\mu=\phi_U-\phi_L=\phi_U-0=\phi_U$$
We can now obtain the integral equation mentioned above.
Evaluating equation (3.2.7) on the upper surface of $S$, we obtain after using (3.2.11) and (3.2.12)
(3.2.13) $$\mu(P)-\frac{1}{4\pi}\left(\iint\limits_{S}\mu\hat{n}\cdot\nabla\frac{1}{R}\mathrm{d}S\right)_U=\frac{1}{4\pi}\iint\limits_{S}\frac{\vec{V}_\infty\cdot\hat{n}}{R}\mathrm{d}S$$
When proper care is taken to evaluate the integral appearing on the left hand side on the upper surface of $S$, this equation is the integral equation for $\mu(Q)$ that is solved by PANAIR, given the Morino formulation of the boundary value problem.
当仔细评估出现在 S 上表面的左侧积分时，该方程是由 PANAIR 求解的关于$\mu(Q)$的积分方程，基于 Morino 公式的边界值问题

## 3.3 Discretization

We now outline the discretization process by which a panel method solves the integral equation obtained by combining (3.2.7) with a properly posed set of boundary conditions.

In point of fact we will not actually describe the integral equation formulation of the potential flow problem.

Rather, we shall describe in an operational way the process by which PANAIR transforms a specific boundary condition imposed at a particular point into a constraint relation imposed on a set of singularity parameters.
相反，我们将以操作性的方式描述 PANAIR 如何将施加在特定点上的特定边界条件转化为对一组奇异元参数施加的约束关系。
This point of view is consistent with the actual operation of PANAIR, in which the problem formulation is implicitly left as a task to the user.


# 4. An overview of PANAIR

## 4.1 Historical development of panel methods

features
- continuous geometry
- linear source and quadratic doublet variation
- continuity of doublet strength
these features make PANAIR more accurate and reliable than previous methods.

>[!attention] the effects of "gaps"
>in subsonic flow, the gaps cause little numerical error,
>but in supersonic flow, the cumulative effect of the gaps is serious

PANAIR employs a ==linear source variation== and a ==quadratic doublet variation==: the basis function $b_i$ corresponding to a source parameter is locally linear, while the basis function corresponding to a doublet parameter is locally quadratic.

## 4.2 summary of PANAIR technology
(4.2.1)$$\phi_{A}(P) =\sum^{N}_{j=1}\llcorner\phi IC(P)\lrcorner_{j}\lambda_{j}=\llcorner\phi IC(P)\lrcorner\vec{\lambda}$$
(4.2.2)$$\left(\vec{V}_{A}(P)\right)_i=\sum^{N}_{j=1}[VIC(P)]_{ij}\lambda_{j}=([VIC(P)]\vec{\lambda})_i,\ i=1,2,3$$
$\llcorner\phi IC(P)\lrcorner$ : "potential influence coefficient" row vector
$[VIC(P)]$: "velocity influence coefficient" matrix

finally, a fairly general boundary condition of the form (4.2.3) leads to a row $\llcorner AIC(P)\lrcorner$ of $[AIC]$ as follows:$$a_{A}\vec{V}_{A}\cdot n+c_{A}\phi_{A}+\vec{t}_{A}\cdot\vec{V}_{A}=b$$



---
NOTE
>[!NOTE]

SUMMARY
>[!SUMMARY]

BUG
>[!BUG]

FAILURE
>[!FAILURE]

ATTENTION
>[!ATTENTION]

ERROR
>[!ERROR]

CHECK
>[!CHECK]

TODO
>[!TODO]

QUESTION
>[!QUESTION]












