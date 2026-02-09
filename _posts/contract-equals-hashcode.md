---
title: "A relação entre equals e hashCode em estruturas de dados baseadas em hashing"
date: 2026-02-09
---

# A relação entre equals e hashCode em estruturas de dados baseadas em hashing

Em Java, todo objeto herda de `Object`, e `Object` define, entre outras coisas, os métodos equals e `hashCode`.

A implementação padrão de `equals` compara a posição de memória que abriga o objeto, logo esse exemplo retorna true.

```java
@Test
void test() {

  var x = new Object();
  var y = x;

  assertTrue(x.equals(y));
}
```

A implementação padrão pode ser sobrescrita, e na maioria das vezes faz sentido sobrescrevê-las dentro da camada de domínio. Por exemplo:

```java
public class Operation {

  String type;

  @Override
  public boolean equals(Object o) {
    if (o == null || getClass() != o.getClass()) return false;
    Operation operation = (Operation) o;
    return Objects.equals(type, operation.type);
  }

  @Override
  public int hashCode() {
    return Objects.hashCode(type);
  }
}
```

A implementação padrão do `hashCode` devolve um inteiro que representa o hash do objeto. A motivação para a existência desse método são os benefícios das tabelas hash, onde é possível obter acesso ao índice em tempo médio O(1) ao invés de O(n).
Tabelas hash são uma estratégia, e tal estratégia pode ser vista em implementações como `HashMap` e `HashSet`.

Apesar de serem métodos distintos que servem a propósitos distintos, existe um contrato entre eles, e o programador precisa estar atento a isso. Olhemos o que diz a doc do método `hashCode`:

> If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.

Tabelas hash usam um array interno. Os elementos desse array são buckets. O `hashCode` é usado como índice para esse array.
Os buckets, por sua vez, agrupam entries (um par chave/valor), e pode haver colisão de hashes, ou seja, um bucket pode conter n entries.
Como forma de ilustrar isso, podemos ver os buckets como gavetas: o `hashCode` nos diz qual gaveta abrir; estando a gaveta aberta, o `equals` nos diz qual é a entry correta.

Dado esse contexto, agora o contrato descrito na doc faz sentido. Vejamos na prática o que ocorre quando esse contrato não é respeitado.

```java
public class Car {

  private String color;

  public String getColor() {
    return color;
  }

  public void setColor(String color) {
    this.color = color;
  }

  @Override
  public boolean equals(Object o) {
    if (o == null || getClass() != o.getClass()) return false;
    Car car = (Car) o;
    return Objects.equals(color, car.color);
  }

  @Override
  public int hashCode() {
    return super.hashCode();
  }
}
```

```java
@Test
void test() {

  var c1 = new Car();
  c1.setColor("Blue");

  var c2 = new Car();
  c2.setColor("Blue");

  assertTrue(c1.equals(c2));

  var set = new HashSet<>();

  set.add(c1);

  assertTrue(set.contains(c2)); // fail
}
```

## O risco de usar objetos mutáveis como índices em estruturas de dados baseadas em hashing

Dado que você tem um objeto cujo hashing é calculado com base em seus atributos, se você mudar o valor desses atributos o valor do hashing mudará.
Se esse objeto estiver dentro de alguma estrutura de dados baseada em hashing, como um `HashSet`, por exemplo, acontecerá um mismatch quando você tentar recuperar esse objeto novamente.
Dado que o `hashCode` mudou, o índice do bucket muda também.

```java
@Test
void test() {

  var op = new Operation();

  var set = new HashSet<Operation>();

  set.add(op);

  assertTrue(set.contains(op));

  op.setType("sum"); // change the hash code

  assertTrue(set.contains(op)); // fail
}
```
