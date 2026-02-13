---
title: "Checked e Unchecked Exceptions no Java"
date: 2026-02-13
---

# Checked e Unchecked Exceptions no Java

A hierarquia de exceptions no Java não existe apenas para herdar atributos e comportamentos — ela também conta uma história.  
Navegar pela hierarquia de uma exception nos diz muito sobre sua natureza.

Neste post vou focar nas exceptions que herdam de `Exception` e nas que herdam de `RuntimeException`, também conhecidas como **checked** e **unchecked exceptions**, respectivamente.

Antes de falar das diferenças, entenda que também existe similaridade entre elas: ambas representam desvios no comportamento esperado (o fluxo feliz da aplicação).

## Checked Exceptions

Existem cenários onde é previsível a possibilidade de um erro acontecer. Por exemplo, ao ler um arquivo da memória existe a possibilidade de o arquivo não existir; ao tentar converter uma string para objeto, existe claramente a possibilidade de a conversão falhar devido a estruturas incompatíveis.  
As checked exceptions são um recurso para forçar o programador a tratar esses casos ainda em tempo de compilação.

É um recurso que deve ser usado com cautela, pois aumenta o acoplamento entre classes. O acoplamento é uma métrica diretamente proporcional a quanta informação uma classe sabe sobre outra. Então, quando uma classe define um método que lança uma checked exception, ela está descrevendo algo sobre sua forma de trabalhar e forçando a _caller_ a lidar com isso.

O problema ocorre quando essa forma de trabalhar muda, impactando todo mundo que usa — inclusive forçando a recompilação, já que a checked exception passa a fazer parte da assinatura do método.

Uncle Bob critica o uso excessivo desse tipo de exception no capítulo 7 do _Clean Code_. Segundo ele:

1. É possível desenvolver software robusto sem elas. Essa afirmação é evidenciada quando olhamos para linguagens como C++ e Python, que não aderiram a esse tipo de exception.
2. Elas quebram o OCP (_Open-Closed Principle_), dado que não conseguimos adicionar novos comportamentos sem que outras classes fiquem sabendo. Por exemplo, se você adiciona um método que lança uma checked exception, precisa ajustar esse novo contrato em todas as _callers_.

Por fim, ele termina dizendo para usar com cautela, apenas onde os benefícios superam os danos.

> Checked exceptions can sometimes be useful if you are writing a critical library: You must catch them. But in general application development the dependency costs outweigh the benefits.

## Unchecked Exceptions

Na prática, é o tipo mais comum de exception usada dentro de domínios. O conselho de usar com cautela de Bob parece ter sido ouvido pelo time do Java, já que até a biblioteca padrão possui muito mais unchecked exceptions do que checked exceptions.

Como sua classe pai sugere, esse tipo de exception é percebida e lançada apenas em tempo de execução, podendo descrever:

- um erro do programador (`ArithmeticException`);
- uma condição excepcional do domínio (`OrderNotFoundException`);
- ou até problemas externos que o programador não controla (`OptimisticLockException`).

Sobre acoplamento, diferente das checked que criam acoplamento sintático, as unchecked criam acoplamento comportamental.  
Acoplamento sintático implica impacto em cadeia e recompilação.  
Acoplamento comportamental implica mudança de comportamento não esperada pela _caller_.

Em sistemas bem modelados, o acoplamento comportamental tende a ter menor impacto de manutenção e oferece mais flexibilidade evolutiva.








