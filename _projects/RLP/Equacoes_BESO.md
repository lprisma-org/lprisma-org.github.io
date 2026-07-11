---
layout: project_hub
title: Método BESO
description: Bi-directional Evolutionary Structural Optimization (BESO) Algorithm
date: 2025-01-01
importance: 4
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

## Minimização da _Compliance_ Estrutural

Um dos problemas mais comuns da otimização topológica estrutural consiste na minimização da _compliance_ média. Assim, nesta seção é considerada a metodologia _Bi-directional Evolutionary Structural Optimization Method_ (BESO) para a solução desse problema, cuja formulação é dada por (Huang; Xie, 2010a):

$$ \tag{1}
    \begin{aligned}
        \text{Minimizar:}\qquad & C=\frac{1}{2}\mathbf{f}^{T}\mathbf{u},\\[1mm]
        \text{sujeito a:}\qquad &
        \mathbf{K}\mathbf{u}=\mathbf{f},\\
        &
        V^{*}-\sum_{i=1}^{N_{\mathrm{elD}}}V_i x_i=0,\\
        &
        x_i=x_{\min}\ \text{ou}\ 1.
    \end{aligned}
$$

em que $C$ representa a _compliance_ da estrutura, $V^*$ corresponde à fração volumétrica final prescrita e $\sum_{i=1}^{N_{elD}}V_i x_i$ representa o volume ocupado pela estrutura em uma determinada iteração do processo de otimização. O termo $N_{elD}$ denota o número total de elementos pertencentes ao domínio de projeto. A equação $\mathbf{K}\mathbf{u}=\mathbf{f}$ representa o sistema linear da elasticidade estática, em que $\mathbf{K}$ é a matriz global de rigidez da estrutura, $\mathbf{u}$ é o vetor global de deslocamentos e $\mathbf{f}$ é o vetor global de carregamentos. A restrição de igualdade imposta à fração volumétrica é uma característica comum dos algoritmos evolucionários, uma vez que a geometria da estrutura é modificada continuamente ao longo do processo de otimização. A equação de equilíbrio pode ainda representar problemas multifísicos, permitindo considerar fenômenos acústicos, térmicos, elétricos ou outras formulações acopladas.

A variável de projeto (pseudo-densidade) é representada por $x_i$, assumindo apenas valores discretos compreendidos entre $x_\text{min}$ e 1. Vale salientar ainda que inicialmente adotava-se $x_\text{min}=0$, implicando na remoção completa dos elementos classificados como vazios do domínio de projeto. Embora essa estratégia reduza o custo computacional, uma vez que elementos e nós eliminados deixam de participar das iterações subsequentes, os chamados métodos **hard-kill** frequentemente conduzem a soluções subótimas ou apresentam dificuldades de convergência (Zhou; Rozvany, 2001; Huang; Xie, 2010a; Huang; Xie, 2010c; Sigmund; Maute, 2013).

Posteriormente, Rozvany e Querin (2002) propuseram o método _Sequential Element Rejection and Admission_ (SERA), no qual os elementos removidos são substituídos por um material de densidade muito baixa, introduzindo o conceito conhecido como **soft-kill**, isto é, $0<x_{\min}\ll1$. Mais tarde, Huang e Xie (2009) combinaram essa estratégia **soft-kill** com uma lei de potência semelhante à utilizada no método SIMP, permitindo calcular diretamente os gradientes da função objetivo em vez de utilizar aproximações baseadas em diferenças de sensibilidade. Essa formulação constitui a base do algoritmo BESO moderno e será apresentada na seção seguinte.

## Análise de Sensibilidade

Com o objetivo de obter uma configuração final composta apenas por regiões sólidas e vazias, adota-se um esquema de interpolação do módulo de elasticidade, dado por

$$ \tag{2}
    E(x_i)=E_0 x_i^{p},
$$

em que $E$ representa o módulo de Young efetivo do elemento, $E_0$ corresponde ao módulo de Young do material sólido e $p$ é o expoente de penalização empregado pelo método BESO. A partir dessa interpolação, a matriz global de rigidez pode ser escrita como,

$$ \tag{3}
    \mathbf{K}=\sum_{i=1}^{N_{\mathrm{elD}}}x_i^{p}\mathbf{K}_{i}^0,
$$

onde $\mathbf{K}_{i}^0$ representa a matriz de rigidez do i-ésimo elemento calculada considerando o material sólido.

Quando um elemento é adicionado ou removido do domínio de projeto, a função objetivo sofre uma alteração em decorrência da modificação da rigidez estrutural. Essa variação é denominada **número de sensibilidade**, representado por $\alpha_i$, e é associado ao elemento modificado. No método BESO com estratégia _soft-kill_, os números de sensibilidade são obtidos a partir do gradiente da função objetivo, resultando em,

$$ \tag{4}
    \frac{dC}{dx_i}=\frac{1}{2}\frac{d}{dx_i}\left(\mathbf{f}^T\mathbf{u}\right)=\frac{1}{2}\left(\frac{\partial\mathbf{f}^T}{\partial x_i}\mathbf{u}+\mathbf{f}^T\frac{\partial\mathbf{u}}{\partial x_i}\right).
$$

Como a derivada $\partial\mathbf{u}/\partial x_i$ é desconhecida, emprega-se o método adjunto (Tortorelli; Michaleris, 1994), introduzindo, na função objetivo, um vetor de multiplicadores de Lagrange, $\boldsymbol{\lambda}$, e o termo extra que não provoca alteração, $\left(\mathbf{f}-\mathbf{K}\mathbf{u}\right)$, vem,

$$ \tag{5}
    C=\frac{1}{2}\mathbf{f}^T\mathbf{u}+\boldsymbol{\lambda}^{T}\left(\mathbf{f}-\mathbf{K}\mathbf{u}\right).
$$

Assumindo que o vetor global de carregamentos não depende das variáveis de projeto, isto é,

$$ \tag{6}
    \frac{\partial\mathbf{f}^T}{\partial x_i}=0,
$$

obtém-se,

$$ \tag{7}
    \frac{dC}{dx_i}=\left(\frac{1}{2}\mathbf{f}^T-\boldsymbol{\lambda}^T\mathbf{K}\right)\frac{\partial\mathbf{u}}{\partial x_i}-\boldsymbol{\lambda}^T\frac{\partial\mathbf{K}}{\partial x_i}\mathbf{u}.
$$

Para eliminar a derivada desconhecida do campo de deslocamentos, escolhe-se o multiplicador adjunto de forma que,

$$ \tag{8}
    \frac{1}{2}\mathbf{f}^T-\boldsymbol{\lambda}^{T}\mathbf{K}=0.
$$

Utilizando a equação de equilíbrio mecânico e sabendo que a matriz de rigidez é simétrica ($\mathbf{K}=\mathbf{K}^{T}$),

$$ \tag{9}
    \mathbf{f}^T=\mathbf{u}^T\mathbf{K}^{T}=\mathbf{u}^T\mathbf{K}.
$$

Logo,

$$ \tag{10}
    \boxed{\boldsymbol{\lambda}=\frac{1}{2}\mathbf{u},}
$$

e, consequentemente,

$$ \tag{11}
    \frac{dC}{dx_i}=-\frac{1}{2}\mathbf{u}^T\frac{\partial\mathbf{K}}{\partial x_i}\mathbf{u}.
$$

Nesse sentido, pode-se ainda substitutir a Eq. (3) na Eq. (11), obtendo para o i-ésimo elemento,

$$ \tag{12}
    \boxed{\alpha_i = \frac{dC}{dx_i} = -\frac{1}{2}\,p\,x_i^{p-1}\mathbf{u}_i^T\mathbf{K}_i^0\mathbf{u}_i.}
$$

### Número de Sensibilidade: Interpretação Física

Considerando então a Eq. 12, e o fato de que,

$$ \tag{13}
    \mathbf{K}^0_i=\int_{\Omega_i}\mathbf{B}^{T}\mathbf{D}_0\mathbf{B}\,\mathrm{d}\Omega_i,
$$

segue que,

$$ \tag{14}
    \alpha_i=-\frac{1}{2}p\,x_i^{\,p-1}\int_{\Omega_i}\mathbf{u}_i^{T}\mathbf{B}^{T}\mathbf{D}_0\mathbf{B}\mathbf{u}_i\,\mathrm{d}\Omega_i.
$$

Observando que,

$$ \tag{15}
    \mathbf{B}\mathbf{u}_i=\boldsymbol{\varepsilon}(\mathbf{u}_i),
$$

e que,

$$ \tag{16}
    \mathbf{D}_0\boldsymbol{\varepsilon}(\mathbf{u}_i)=\boldsymbol{\sigma}_0(\mathbf{u}_i),
$$

além da simetria da matriz constitutiva ($\mathbf{D}_0 = \mathbf{D}_0^T$), vem,

$$ \tag{17}
    \alpha_i=-\frac{1}{2}p\,x_i^{\,p-1}\int_{\Omega_i}\boldsymbol{\sigma}_0(\mathbf{u}_i)^T\boldsymbol{\varepsilon}(\mathbf{u}_i)\,\mathrm{d}\Omega_i = -p\,x_i^{\,p-1}\int_{\Omega_i}\frac{1}{2}\,\boldsymbol{\sigma}_0(\mathbf{u}_i):\boldsymbol{\varepsilon}(\mathbf{u}_i)\,\mathrm{d}\Omega_i.
$$

Considerando ainda a forma fraca da elasticidade linear e a ausencia de forças de corpo, pode-se em fim escrever,

$$ \tag{18}
    \boxed{\alpha_i=-p\,x_i^{\,p-1}\int_{\Gamma_N}\frac{1}{2}\mathbf{f}_i^T\mathbf{u}_i = -p\,x_i^{\,p-1}\, C_i.}
$$

## Filtro de Sensibilidades

Como as sensibilidades calculadas a nível elementar não possuem significado físico direto, torna-se necessário realizar um processo de suavização espacial, de modo a reduzir a dependência de malha e evitar o aparecimento de instabilidades numéricas, como padrões do tipo tabuleiro de xadrez (_checkerboard_). Para isso, define-se um raio de filtragem, que independe da malha, denotado por $r_{\min}$, centrado no centróide de cada elemento. Todos os nós localizados no interior dessa vizinhança passam a contribuir para o cálculo da sensibilidade nodal associada ao elemento considerado. Assim, o procedimento de filtragem dos números de sensibilidade inicia-se com a distribuição desses valores nos nós da malha.

Considerando $\alpha_{n_d}$ como a sensibilidade associada ao nó $n_d$, $M$ o número de elementos conectados a esse nó, $w_i$ o fator de ponderação correspondente ao elemento $i$, e $r_{in}$ a distância entre o centróide do elemento $i$ e o nó $n_d$, a sensibilidade nodal pode ser calculada por,

$$ \tag{19}
    \alpha_{n_d}=\sum_{i=1}^{M}w_i\alpha_i.
$$

Já o fator de ponderação é definido como,

$$ \tag{20}
    w_i=\begin{cases}\quad\, 1,&M=1,\\[1mm]\dfrac{1}{M-1}\left(1-\dfrac{r_{in}}{\sum_{i=1}^{M}r_{in}}\right),&M>1.\end{cases}
$$

A Equação anterior indica que os nós mais próximos ao centróide do elemento exercem maior influência sobre a sensibilidade nodal do que aqueles localizados mais distantes. Após a obtenção das sensibilidades nodais, calcula-se a sensibilidade suavizada de cada elemento por meio da média ponderada das sensibilidades dos nós pertencentes à sua vizinhança,

$$ \tag{21}
    \alpha_i=\frac{\sum_{I}w(r_{in})\alpha_{n_d}}{\sum_{I}w(r_{in})}.
$$

Nessa expressão, $I$ representa o conjunto de nós localizados no interior da vizinhança de raio $r_{\min}$, enquanto a função peso é definida por,

$$ \tag{22}
    w(r_{in})=r_{\min}-r_{in}.
$$

Observa-se que essa função de ponderação é linear, atribuindo maior influência aos nós mais próximos do centróide do elemento e reduzindo gradualmente sua contribuição à medida que a distância aumenta. Como consequência, obtém-se um campo de sensibilidades suavizado e praticamente independente da discretização empregada, reduzindo significativamente a ocorrência de instabilidades numéricas e favorecendo a convergência do algoritmo de otimização.

## Estabilização e Normalização das Sensibilidades

Com o objetivo de aumentar a estabilidade do processo de otimização, uma estratégia amplamente empregada consiste em realizar uma média histórica dos números de sensibilidade entre iterações consecutivas. Essa técnica reduz oscilações excessivas na distribuição das sensibilidades, contribuindo para uma evolução mais estável da topologia. Assim, a sensibilidade estabilizada pode ser calculada por,

$$ \tag{23}
    \alpha_i^{(r)}=\frac{\alpha_i^{(r-1)}+\alpha_i^{(r)}}{2}.
$$

Nessa expressão, o sobrescrito $(\cdot)^{(r)}$ indica a iteração atual do algoritmo de otimização topológica, enquanto $(\cdot)^{(r-1)}$ representa a iteração imediatamente anterior.

Outra estratégia de estabilização foi proposta por Zhou et al. (2021), baseada na aplicação da técnica de escalonamento **Min-Max** para normalização dos números de sensibilidade. Em diversas situações, recomenda-se aplicar essa etapa adicional, uma vez que as sensibilidades podem assumir simultaneamente valores positivos e negativos, dificultando sua comparação direta durante o processo evolucionário. A normalização é realizada por meio da expressão,

$$ \tag{24}
    \alpha_i^{(r)}=\frac{\alpha_i^{(r)}-\alpha_{\min}^{(r)}}{\alpha_{\max}^{(r)}-\alpha_{\min}^{(r)}}.
$$

Nessa equação, $\alpha_{\max}^{(r)}$ e $\alpha_{\min}^{(r)}$ representam, respectivamente, os maiores e os menores valores de sensibilidade obtidos na iteração atual. Como consequência, todas as sensibilidades passam a pertencer ao intervalo,

$$ \tag{25}
    0\leq\alpha_i^{(r)}\leq1,
$$

preservando a ordenação relativa entre os elementos e tornando o algoritmo menos sensível às variações de magnitude das sensibilidades ao longo das iterações. Essa normalização favorece a estabilidade numérica do processo de otimização, especialmente em problemas envolvendo múltiplos materiais, diferentes escalas físicas ou grandes contrastes de rigidez.

## Atualização Heurística da Topologia e Critério de Parada

Para atualizar as variáveis de projeto é necessário, inicialmente, definir o volume alvo da próxima iteração. Considerando a Taxa Evolucionária, (_Evolutionary Rate or ER_), como a variação da fração volumétrica entre iterações consecutivas, a relação entre a fração volumétrica da iteração atual, $V_r$, e da iteração seguinte, $V_{r+1}$, pode ser escrita como,

$$ \tag{26}
    V_{r+1}=V_r(1\pm\mathrm{ER}).
$$

Nos métodos de otimização evolucionária, os números de sensibilidade constituem índices locais que podem ser ordenados em ordem decrescente, permitindo que o material seja redistribuído de acordo com sua importância estrutural. Dessa forma, a definição da fração volumétrica alvo $V_{r+1}$ estabelece um valor limite (ou limiar) sobre o vetor ordenado de sensibilidades, determinando quantos elementos permanecerão sólidos ($x_i=1$) e quantos serão considerados vazios ($x_i=x_{\min}$). Assim,

$$ \tag{27}
    \alpha_i<\alpha_{\mathrm{th}},\qquad x_i=x_{\min},
$$

$$ \tag{28}
    \alpha_i>\alpha_{\mathrm{th}},\qquad x_i=1,
$$

em que $\alpha_{\mathrm{th}}$ representa o limiar de sensibilidade utilizado para distinguir elementos sólidos e vazios.

Como o método BESO é um procedimento bidirecional, não apenas a remoção de material é permitida, mas também sua reinserção. Para controlar esse processo é introduzida a Razão de Admissão (__Addition Ratio or AR_), responsável por limitar a quantidade de elementos vazios que podem voltar a se tornar sólidos, ou vice-versa, durante uma mesma iteração. Entretanto, para evitar oscilações excessivas na evolução da topologia, estabelece-se um valor máximo admissível para essa quantidade, denominado $\mathrm{AR}_{\max}$. Caso a razão de admissão calculada seja superior ao limite especificado, isto é,

$$ \tag{29}
    \mathrm{AR}>\mathrm{AR}_{\max}, \quad \text{então,} \quad  \mathrm{AR}=\mathrm{AR}_{\max}.
$$

De maneira geral, essas restrições fazem com que apenas uma parcela dos elementos com menores sensibilidades seja removida, enquanto somente os elementos com maiores sensibilidades sejam readmitidos no domínio estrutural, evitando alterações abruptas da geometria e favorecendo a estabilidade do processo iterativo (Picelli \emph{et al.}, 2015).

Por fim, define-se o critério de parada do algoritmo por meio da variação relativa da _compliance_ média ao longo das últimas $N$ iterações,

$$ \tag{30}
    err=\left|\frac{\sum_{m=1}^{N}C_{r-m+1}-\sum_{m=1}^{N}C_{r-N-m+1}}{\sum_{m=1}^{N}C_{r-m+1}}\right|\leq\tau.
$$

Nessa expressão, $\tau$ representa a tolerância adotada para a convergência do algoritmo. É importante destacar que, uma vez atingida a fração volumétrica final prescrita, $V_f$, o volume da estrutura permanece constante durante as iterações subsequentes. A partir desse momento, apenas a redistribuição do material é realizada, até que o critério de convergência seja satisfeito pela Equ. (30).

## Algoritmo BESO

De forma geral, o algoritmo é composto pelas seguintes etapas:

- Discretizar o domínio de projeto utilizando o Método dos Elementos Finitos, atribuir as variáveis de projeto aos elementos da malha inicial e definir os parâmetros de otimização de acordo com o problema de otimização considerado;

- Resolver o problema de elementos finitos, obtendo os deslocamentos nodais e os demais campos necessários à avaliação da função objetivo;

- Realizar a análise de sensibilidade da função objetivo em relação às variáveis de projeto;

- Aplicar o filtro de malha aos números de sensibilidade, bem como aplicar o procedimento de estabilização por meio da média histórica entre iterações e realizar a normalização utilizando o escalonamento Min-Max;

- Determinar a fração volumétrica alvo para a próxima iteração utilizando a taxa evolucionária (ER);

- Atualizar a topologia, removendo e/ou readmitindo elementos segundo o procedimento de atualização heurística, respeitando simultaneamente a restrição de volume e o limite máximo da razão de admissão (AR);

- Repetir as etapas de 2 a 6 até que a fração volumétrica final seja atingida e o critério de convergência definido pela Equ. (30) seja satisfeito.
