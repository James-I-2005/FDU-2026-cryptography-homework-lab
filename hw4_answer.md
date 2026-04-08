# Homework 4：完美保密定义 2.1 与 2.4 等价性证明

**密码学课程作业**  
**2026 年 3 月 27 日**

---

## 1 定义回顾

### 定义 2.1（信息论完美保密）

加密方案 $(\text{Gen}, \text{Enc}, \text{Dec})$ 定义在消息空间 $\mathcal{M}$ 上，若对任意消息分布、任意 $m \in \mathcal{M}$、任意满足 $\Pr[C = c] > 0$ 的密文 $c \in \mathcal{C}$，均满足：

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

则称该加密方案满足**完美保密**。

### 定义 2.4（敌手实验完美保密）

加密方案 $(\text{Gen}, \text{Enc}, \text{Dec})$ 定义在消息空间 $\mathcal{M}$ 上，若对任意敌手 $\mathcal{A}$，在窃听实验 $\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi}$ 中：

1. 敌手 $\mathcal{A}$ 输出两个消息 $m_0, m_1 \in \mathcal{M}$；
2. 运行 $\text{Gen}$ 生成密钥 $k$，随机选取 $b \xleftarrow{\$} \{0,1\}$，计算 $c = \text{Enc}_k(m_b)$；
3. 敌手输出猜测 $b'$，若 $b' = b$ 则实验成功。

实验成功概率满足：

$$\Pr\!\left[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1\right] = \frac{1}{2}$$

则称该加密方案满足完美保密。

---

## 2 等价性证明

### 2.1 方向一：定义 2.1 $\Rightarrow$ 定义 2.4

**命题：** 若加密方案 $\Pi$ 满足定义 2.1，则对任意敌手 $\mathcal{A}$，有

$$\Pr\!\left[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1\right] = \frac{1}{2}$$

**证明：**

设敌手 $\mathcal{A}$ 在实验第一步输出消息对 $(m_0, m_1)$，实验随机选取 $b \xleftarrow{\$} \{0,1\}$ 并发送密文 $c = \text{Enc}_k(m_b)$。敌手观察到密文 $c$ 后输出猜测 $b'$。

对实验成功概率按全概率公式展开：

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1] = \Pr[b' = 0 \mid b = 0]\cdot\Pr[b=0] + \Pr[b' = 1 \mid b = 1]\cdot\Pr[b=1]$$

$$= \frac{1}{2}\Pr[b' = 0 \mid b = 0] + \frac{1}{2}\Pr[b' = 1 \mid b = 1]$$

敌手的策略是：观察密文 $c$，输出使 $\Pr[b = \beta \mid C = c]$ 最大的 $\beta$。

由定义 2.1，对任意 $m \in \mathcal{M}$ 及任意满足 $\Pr[C = c] > 0$ 的密文 $c$，有

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

在实验中，$b$ 均匀随机取自 $\{0,1\}$，故 $m_0, m_1$ 被加密的先验概率各为 $\frac{1}{2}$。

对任意密文 $c$，利用贝叶斯公式：

$$\Pr[b = 0 \mid C = c] = \frac{\Pr[C = c \mid b = 0]\cdot\Pr[b=0]}{\Pr[C = c]}$$

由定义 2.1，密文 $c$ 的分布与明文无关，即：

$$\Pr[C = c \mid b = 0] = \Pr[C = c \mid M = m_0] = \Pr[C = c]$$

$$\Pr[C = c \mid b = 1] = \Pr[C = c \mid M = m_1] = \Pr[C = c]$$

> **此处详细推导：** 由定义 2.1，$\Pr[M = m \mid C = c] = \Pr[M = m]$ 等价于 $M$ 与 $C$ 统计独立，即 $\Pr[C = c \mid M = m] = \Pr[C = c]$ 对所有 $m, c$ 成立（见引理）。

因此：

$$\Pr[b = 0 \mid C = c] = \frac{\Pr[C = c] \cdot \frac{1}{2}}{\Pr[C = c]} = \frac{1}{2}$$

$$\Pr[b = 1 \mid C = c] = \frac{1}{2}$$

这说明无论敌手观察到何种密文 $c$，关于 $b$ 的后验分布仍为均匀分布，密文不提供任何关于 $b$ 的信息。

因此，对任意（确定性或随机性）敌手策略 $\mathcal{A}$：

$$\Pr[b' = 0 \mid b = 0] + \Pr[b' = 1 \mid b = 1] = 1$$

> **说明：** 无论 $\mathcal{A}$ 的策略如何，由于 $\Pr[b=0 \mid C=c] = \Pr[b=1 \mid C=c] = \frac{1}{2}$ 对所有 $c$ 成立，$\mathcal{A}$ 的最优策略等价于随机猜测，故 $\Pr[b'=0 \mid b=0] = \Pr[b'=1 \mid b=1]$，且两者之和为 1，即各为 $\frac{1}{2}$（若 $\mathcal{A}$ 固定输出某值，则一项为 1 另一项为 0，但总和仍为 1，代入后结果相同）。

更严格地，对任意敌手 $\mathcal{A}$：

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1]$$

$$= \sum_{c} \Pr[C = c] \cdot \Pr[b' = b \mid C = c]$$

$$= \sum_{c} \Pr[C = c] \left(\Pr[b'=0 \mid C=c]\cdot\Pr[b=0 \mid C=c] + \Pr[b'=1 \mid C=c]\cdot\Pr[b=1 \mid C=c]\right)$$

$$= \sum_{c} \Pr[C = c] \left(\Pr[b'=0 \mid C=c]\cdot\frac{1}{2} + \Pr[b'=1 \mid C=c]\cdot\frac{1}{2}\right)$$

$$= \sum_{c} \Pr[C = c] \cdot \frac{1}{2}\left(\Pr[b'=0 \mid C=c] + \Pr[b'=1 \mid C=c]\right)$$

$$= \sum_{c} \Pr[C = c] \cdot \frac{1}{2} \cdot 1 = \frac{1}{2}$$

故定义 2.1 $\Rightarrow$ 定义 2.4 得证。$\blacksquare$

---

### 2.2 方向二：定义 2.4 $\Rightarrow$ 定义 2.1

**命题：** 若加密方案 $\Pi$ 满足定义 2.4，则对任意消息分布、任意 $m \in \mathcal{M}$、任意满足 $\Pr[C = c] > 0$ 的密文 $c \in \mathcal{C}$，有

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

**证明（反证法）：**

假设 $\Pi$ 满足定义 2.4，但不满足定义 2.1。则存在某消息分布、某 $m^* \in \mathcal{M}$、某满足 $\Pr[C = c^*] > 0$ 的密文 $c^* \in \mathcal{C}$，使得：

$$\Pr[M = m^* \mid C = c^*] \neq \Pr[M = m^*]$$

分两种情形讨论。

**情形 A：** $\Pr[M = m^* \mid C = c^*] > \Pr[M = m^*]$

构造敌手 $\mathcal{A}$ 如下：

- 选取任意 $m_1 \in \mathcal{M}$，$m_1 \neq m^*$（消息空间 $|\mathcal{M}| \geq 2$，完美保密的必要条件保证此点）；
- 令 $m_0 = m^*$，输出消息对 $(m_0, m_1)$；
- 收到密文 $c$ 后，若 $c = c^*$ 则输出 $b' = 0$，否则随机猜测。

计算 $\mathcal{A}$ 的成功概率：

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1]$$

$$= \Pr[b=0]\cdot\Pr[b'=0 \mid b=0] + \Pr[b=1]\cdot\Pr[b'=1 \mid b=1]$$

当 $b=0$ 时，加密的是 $m_0 = m^*$：

$$\Pr[b'=0 \mid b=0] = \Pr[C = c^* \mid M = m^*] + \Pr[C \neq c^* \mid M = m^*]\cdot\frac{1}{2}$$

当 $b=1$ 时，加密的是 $m_1$：

$$\Pr[b'=1 \mid b=1] = \Pr[C \neq c^* \mid M = m_1]\cdot\frac{1}{2} + \Pr[C = c^* \mid M = m_1]\cdot 0$$

$$= \frac{1}{2}\Pr[C \neq c^* \mid M = m_1]$$

为简化分析，考虑更直接的构造：

**简化构造：** 令消息分布为均匀分布于 $\{m_0, m_1\} = \{m^*, m_1\}$。

由条件 $\Pr[M = m^* \mid C = c^*] > \Pr[M = m^*]$，利用贝叶斯公式：

$$\Pr[M = m^* \mid C = c^*] = \frac{\Pr[C = c^* \mid M = m^*]\cdot\Pr[M = m^*]}{\Pr[C = c^*]}$$

故 $\Pr[M = m^* \mid C = c^*] > \Pr[M = m^*]$ 等价于：

$$\Pr[C = c^* \mid M = m^*] > \Pr[C = c^*] \tag{$*$}$$

构造敌手 $\mathcal{A}$：输出 $(m_0, m_1) = (m^*, m_1)$；收到密文 $c$ 后，若 $c = c^*$ 则猜 $b' = 0$，否则猜 $b' = 1$。

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1]$$

$$= \frac{1}{2}\Pr[C = c^* \mid M = m^*] + \frac{1}{2}\Pr[C \neq c^* \mid M = m_1]$$

$$= \frac{1}{2}\Pr[C = c^* \mid M = m^*] + \frac{1}{2}(1 - \Pr[C = c^* \mid M = m_1])$$

$$= \frac{1}{2} + \frac{1}{2}\left(\Pr[C = c^* \mid M = m^*] - \Pr[C = c^* \mid M = m_1]\right)$$

由全概率公式（在均匀分布 $\Pr[M=m^*]=\Pr[M=m_1]=\frac{1}{2}$ 下）：

$$\Pr[C = c^*] = \frac{1}{2}\Pr[C = c^* \mid M = m^*] + \frac{1}{2}\Pr[C = c^* \mid M = m_1]$$

由 $(*)$ 式：

$$\Pr[C = c^* \mid M = m^*] > \Pr[C = c^*] = \frac{1}{2}\Pr[C = c^* \mid M = m^*] + \frac{1}{2}\Pr[C = c^* \mid M = m_1]$$

化简得：

$$\Pr[C = c^* \mid M = m^*] > \Pr[C = c^* \mid M = m_1]$$

因此：

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1] = \frac{1}{2} + \frac{1}{2}\underbrace{\left(\Pr[C = c^* \mid M = m^*] - \Pr[C = c^* \mid M = m_1]\right)}_{> 0} > \frac{1}{2}$$

这与定义 2.4 矛盾。

**情形 B：** $\Pr[M = m^* \mid C = c^*] < \Pr[M = m^*]$

类似地，此时 $\Pr[C = c^* \mid M = m^*] < \Pr[C = c^*]$，即存在某 $m_1$ 使得 $\Pr[C = c^* \mid M = m_1] > \Pr[C = c^* \mid M = m^*]$。

构造敌手 $\mathcal{A}$：输出 $(m_0, m_1)$；收到密文 $c$ 后，若 $c = c^*$ 则猜 $b' = 1$，否则猜 $b' = 0$。

同理可得：

$$\Pr[\text{PrivK}^{\text{eav}}_{\mathcal{A}, \Pi} = 1] = \frac{1}{2} + \frac{1}{2}\left(\Pr[C = c^* \mid M = m_1] - \Pr[C = c^* \mid M = m^*]\right) > \frac{1}{2}$$

同样与定义 2.4 矛盾。

**结论：** 两种情形均导致矛盾，故假设不成立。因此，满足定义 2.4 的加密方案必然满足定义 2.1。$\blacksquare$

---

## 3 总结

综合两个方向的证明：

$$\text{定义 2.1} \Rightarrow \text{定义 2.4} \quad \text{（2.1节）}$$

$$\text{定义 2.4} \Rightarrow \text{定义 2.1} \quad \text{（2.2节）}$$

因此，**定义 2.1 $\Longleftrightarrow$ 定义 2.4**，两种完美保密定义完全等价。$\blacksquare$
