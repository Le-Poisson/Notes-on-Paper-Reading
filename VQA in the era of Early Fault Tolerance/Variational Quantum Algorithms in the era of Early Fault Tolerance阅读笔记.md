## Variational Quantum Algorithms in the era of Early Fault Tolerance 阅读笔记

## 前置知识

### VQA

VQA（Variational Quantum Algorithm，变分量子算法）是量子计算中一种结合经典计算与量子计算的混合算法框架，特别适用于当前含噪声中等规模量子（NISQ, Noisy Intermediate-Scale Quantum）设备。

#### NISQ

Noisy（含噪声）：量子门操作存在误差，相干时间短，测量不完美，无法长时间维持量子态。

Intermediate-Scale（中等规模）：通常指50–1000 个物理量子比特，足以超越经典模拟（在某些任务上），但远不足以运行容错量子算法（如 Shor 算法）。

NISQ 设备的主要特点：

- 无量子纠错：不使用逻辑量子比特或表面编码等容错技术，所有操作直接在物理量子比特上进行。
- 有限连通性：并非所有量子比特之间都能直接相互作用，需通过 SWAP 操作间接连接，增加开销和错误。
- 需要经典协同：常与经典计算机结合使用（如 VQA、QAOA 等混合算法），以弥补硬件限制。

#### 算法流程

VQA 通常遵循以下步骤：

1. **初始化参数**  
   设定参数化量子电路中的可调参数（如旋转角度）的初始值。

2. **量子计算部分**  
   - 在量子设备上运行参数化电路，制备一个量子态 \(|\psi(\boldsymbol{\theta})\rangle\)。  
   - 测量该态以获得目标函数（例如哈密顿量的期望值 \(\langle \psi(\boldsymbol{\theta}) | H | \psi(\boldsymbol{\theta}) \rangle\)）。

3. **经典优化部分**  
   - 将测量结果传给经典计算机。  
   - 使用经典优化算法（如梯度下降、Adam 等）更新参数 \(\boldsymbol{\theta}\)，以最小化目标函数。

4. **迭代**  
   重复步骤 2–3，直到收敛或达到停止条件。

#### OPR

Optimal Parameter Resilience（OPR，最优参数鲁棒性） 是变分量子算法（Variational Quantum Algorithms, VQAs）中一个重要的理论性质：即使我们在有噪声的中等规模量子设备（NISQ 设备）上运行 VQA 并进行参数优化，所找到的“最优”参数值，在理想情况下（没有噪声）依然表现良好。

- 意义：我们无法在当前硬件上完全消除噪声，但如果 OPR 成立，就意味着即使在噪声环境下训练出的模型，在理论上（或未来低噪设备上）依然有效。
- 局限：当噪声水平过高时，测量到的 loss 值会被严重扭曲，导致优化器被误导，找到的“最优参数”在无噪情况下表现很差。

#### VQE

VQE（Variational Quantum Eigensolver，变分量子本征求解器）是 VQA 的一种特例，专门用于近似求解给定哈密顿量的基态能量（即最小本征值），其对应的量子态 $\ket{ψ(θ)}$ 近似为基态。

目标是最小化哈密顿量的期望值 \(\langle \psi(\boldsymbol{\theta}) | H | \psi(\boldsymbol{\theta}) \rangle\)。

### 通用量子门集

$Clifford+T$ 门可以逼近任何酉操作。然而，要达到高精度，必须将非$Clifford$ 量子门分解为由 $Clifford+T$ 门组成的长序列，而更高的精度往往意味着更长的门序列。

论文考虑使用 $Clifford+Rz$ 门结合 VQA 逼近任何酉操作。

### 量子态蒸馏

#### FTQC

FTQC 范式指的是 容错量子计算（Fault-Tolerant Quantum Computing） 的实现框架或方法论，即用多个易错的物理量子比特编码成一个高保真的“逻辑量子比特”，并通过容错操作实现任意长度的可靠量子计算。

例如：shor码（9物理比特）和表面码（Surface Code）

#### 表面码（Surface Code）

参考 `表面码笔记.md`。

#### Eastin–Knill 定理

任何量子纠错码，如果能以“横向”（transversal）方式实现所有通用量子门，那么它就无法纠正任意错误。

横向的概念参考 `什么是横向门.md`。

论文讨论如何实现非 Clifford 门操作，尤其是 T 门（论文中称为称为 T 态浓缩）

