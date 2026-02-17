---
title: "Espaços Vetoriais"
date: 2026-02-17
---

Ao iniciar os estudos em Álgebra Linear, o primeiro conceito apresentado é o de espaço vetorial.

Segundo o matemático brasileiro **Elon Lages Lima**, um espaço vetorial é:

> "... o terreno onde se desenvolve toda a Álgebra Linear ..."

Um programador poderia chamá-lo de sistema.  
Um filósofo talvez o chamasse de um mundo restrito.  
O matemático o chama de estrutura algébrica.

Seguindo a analogia filosófica, trata-se de um mundo particular cujos objetos são chamados vetores e onde apenas duas operações são permitidas: adição e multiplicação por escalar.

Formalmente, dado um conjunto $V$ e um corpo $F$ (como ℝ ou ℂ), se as operações definidas em $V$ satisfazem propriedades como:

- comutatividade da soma
- associatividade
- existência do vetor zero
- existência de inverso aditivo
- distributividade
- identidade multiplicativa
- compatibilidade da multiplicação escalar

então $V$ é um espaço vetorial sobre $F$.

Essa abstração exige um certo nível de pensamento matemático, pois, diferentemente da geometria euclidiana ou analítica, um espaço vetorial não está limitado a duas ou três dimensões.

A analogia com pontos e setas ajuda como introdução, mas é apenas um caso particular.

O conceito de vetor aqui é amplo: qualquer objeto que satisfaça os axiomas pode desempenhar esse papel — números, funções, matrizes ou sequências.

## Subespaços

Subespaços se formam quando um grupo de vetores está confinado na mesma região, ainda que combinados. Tomemos o exemplo da seção 1.C de *Linear Algebra Done Right*, de Sheldon Axler. — esse conceito se torna muito intuitivo em $F^3$

Dado que temos

$$
\{(x_1, x_2, 0) : x_1, x_2 \in F\}
$$
Esse conjunto vive em $F^3$ e representa o plano $z = 0$.

São vetores confinados nas dimensões $x$ e $y$; ainda que combinados, eles não mudarão de plano.

Os matemáticos chamam isso de **fechado para soma** e **fechado para multiplicação por escalar**