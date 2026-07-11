---
layout: project_hub
title: Equações da Elasticidade Linear
description: Dedução das equações da elasticidade linear, desde a forma forte até a forma matricial do método dos elementos finitos.
date: 2021-05-22
importance: 2
category:
sidebar_id: otimizacao_topologica_fenicsx

authors:
  - name: Rodrigo L. Pereira
    url: ""
    affiliations:
      name: IFB - Estrutural

bibliography: 2018-12-22-distill.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Forma Forte
    # if a section has subsections, you can add them as follows:
    # subsections:
  - name: Forma Fraca
    subsections:
      - name: Representação Clássica
  - name: Representação Matricial
---

## Forma Forte

Considere um corpo elástico sujeito a pequenas deformações, isotrópico e homogêneo. As equações que regem o comportamento desse corpo podem ser condensadas no Problema de Valor de Contorno (PVC) ou forma forte,

$$ \tag{1}
    \begin{cases}
        \nabla \cdot \boldsymbol{\sigma} + \mathbf{b} = \mathbf{0},
        & \text{em } \Omega,\\[2mm]
        \mathbf{u}=\bar{\mathbf{u}},
        & \text{em } \Gamma_D,\\[2mm]
        \boldsymbol{\sigma}\cdot\mathbf{n}=\mathbf{f},
        & \text{em } \Gamma_N,
    \end{cases}
$$

onde $\Omega$ representa o domínio do problema, enquanto $\Gamma_D$ e $\Gamma_N$ correspondem, respectivamente, às porções do contorno onde são impostas condições de Dirichlet (deslocamentos prescritos) e Neumann (trações superficiais prescritas). O vetor $\mathbf{u}$ representa o campo de deslocamentos, sendo $\bar{\mathbf{u}}$ o deslocamento imposto sobre $\Gamma_D$. O $\boldsymbol{\sigma}$ denota o tensor de tensões de Cauchy, $\mathbf{b}$ representa o vetor de forças de corpo (N/m$^3$), $\mathbf{n}$ é o vetor unitário normal exterior ao contorno e $\mathbf{f}$ representa o vetor de forças de superfície (N/m$^2$) aplicado sobre $\Gamma_N$. A primeira equação expressa o equilíbrio estático de forças no interior do domínio, enquanto as duas últimas definem, respectivamente, as condições essenciais e naturais do problema.

Considerando a Lei de Hooke Generalizada, pode-se ainda escrever matematicamente o tensor de tensões de Cauchy, $\boldsymbol{\sigma}$, em que $\mu$ e $\lambda$ são os coeficientes de Lamé, dependentes das propriedades elásticas do material e expressos em função do módulo de Young, $E$, e do coeficiente de Poisson, $\nu$. Já $\boldsymbol{\varepsilon}$ representa o tensor de pequenas deformações, obtido a partir da parte simétrica do gradiente dos deslocamentos, enquanto $e$ corresponde à deformação volumétrica (ou dilatação), sendo $\mathrm{tr}(\cdot)$ o operador traço. A matriz $\mathbf{I}$ denota o tensor identidade de segunda ordem. Os termos $\sigma_{ij}$ representam as componentes do tensor de tensões de Cauchy, enquanto $\varepsilon_{ii}$ representam as deformações normais e $\varepsilon_{ij}$ ($i\neq j$) as deformações de cisalhamento. O operador $\nabla$ corresponde ao operador gradiente, $\nabla\mathbf{u}$ representa o gradiente do campo de deslocamentos e o sobrescrito $(\cdot)^S$ indica a parte simétrica de um tensor de segunda ordem.

$$ \tag{2}
    \boldsymbol{\sigma} = 2\mu\boldsymbol{\varepsilon} + \lambda e\,\mathbf{I} =
    \begin{bmatrix}
        \sigma_{xx} & \sigma_{xy} & \sigma_{xz}\\
        \sigma_{xy} & \sigma_{yy} & \sigma_{yz}\\
        \sigma_{xz} & \sigma_{yz} & \sigma_{zz}
    \end{bmatrix},
$$

$$ \tag{3}
    \mu=\frac{E}{2(1+\nu)}, \quad \lambda = \frac{E\nu}{(1+\nu)(1-2\nu)},
$$

$$ \tag{4}
    \boldsymbol{\varepsilon} = \frac12\left(\nabla\mathbf{u} + \nabla\mathbf{u}^{T}\right) = \left(\nabla\mathbf{u}\right)^S = \begin{bmatrix}
        \varepsilon_{xx} & \varepsilon_{xy} & \varepsilon_{xz}\\
        \varepsilon_{xy} & \varepsilon_{yy} & \varepsilon_{yz}\\
        \varepsilon_{xz} & \varepsilon_{yz} & \varepsilon_{zz}
    \end{bmatrix},
$$

$$ \tag{5}
    e = \varepsilon_{xx} + \varepsilon_{yy} + \varepsilon_{zz} = \operatorname{tr}(\boldsymbol{\varepsilon}) = \nabla\cdot\mathbf{u} = \frac{\partial u_x}{\partial x} + \frac{\partial u_y}{\partial y} + \frac{\partial u_z}{\partial z},
$$

$$ \tag{6}
    \mathbf{b} =
        \begin{Bmatrix}
        b_x\\
        b_y\\
        b_z
        \end{Bmatrix}, \quad
    \mathbf{u} =
        \begin{Bmatrix}
        u_x\\
        u_y\\
        u_z
        \end{Bmatrix}, \quad
    \nabla\mathbf{u} =
        \begin{bmatrix}
        u_{x,x} & u_{x,y} & u_{x,z}\\
        u_{y,x} & u_{y,y} & u_{y,z}\\
        u_{z,x} & u_{z,y} & u_{z,z}
        \end{bmatrix}, \quad
    \mathbf{I} =
        \begin{bmatrix}
        1 & 0 & 0\\
        0 & 1 & 0\\
        0 & 0 & 1
    \end{bmatrix}.
$$

## Forma Fraca

Considerando ainda uma função peso $\mathbf{v}$, podemos rescrever o primeiro termo do PVC como,

$$ \tag{7}
    \int_{\Omega}(\nabla\cdot\boldsymbol{\sigma}) \cdot \mathbf{v}\,d\Omega + \int_{\Omega}\mathbf{b}\cdot\mathbf{v}\,d\Omega = 0.
$$

Aplicando o **Teorema da divergência** ao primeiro termo,

$$ \tag{8}
    \int_{\Omega}(\nabla\cdot\boldsymbol{\sigma}) \cdot \mathbf{v}\,d\Omega = -\int_{\Omega}\boldsymbol{\sigma}:\nabla\mathbf{v}\,d\Omega + \int_{\Gamma_N}(\boldsymbol{\sigma}\cdot\mathbf{n}) \cdot \mathbf{v}\,d\Gamma,
$$

a forma fraca ou variacional pode então ser definida,

$$ \tag{9}
    \boxed{\int_{\Omega}\boldsymbol{\sigma}:\nabla\mathbf{v}\,d\Omega=\int_{\Omega}\mathbf{b}\cdot\mathbf{v}\,d\Omega + \int_{\Gamma_N}(\boldsymbol{\sigma}\cdot\mathbf{n})\cdot\mathbf{v}\,d\Gamma.}
$$

A expressão acima é geralmente separada em duas partes: o lado antes da igualdade, tratado como _left-hand side_ (lhs) ou forma bilinear, e o lado após da igualdade, sendo o _right-hand side_ (rhs) ou forma linear.

Outra forma de representar a expressão acima é expandindo o tensor $\nabla\mathbf{v}$ em sua parte simétrica e anti-simétrica, uma vez que todo tensor de segunda ordem pode ser assim decomposto. Além disso, pode ser mostrado que a multiplicação de um tensor simétrico com um anti-simétrico resulta em zero. Com isso em mente, vem,

$$ \tag{10}
    \nabla\mathbf{v}=\left(\nabla\mathbf{v}\right)^S+\left(\nabla\mathbf{v}\right)^A,
$$

em que,

$$ \tag{11}
    (\nabla\mathbf{v})^S=\frac{1}{2}\left(\nabla\mathbf{v}+\nabla\mathbf{v}^{T}\right) \ \ \ \text{e} \ \ \ (\nabla\mathbf{v})^A=\frac{1}{2}\left(\nabla\mathbf{v}-\nabla\mathbf{v}^{T}\right).
$$

Como o tensor de tensões de Cauchy é simétrico,

$$ \tag{12}
    \boldsymbol{\sigma}:\left(\nabla\mathbf{v}\right)^A=0,
$$

de modo que,

$$ \tag{13}
    \boldsymbol{\sigma}:\nabla\mathbf{v}=\boldsymbol{\sigma}:(\nabla\mathbf{v})^S=\boldsymbol{\sigma}:\boldsymbol{\varepsilon}(\mathbf{v}).
$$

Assim, o problema da elasticidade linear pode então ser enunciado como,

$$
    \text{Encontrar } \mathbf{u}\in U \text{ tal que }
$$

$$ \tag{14}
    \int_{\Omega}\boldsymbol{\sigma}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})\,d\Omega=\int_{\Omega}\mathbf{b}\cdot\mathbf{v}\,d\Omega+\int_{\Gamma_N}\mathbf{f}\cdot\mathbf{v}\,d\Gamma, \quad \forall \mathbf{v} \in U_0,
$$

$$
    \text{onde} \ U=\left\{\mathbf{u}\in H^1 \mid \mathbf{u}=\bar{\mathbf{u}} \text{ em } \Gamma_D\right\}
    \ \text{e} \ U_0=\left\{\mathbf{v}\in H^1 \mid \mathbf{v}=\mathbf{0} \text{ em } \Gamma_D\right\}.
$$

Na expressão acima $H^1$ é um espaço de Sobolev.

### Representação Clássica

Substituindo ainda a Lei de Hooke Generalizada,

$$ \tag{15}
    \boldsymbol{\sigma}(\mathbf{u})=2\mu\,\boldsymbol{\varepsilon}(\mathbf{u})+\lambda e \mathbf{I},
$$

obtém-se,

$$ \tag{16}
    \int_{\Omega}\left[2\mu\,\boldsymbol{\varepsilon}(\mathbf{u})+\lambda e \mathbf{I}\right]:\boldsymbol{\varepsilon}(\mathbf{v})\,\mathrm{d}\Omega=\int_{\Omega}\mathbf{b}\cdot\mathbf{v}\,\mathrm{d}\Omega+\int_{\Gamma_N}\mathbf{f}\cdot\mathbf{v}\,\mathrm{d}\Gamma.
$$

Utilizando as identidades

$$ \tag{17}
    \mathbf{I}:\boldsymbol{\varepsilon}(\mathbf{v})=\operatorname{tr}\!\left(\boldsymbol{\varepsilon}(\mathbf{v})\right)=\nabla\cdot\mathbf{v} \ \ \text{e} \ \ \nabla\cdot\mathbf{u}=\operatorname{tr}\!\left(\boldsymbol{\varepsilon}(\mathbf{u})\right) = e,
$$

obtém-se a forma variacional clássica da elasticidade linear,

$$ \tag{18}
    \boxed{\int_{\Omega}\left[2\mu\,\boldsymbol{\varepsilon}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})+\lambda(\nabla\cdot\mathbf{u})(\nabla\cdot\mathbf{v})\right]\,\mathrm{d}\Omega=\int_{\Omega}\mathbf{b}\cdot\mathbf{v}\,\mathrm{d}\Omega+\int_{\Gamma_N}\mathbf{f}\cdot\mathbf{v}\,\mathrm{d}\Gamma.}
$$

## Representação Matricial

A relação constitutiva e a relação deformação-deslocamento pode ser escritas, respectivamente, como,

$$ \tag{19}
    \boldsymbol{\sigma}=\mathbf{D}\boldsymbol{\varepsilon} \quad \text{e} \quad \boldsymbol{\varepsilon}=\mathbf{L}\mathbf{u}.
$$

Adotando a aproximação de Galerkin,

$$ \tag{20}
    \mathbf{u}=\mathbf{N}\mathbf{u}_e \quad \text{e} \quad \mathbf{v}=\mathbf{N}\mathbf{v}_e,
$$

Segue que,

$$ \tag{21}
    \boldsymbol{\varepsilon}=\mathbf{B}\mathbf{u}_e \quad \text{com} \quad \mathbf{B}=\mathbf{L}\mathbf{N}.
$$

Vale salientar que , $\mathbf{D}$ representa a matriz constitutiva do material, responsável por relacionar as tensões às deformações segundo a Lei de Hooke Generalizada. A matriz $\mathbf{L}$ corresponde ao operador diferencial deformação-deslocamento, responsável por transformar o campo de deslocamentos em deformações. Esse operador contém derivadas espaciais e sua forma depende da dimensionalidade do problema (2D ou 3D). A matriz de funções de forma (_shape functions_) é denotada por $\mathbf{N}$, sendo utilizada para interpolar os deslocamentos no interior do elemento finito a partir dos deslocamentos nodais. O vetor $\mathbf{u}_e$ representa o vetor de deslocamentos nodais do elemento, contendo todos os graus de liberdade associados aos nós do e-ésimo elemento finito. De maneira análoga, $\mathbf{v}_e$ representa o vetor de deslocamentos virtuais nodais, empregado na formulação variacional pelo método de Galerkin. A matriz $\mathbf{B}$, denominada matriz deformação-deslocamento, é obtida pela aplicação do operador diferencial às funções de forma, ou seja, $\mathbf{B} = \mathbf{L}\mathbf{N}$.

Substituindo essas aproximações na forma variacional, Eq. (14), e considerando que cada um desses termos é um escalar ($\alpha = \alpha^T \,\, \forall\alpha\,\, \text{escalar}$), escreve-se,

$$ \tag{22}
    \int_{\Omega_e}\left[\left(\mathbf{D}\mathbf{L}\mathbf{N}\mathbf{u}_e\right)^T \left(\mathbf{L}\mathbf{N}\mathbf{v}_e\right)\right]^T\mathrm{d}\Omega_e = \int_{\Omega_e}\left(\mathbf{b}^T\mathbf{N}\mathbf{v}_e\right)^T \,\mathrm{d}\Omega_e + \int_{\Gamma_{N_e}}(\mathbf{f}^T \mathbf{N}\mathbf{v}_e)^T\,\mathrm{d}\Gamma_{N_e},
$$

ou seja,

$$ \tag{23}
    \int_{\Omega_e}\mathbf{v}_e^T\left(\mathbf{N}^T\mathbf{L}^T \mathbf{D}\mathbf{L}\mathbf{N}\mathbf{u}_e\right)\,\mathrm{d}\Omega_e = \int_{\Omega_e}\mathbf{v}_e^T\left(\mathbf{N}^T\mathbf{b}\right) \,\mathrm{d}\Omega_e + \int_{\Gamma_{N_e}}\mathbf{v}_e^T\left(\mathbf{N}^T\mathbf{f}\right)\,\mathrm{d}\Gamma_{N_e},
$$

$$ \tag{24}
    \mathbf{v}_e^{T}\left(\int_{\Omega_e}\mathbf{B}^{T}\mathbf{D}\mathbf{B}\,\mathrm{d}\Omega_e\right)\mathbf{u}_e=\mathbf{v}_e^{T}\left(\int_{\Omega_e}\mathbf{N}^{T}\mathbf{b}\,\mathrm{d}\Omega_e+\int_{\Gamma_e}\mathbf{N}^{T}\mathbf{f}\,\mathrm{d}\Gamma_{N_e}\right).
$$

Como os deslocamentos virtuais nodais $\mathbf{v}_e$ são arbitrários, conclui-se que,

$$ \tag{25}
    \boxed{\left(\int_{\Omega_e}\mathbf{B}^{T}\mathbf{D}\mathbf{B}\,\mathrm{d}\Omega_e\right)\mathbf{u}_e=\int_{\Omega_e}\mathbf{N}^{T}\mathbf{b}\,\mathrm{d}\Omega_e + \int_{\Gamma_e}\mathbf{N}^{T}\mathbf{f}\,\mathrm{d}\Gamma_{N_e}.}
$$

Define-se, então, a matriz de rigidez elementar como,

$$ \tag{26}
    \mathbf{K}_e=\int_{\Omega_e}\mathbf{B}^{T}\mathbf{D}\mathbf{B}\,\mathrm{d}\Omega_e,
$$

o vetor de forças de corpo,

$$ \tag{27}
    \mathbf{f}_{b,e}=\int_{\Omega_e}\mathbf{N}^{T}\mathbf{b}\,\mathrm{d}\Omega_e,
$$

e o vetor de forças superficiais

$$ \tag{28}
    \mathbf{f}_{t,e}=\int_{\Gamma_e}\mathbf{N}^{T}\mathbf{f}\,\mathrm{d}\Gamma_{N_e}.
$$

Consequentemente, a equação matricial elementar pode ser escrita como

$$ \tag{29}
    \boxed{\mathbf{K}_e\mathbf{u}_e=\mathbf{f}_{b,e}+\mathbf{f}_{t,e}.}
$$

Finalmente, após a montagem das contribuições de todos os elementos da malha, obtém-se o sistema global de equações,

$$ \tag{30}
    \boxed{\mathbf{K}\mathbf{u}=\mathbf{f}_{b}+\mathbf{f}_{t}.}
$$
