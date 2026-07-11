---
layout: project_hub
title: Otimização Topológica com FEniCSx
description: Tutoriais numéricos de otimização topológica utilizando o framework FEniCSx para o método dos elementos finitos.
importance: 1
category: R. L. Pereira
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
  - name: Sobre este Projeto
    # if a section has subsections, you can add them as follows:
    # subsections:
  - name: O que é o FEniCSx?
  - name: O que é Otimização Topológica?
    subsections:
      - name: Métodos Abordados
  - name: Estrutura dos Tutoriais
    subsections:
      - name: Fundamentos
      - name: Otimização Topológica
      - name: Exemplos
  - name: Referências
---

## Sobre este Projeto

Este projeto reúne uma coleção de tutoriais numéricos voltados à **otimização topológica de estruturas**, implementados utilizando o framework **FEniCSx** para o método dos elementos finitos. O conteúdo é organizado de forma progressiva, partindo dos fundamentos teóricos até aplicações práticas em problemas de engenharia.

---

## O que é o FEniCSx?

O [FEniCSx](https://fenicsproject.org/) é uma plataforma de código aberto para a resolução de equações diferenciais parciais (EDPs) pelo Método dos Elementos Finitos (MEF). Ele permite que o usuário escreva a formulação variacional de um problema de forma muito próxima à notação matemática, automatizando a geração de código de elementos finitos de alto desempenho.

As principais características do FEniCSx incluem:

- **Formulação variacional em alto nível** — A linguagem UFL (_Unified Form Language_) permite expressar formas bilineares e lineares diretamente em Python, de maneira quase idêntica à notação matemática.
- **Geração automática de código** — O compilador de formas FFCx gera código C otimizado a partir das formas UFL, eliminando a necessidade de implementação manual de rotinas de montagem.
- **Paralelismo nativo** — Suporte integrado a MPI para computação em múltiplos processadores.
- **Flexibilidade de malhas** — Suporte a malhas de triângulos, tetraedros, quadriláteros e hexaedros, com capacidade de importação de malhas externas via [Gmsh](https://gmsh.info/).

---

## O que é Otimização Topológica?

A **otimização topológica** é um método computacional que determina a distribuição ótima de material dentro de um domínio de projeto, de forma a otimizar uma função objetivo (tipicamente a rigidez estrutural) sujeita a restrições (como volume máximo de material).

Diferentemente da otimização de forma, que apenas modifica os contornos de uma geometria pré-definida, a otimização topológica permite a criação e remoção de furos e conectividades, resultando em geometrias não intuitivas e altamente eficientes.

### Métodos abordados

Os tutoriais deste projeto abordam os seguintes métodos de otimização topológica:

| Método   | Descrição                                                                                                                   |
| -------- | --------------------------------------------------------------------------------------------------------------------------- |
| **SIMP** | _Solid Isotropic Material with Penalization_ — penaliza densidades intermediárias para convergir a uma solução 0/1          |
| **BESO** | _Bi-directional Evolutionary Structural Optimization_ — adiciona e remove elementos discretamente com base na sensibilidade |

---

## Estrutura dos Tutoriais

Os tutoriais estão organizados nas seguintes seções, acessíveis pela barra de navegação lateral:

### Fundamentos

Revisão das equações governantes da mecânica dos sólidos, formulação do método dos elementos finitos e introdução ao FEniCSx.

### Otimização Topológica

Teoria e implementação dos métodos SIMP e BESO, incluindo técnicas de filtragem de sensibilidade.

### Exemplos

Aplicações práticas em problemas clássicos de otimização topológica, como a viga MBB e a viga em balanço.

## Referências

---

> **Nota:** Os tutoriais marcados como _(em breve)_ na barra lateral estão em desenvolvimento e serão disponibilizados progressivamente.
