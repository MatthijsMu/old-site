Derivation of the student's $t$-distribution

When I followed the course Statistics in my mathematics 
bachelor, the subject was approached from a 
probability-theoretical perspective. Given a zoo of distributions
from probability theory, how (and to what degree of accuracy)
can we infer the parameters from a finite sample? Yet, despite
this perspective, we never formally derived the student's $t$-distribution.

Since I myself am inclined to only believe
these things better if I can explain the
mathematics backing them, I decided to look for a derivation
myself. I found one in *Statistics for Mathematicians* by
Panaretos (2016). It will be discussed here.



> Note: I will use the notation $f_X$ for the density function of
an absolutely continuous real random variable $X$.

> Note 2: the mean of an i.i.d. sample $X_1, ..., X_n$ is denoted $\overline X$
and the sample variance $S^2 = \frac 1 {n-1} \sum_{i=1}^n (X_i-\overline X)^2
$

The steps taken are:

1. Define the density of $t_n$ and $\chi^2_n$. I assume that you know the distributions $N(\mu,\sigma^2)$ and $N_n(\mu, \sigma^2)$ yourself.

2. prove a that $S^2$ and $\overline X$ are independent and follow a $\chi^2_{n-1}$ and $N(\mu, \sigma^2)$ distribution, respectively.

3. derive the density of $\chi_n^2$ using convolutions

4. derive the density of $t_n$

## Definition and main theorem

Here is the definition of the student's t distribution:

> **Definition 1**.
>
> A real random variable $X$ is distributed according to
> a $t$-distribution with $n$ degrees of freedom, denoted $X \sim t_{n-1}$,
> iff.
> $$ f_X(x) = \frac {\Gamma(\frac{k+1}{2})}{\Gamma(\frac k2)\sqrt{k\pi}}(1 +\frac {x^2}k)^{-\frac {k+1}2}

$\Gamma$ is the gamma-function:
$$\Gamma(z) := \int_0^\infty t^{z-1}e^{-t}dt$$
One can show that this improper integral converges for arbitrary $z \in \mathbb C$ s.t. $Re(z)>0$, but that belongs to a course in (Complex) Analysis (something I will do next semester). 

Given its existence, we can derive identities such as $\Gamma (z+1) = z\Gamma(z)$ using partial integration.

> **Theorem**
>
> Let $X_1, ... , X_n \sim^{iid} N(\mu, \sigma^2)$. Then
> $$ \frac {\overline X - \mu} {S / \sqrt{n}} \sim t_{n-1}$$

## Lemmas

The first lemma that is typically proven:

> **Lemma**
>
> $S$ and $\overline X$ are independently distributed, and 
> $$\overline X \sim N(\mu, \sigma^2/n)$$
> $$\frac {n-1}{\sigma^2}S^2 \sim \chi_{n-1}^2

**Proof**:

An analogous theorem from the situation of linear regression,
where we have the more general situation: $$Y = X \beta + U$$

Where $ \quad U\sim N_n(0, \sigma^2I_n) \quad, \beta \in R^k, X \in \R^{n\times k}$

And rather than $\overline X$, we assume a $\hat\beta$ that minimizes $\lVert Y - X\hat\beta \rVert^2$. $\hat \beta$ generalizes the mean 
$ \overline X$, which is in the special case used to minimize $$\lVert (X_1, ... , X_n)^t - (1, ... , 1)^t\overline X \rVert^2$$

Effectively, the $n$-vector $Y$ takes the role of $X_1, ... , X_n$. For the case described in the lemma, rather than using a matrix of covariates $X = (x_{ij})$ we try to predict $X$ with a vector of $1$'s $(1, ... , 1)^t$, rat.

We then define $SS_{res} = \lVert Y - X\hat\beta \rVert^2$.

We can then prove the more general theorem:

> **Lemma** (generalization)
>
> $$Y - X\hat\beta \quad  and \quad X\hat\beta$$
> are independent. This means in particular that $X\hat \beta$ is independent of $SS_{res}$. Furthermore, if we state $p = \dim R$, where $R = \{ X\beta : \beta \in \R^k \}$ the colspace of $X$, then we have that
> $$ SS_{res}/\sigma^2 \sim \chi_{n-p}^2$$

**Proof**:

choose an orthonormal basis $b = \{ e_1, ... e_n \}$ for $\R^n$, such that $e_1, ... , e_p$ forms a basis for $R$. 

Let $E = [ e_1 \mid ... \mid e_n]$ be the $n\times n$ matrix with these basis vectors as its columns. $E$ is unitary because $b$ was orthonormal. 

This means that $E^tE = I_n$ and $Z = E^tU$ follows a $N_n(0, \sigma^2I_n)$-distribution, for:

$$f_Z(z) = \lvert \det E^t \rvert f_U(Ez) = f_U(z)$$

The latter equality holds because $f_U$ will only take an exponential of $(Ez)^tEz$, which equals $z^tz$ by unitarity of $E$, and $\lvert \det E^t \rvert = 1$ obviously.

Therefore $Z_1, ... Z_n$ are independent. Let $P_R$ be the orthogonal projection onto $R$. Since

$$Y = X\beta + Z_1 e_1 + ... + Z_p e_p$$

and $X\beta, e_1, ... , e_p \in R$ and $e_{p+1}, ... , e_n \in R^\bot$, we have:

$$X\hat\beta = P_RY = X\beta + Z_1e_1 + ... + Z_n e_n$$

Therefore:

$$ Y - X\hat\beta = Z_{p+1}e_{p+1} + ... + Z_n e_n$$

So that:

$$ SS_{res}^2/\sigma^2 = Z_{p+1}^2/\sigma^2 + ... + Z_n^2/\sigma^2$$

is the sum of $n-p$ i.i.d $N(0,1)$-variables, hence it follows by definition a $\chi_{n-p}^2$-distribution.

Also note that $Y - X\hat\beta$ is $Z_{p+1}e_{p+1} + ... + $Z_ne_n$ while $X\hat\beta = X\beta + Z_1e_1 + ... + Z_p e_p$, and since $Z_i$ were independently distributed this gives that $X\hat\beta$ and $Y - X\hat\beta$ are independent, so that in particular $SS_{res} $ and $X\beta$ are independent.

In our special case that $\hat\beta = \overline X$ and $SS_{res} = (n-1)S^2$, this gives the required result for the first lemma.

## Deriving the PDF of chi-squared

In the above lemma, we defined the $\chi^2_n$-distribution to be the distribution of a random variable $Z_1^2 + ... + Z_n^2$, where $Z_i \sim N(0,1)$ i.i.d. 

But this does not answer what the corresponding density function actually is. 

> **Lemma**
>
> $X \sim \chi_k^2$ iff.
> $$ f_X(x) = 0, x < 0$$
> $$ f_X(x) = \frac 1 {2^{k/2}\Gamma(\frac k 2)} x^{\frac k 2 - 1} e^{-\frac x 2}$$

**Proof**:
Obviously $Z_1^2 + ... + Z_n^2 \geq 0$ so the density below $0$ should be $0$.

We use an induction argument on $k\geq 1$, where we apply the convolution formula:

$$ f_{X+Y}(a) = \int_{-\infty}^{\infty} f_X(a)f_Y(x-a)dx$$

In our case this reduces to

$$ f_{X+Y}(a) = \int_{0}^{\infty} f_X(a)f_Y(x-a)dx$$

And looking at the induction hypothesis, we can already sense the $\Gamma$ popping up somewhere.

For $k = 1$ we have

$$f_{Z^2} (x) = \frac d {dx} \mathbb P [\lvert Z \rvert \geq x^{1/2}] $$

$$ ... = \frac d {dx} (2 \Phi(x^{1/2}))$$

by symmetry of $\Phi$ etc. etc. So we finish it with the chain rule:

$$ ... = \Phi(x^{1/2})x^{-1/2}$$

$$ ... = \frac 1 {2^{1/2}\Gamma(\frac 1 2)} x^{\frac 1 2 - 1} e^{-\frac x 2}$$

I will not show that we can indeed find a $\Gamma(1/2)$ in the multiplicative constants of the $\Phi$, but please do this yourself.

Next, the induction step: If we already know that 

$$f_X(x) = \frac 1 {2^{k/2}\Gamma(\frac k 2)} x^{\frac k 2 - 1} e^{-\frac x 2}$$

for $X = Z_1^2 + .. + Z_k^2$, let us add a $Z_{k+1}^2$ and use convolution:

$$f_{X+Z_{k+1}^2}(a) = \int_{0}^{\infty} f_X(x)f_{Z_{k+1}^2}(a-x)dx$$

$$ ... = \int_{0}^{\infty} \frac 1 {2^{k/2}\Gamma(\frac k 2)} x^{\frac k 2 - 1} e^{-\frac x 2} \cdot \frac 1 {2^{1/2}\Gamma(\frac 1 2)} (a-x)^{\frac 1 2 - 1} e^{-\frac {(a-x)} 2} dx$$

$$ ... = \frac 1 {2^{(k+1)/2}\Gamma(\frac k 2)\Gamma(\frac 12)} \cdot \int_{0}^{\infty} x^{\frac k 2 - 1} e^{-\frac x 2} (a-x)^{\frac 1 2 - 1} e^{-\frac {(a-x)} 2} dx$$

