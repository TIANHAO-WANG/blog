# The Channel-Convexity of Channel Capacity

I've been working on a project related to information theory lately. When I'm reading Cuff & Yu's excellent paper [Differential Privacy as a Mutual Information Constraint](https://arxiv.org/abs/1608.03677), in Appendix C (the proof of lemma 2) they claim that "*channel capacity is a convex function of
the channel parameters*". The proof of lemma 2 (a key result of the paper) is based on this fact, and they didn't prove it in the paper because of space constraint. 

In this article, I will give a proof for this proposition:
 
- the first part of the proof is by showing *the Mutual Information is a convex function of the channel parameters*.
- the second part of the proof is handling the maximum in the channel capacity's formula. 

Both of the proofs are short, clean, and insightful. 

## Problem Statement
### Background
Given two finite sets $X$ and $Y$, a discrete memoryless channel is a probability transition matrix $p(y|x)$ that expresses the probability of observing the output symbol $y$ given that we send symbol $x$. The channel is said to be *memoryless* because the probability distribution of the output depends only on the input at that time and is conditionally independent of previous channel inputs or outputs [1]. Because the channel consists of $|X|$ probability distributions over a set of $|Y|$ elements, the channel can be represented by an $|Y| \times |X|$ transition matrix $Q$: 
$$
Q_{yx} = p(y|x)
$$
By representing $P_X$ and $P_Y$ as vectors, this relation can be written as a matrix multiplication: 
$$
P_Y = QP_X
$$
The mutual information $I(X, Y)$ between $X$ and $Y$ represents the *average* amount of information about $Y$ that is gained about observing $X$; phrased differently, it is the amount of uncertainty (entropy) about $Y$ that are reduced by observing $X$. Formally: 
$$
I(X;Y) = H(X) - H(X|Y) = H(Y) - H(Y|X) 
$$  
In the above equation, $H(Y)$ is the entropy of random variable $Y$, and $H(Y|X)$ is the conditional entropy of $Y$ given $X$:
$$
H(Y) = -\sum_y Pr[Y=y]\log(Pr[Y=y])
$$
$$
H(Y|X) = \sum_x Pr[X=x]H(Y|X=x)
$$

The "information" channel capacity of a discrete memoryless channel is 
$$
C(p_{y|x}) = \max_{p(x)} I(X, Y)  
$$
where the maximum is taken over all possible distributions over the domain of $X$. 

### Want to prove
> The function $C: Q \rightarrow C(Q)$ is convex in $Q$. 

In details, this means that given 2 channels $Q^{(0)}$ and $Q^{(1)}$ and a weight $\lambda \in (0, 1)$ we can define a composite channel $Q^{(\lambda)} = \lambda Q^{(0)} + (1-\lambda) Q^{(1)}$, and we have 
$$ 
C(Q^{(\lambda)}) \le \lambda C(Q^{(0)}) + (1-\lambda) C(Q^{(1)})
$$

Intuitive explanation: with regards to the capacity of transmitting information, mixing 2 channels is always worse than only using the most informative of the 2 channels. In particular, mixing 2 fully-deterministic channels can result in a noisy channel. 

## Proof
### Channel-Convexity of Mutual Information
We first prove the convexity of mutual information in terms of channels since channel capacity is a mutual information that takes the maximum of distribution over $X$. To prove this proposition, we revisit the notion of "mixing two channels", by interpreting mixing channel parameters as a new protocol of communication: when we send message $x$, a random bit $z \in \{0, 1\}$ is independently chosen, with $Pr[z=1] = \lambda$. If $z=0$, $x$ is sent through channel $Q^{(0)}$, otherwise through channel $Q^{(1)}$. 

We can now derive the proof by starting with an incredibly intuitive argument: knowing both $y$ and $z$ gives us more information on $x$ than knowing $y$ alone. Formally, 
$$
H(X|Y, Z) \le H(X|Y)
$$
$$
\implies Pr[z=0]H(X|Y, z=0) + Pr[z=1]H(X|Y, z=1) 
\le H(X|Y)
$$
$$
\implies (1-\lambda)H(X|Y)(Q^{(0)}) + 
\lambda H(X|Y)(Q^{(1)}) \le H(X|Y)(Q^{(\lambda)})
$$
$$
\implies (1-\lambda)(H(X)-H(X|Y)(Q^{(0)})) + 
\lambda (H(X)-H(X|Y)(Q^{(1)})) \ge H(X) - H(X|Y)(Q^{(\lambda)})
$$
$$
\implies (1-\lambda)I(X;Y)(Q^{(0)}) + 
\lambda I(X;Y)(Q^{(1)}) \ge I(X;Y) (Q^{(\lambda)})
$$
The last line expresses the convexity of $I(Q)$. 

### Extending to Channel Capacity
Mutual information $I(X, Y)$ can be viewed as a bivariate function $f(x, y)$ where $x=P_X$ and $y=P_{Y|X}$ (channel), which is both concave in $x$ and convex in $y$. We show that the channel capacity $C(P_{Y|X})=\max_{P_X}f(P_X, P_{Y|X})$ is a function that is concave in $P_{Y|X}$. 

Let $g(y)=\max_x f(x, y)$, then for all $\lambda \in (0, 1)$ we have 
$$
\begin{align}
g(\lambda y_1 + (1-\lambda) y_2) &= f(x^*, \lambda y_1 + (1-\lambda)y_2) \\
&\le \lambda f(x^*, y_1) + (1-\lambda)f(x^*, y_2) \\
&\le \lambda f(x_1^*, y_1) + (1-\lambda)f(x_2^*, y_2) \\
&= \lambda g(y_1) + (1-\lambda)g(y_2)
\end{align}
$$

where 

$$
\begin{align}
x^* &= arg\max_{x} f(x, \lambda y_1 + (1-\lambda)y_2) \\
x_1^* &= arg\max_{x} f(x, y_1) \\
x_2^* &= arg\max_{x} f(x, y_2)
\end{align}
$$

 

## References 
[1] Cover, Thomas M. Elements of information theory. John Wiley & Sons, 1999. 