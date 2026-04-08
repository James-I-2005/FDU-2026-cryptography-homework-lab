# 密码学作业 3：完美保密等价定义证明

## 背景与定义

**Definition 2.1（完美保密）**

设加密方案 $\Pi = (\text{Gen}, \text{Enc}, \text{Dec})$ 的明文空间为 $\mathcal{M}$，密文空间为 $\mathcal{C}$，密钥空间为 $\mathcal{K}$。称 $\Pi$ 是**完美保密**的，若对明文空间 $\mathcal{M}$ 上的任意概率分布、任意 $m \in \mathcal{M}$，以及任意满足 $\Pr[C = c] > 0$ 的 $c \in \mathcal{C}$，均有：

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

---

## 题目 1：证明 Lemma 2.2 的必要性

**命题：** 若加密方案 $\Pi$ 是完美保密的（满足 Definition 2.1），则对于 $\mathcal{M}$ 上的任意分布、任意 $m \in \mathcal{M}$ 以及任意 $c \in \mathcal{C}$，均有：

$$\Pr[C = c \mid M = m] = \Pr[C = c]$$

**证明：**

分两种情形讨论。

**情形 1：$\Pr[C = c] = 0$**

由于 $C$ 由 $M$ 和密钥 $K$ 共同决定，且 $K$ 独立于 $M$，有：

$$\Pr[C = c] = \sum_{m' \in \mathcal{M}} \Pr[C = c \mid M = m'] \cdot \Pr[M = m']$$

若 $\Pr[C = c] = 0$，则上式中每一项均非负且求和为 $0$，故对所有满足 $\Pr[M = m'] > 0$ 的 $m'$，都有 $\Pr[C = c \mid M = m'] = 0$。

特别地，对任意 $m \in \mathcal{M}$（无论 $\Pr[M=m]$ 是否为正），均有：

$$\Pr[C = c \mid M = m] = 0 = \Pr[C = c]$$

结论成立。

**情形 2：$\Pr[C = c] > 0$**

由贝叶斯公式：

$$\Pr[M = m \mid C = c] = \frac{\Pr[C = c \mid M = m] \cdot \Pr[M = m]}{\Pr[C = c]}$$

由完美保密性（Definition 2.1），对满足 $\Pr[C = c] > 0$ 的 $c$，有：

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

将完美保密条件代入贝叶斯公式：

$$\Pr[M = m] = \frac{\Pr[C = c \mid M = m] \cdot \Pr[M = m]}{\Pr[C = c]}$$

**子情形 2a：$\Pr[M = m] > 0$**

两边同除以 $\Pr[M = m]$（合法，因其大于 $0$），得：

$$1 = \frac{\Pr[C = c \mid M = m]}{\Pr[C = c]}$$

即：

$$\Pr[C = c \mid M = m] = \Pr[C = c]$$

**子情形 2b：$\Pr[M = m] = 0$**

此时 $\Pr[C = c \mid M = m]$ 的取值不影响任何联合概率，但由于加密方案的密钥分布与明文无关，$\Pr[C = c \mid M = m]$ 由密钥分布唯一确定，与 $\Pr[M=m]$ 的取值无关。

更严格地，注意到 $\Pr[C = c \mid M = m]$ 仅依赖于加密算法 $\text{Enc}$ 和密钥分布 $\text{Gen}$，而与明文的先验分布无关。对任意使得 $\Pr[M = m] > 0$ 的分布（例如令 $M$ 以概率 $1$ 取值 $m$），由子情形 2a 已证 $\Pr[C = c \mid M = m] = \Pr[C = c]$（注意此时 $\Pr[C=c]$ 仅由密钥分布决定，与明文分布无关）。故对原分布同样有：

$$\Pr[C = c \mid M = m] = \Pr[C = c]$$

综合两种情形，对任意分布、任意 $m \in \mathcal{M}$、任意 $c \in \mathcal{C}$，均有：

$$\boxed{\Pr[C = c \mid M = m] = \Pr[C = c]}$$

$\blacksquare$

---

## 题目 2：证明 Lemma 2.3 的充分性

**命题：** 若对 $\mathcal{M}$ 上的任意分布、任意 $m \in \mathcal{M}$ 以及任意 $c \in \mathcal{C}$，均有：

$$\Pr[C = c \mid M = m] = \Pr[C = c]$$

则加密方案 $\Pi$ 是完美保密的，即对任意满足 $\Pr[C = c] > 0$ 的 $c \in \mathcal{C}$，均有：

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

**证明：**

设 $c \in \mathcal{C}$ 满足 $\Pr[C = c] > 0$，$m \in \mathcal{M}$ 任意。

由贝叶斯公式：

$$\Pr[M = m \mid C = c] = \frac{\Pr[C = c \mid M = m] \cdot \Pr[M = m]}{\Pr[C = c]}$$

将已知条件 $\Pr[C = c \mid M = m] = \Pr[C = c]$ 代入分子：

$$\Pr[M = m \mid C = c] = \frac{\Pr[C = c] \cdot \Pr[M = m]}{\Pr[C = c]}$$

由于 $\Pr[C = c] > 0$，可以约分，得：

$$\Pr[M = m \mid C = c] = \Pr[M = m]$$

**验证分母的合理性（利用全概率公式）：**

为确保上述推导的自洽性，验证 $\Pr[C = c]$ 的表达式与已知条件一致。由全概率公式：

$$\Pr[C = c] = \sum_{m' \in \mathcal{M}} \Pr[C = c \mid M = m'] \cdot \Pr[M = m']$$

将已知条件 $\Pr[C = c \mid M = m'] = \Pr[C = c]$ 代入：

$$\Pr[C = c] = \sum_{m' \in \mathcal{M}} \Pr[C = c] \cdot \Pr[M = m'] = \Pr[C = c] \cdot \sum_{m' \in \mathcal{M}} \Pr[M = m'] = \Pr[C = c] \cdot 1 = \Pr[C = c]$$

等式恒成立，说明已知条件与概率公理相容，推导过程无矛盾。

因此，对任意满足 $\Pr[C = c] > 0$ 的 $c \in \mathcal{C}$ 以及任意 $m \in \mathcal{M}$，均有：

$$\boxed{\Pr[M = m \mid C = c] = \Pr[M = m]}$$

即方案 $\Pi$ 满足 Definition 2.1，是完美保密的。

$\blacksquare$
