#### **1. Definition:**

* If $A$ is an $n\times n$ matrix, do nonzero vectors **x** in $R^n$ exist such that $A$**x** is a scalar multiple of **x**? The scalar, denoted by $\lambda$, is called an **eigenvalue** of matrix $A$, and nonzero vector **x** is called an **eigenvector** of $A$ corresponding to $\lambda$. So we have: $A$**x**$=$$\lambda$**x**.

  >Let $A$ be an $n\times n$ matrix. The scalar $\lambda$ is called an **eigenvalue** of $A$ if there is a nonzero vector **x** such that $A$**x**$=$$\lambda$**x**.
  >
  >The vector **x** is called an **eigenvector** of $A$ corresponding to $\lambda$.

  * **Note:** **x** and $\lambda$ cannot be zero.

* **Eigenspaces:**

  * > If $A$ is an $n\times n$ matrix with an eigenvalue $\lambda$, then the set of all eigenvectors of $\lambda$, together with the zero vector
    >
    > ​		{**0**} $$\cup$$ {**x**: **x** is an eigenvector of $\lambda$}
    >
    > is a subspace of $R^n$. This subspace is called the **eigenspace** of $\lambda$.



#### **2. Finding eigenvectors and eigenvalues:**

* First, we writing equation $A$**x**$=$$\lambda$**x** in the form $A$**x**$=$$\lambda I$**x** with $I$ is the $n\times n$ identity matrix. Then we produce 

  ​		($\lambda I-A$)**x** $=$ **0**. $\tag1$

* **Note:** We can see that: $(1)$ has nonzero solution if and only if the coefficient matrix ($\lambda I-A$) is *not* invertible, it means determinant of ($\lambda I-A$) is zero. So we have next theorem.

* >Let $A$ be an $n\times n$ matrix.
  >
  >1. An eigenvalue of $A$ is a scalar $\lambda$ such that
  >
  >   ​	det($\lambda I-A$) $=0$.
  >
  >2. The eigenvectors of $A$ is corresponding to $\lambda$ are the nonzero solutions of
  >
  >   ​	($\lambda I-A$)**x**$=$**0​**.

  * The equation det($\lambda I-A$) $=0$ is called the **characteristic equation** if $A$. Moreover, when expanded to polynomial form, the polynomial

    ​		$|\lambda I-A|=\lambda^n+c_{n-1}\lambda^{n-1}+...+c_1\lambda+c_0$

    is called the **characteristic polynomial** of $A$.

* **Finding eigenvalues and eigenvectors:**

  * > Let A be an $n\times n$ matrix.
    >
    > 1. Form the characteristic equation $|\lambda I-A|=0$. It will be polynomial equation of degree $n$ in the variable $\lambda$.
    > 2. Find the real roots of the characteristic equation. These are the eigenvalues of $A$.
    > 3. For each value $\lambda_i$, find the eigenvectors corresponding to $\lambda_i$, by solving ($\lambda I-A$)**x**$=$**0**.

* *Example:* Finding the eigenvalues and corresponding eigenvectors of $A=\begin{bmatrix}2&1&0\\0&2&0\\0&0&2\end{bmatrix}$. What is the dimension of the eigenspace of each eigenvalue?

  * The characteristic polynomial is
    $$
    |\lambda I-A|= \begin{vmatrix}\lambda-2&-1&0\\0&\lambda-2&0\\0&0&\lambda-2\end{vmatrix}=(\lambda-2)^3
    $$

  * So, characteristic equation is $(\lambda-2)^3=0$. So we have only eigenvalue is $\lambda=2$.

  * To find eigenvectors corresponding of $\lambda=2$, we find **x** such that $(2I-A)$**x**$=$**0**$\tag2$.
    $$
    2I-A=\begin{bmatrix}0&-1&0\\0&0&0\\0&0&0\end{bmatrix}
    $$

    * Let **x** $=\begin{bmatrix}x_1\\x_2\\x_3\end{bmatrix}$, $(2I-A)$**x**$=\begin{bmatrix}-x_2\\0\\0\end{bmatrix}$. Equation $(2)$ satisfied when $x_2=0$. Therefore, **x** $=\begin{bmatrix}x_1\\0\\x_3\end{bmatrix}\begin{bmatrix}s\\0\\t\end{bmatrix}=s\begin{bmatrix}1\\0\\x\end{bmatrix}+t\begin{bmatrix}0\\0\\1\end{bmatrix}$, $s$ and $t$ not both zero.

  * Because $\lambda=2$ has two linearly independent eigenvectors, the dimension of degree of its eigenspace is $2$.

* **Eigenvalues of triangular matrix:**

  * > If $A$ is an $n\times n$ triangular matrix, then its eigenvalues are the entries on its main diagonal.



#### Reference

1. <a href='https://drive.google.com/drive/u/3/my-drive'>Elementary Linear Algebra</a>.

