# **SOLID**

### O que é SOLID?

S.O.L.I.D: Os 5 princípios da POO:
S — Single Responsiblity Principle (Princípio da responsabilidade única)
O — Open-Closed Principle (Princípio Aberto-Fechado)
L — Liskov Substitution Principle (Princípio da substituição de Liskov)
I — Interface Segregation Principle (Princípio da Segregação da Interface)
D — Dependency Inversion Principle (Princípio da inversão da dependência)

Esses princípios ajudam o programador a escrever códigos mais limpos, separando responsabilidades, diminuindo acoplamentos, facilitando na refatoração e estimulando o reaproveitamento do código.

Em PHP, teremos um exemplo simples de como funciona.

####1) SRP - Principio da responsabilidade única (Single responsability principle)
Uma classe deve ter um, e somente um, motivo para mudar.
Esse princípio declara que uma classe deve ser especializada em um único assunto e possuir apenas uma responsabilidade dentro do software, ou seja, a classe deve ter uma única tarefa ou ação para executar.

O princípio da responsabilidade única não se limita somente a classes, ele também pode ser aplicado em métodos e funções, ou seja, tudo que é responsável por executar uma ação, deve ser responsável por apenas aquilo que se propõe a fazer.


```php
<?php

class Order
{
    public function calculateTotalSum(){/*...*/}
    public function getItems(){/*...*/}
    public function getItemCount(){/*...*/}
    public function addItem($item){/*...*/}
    public function deleteItem($item){/*...*/}
}

class OrderRepository
{
    public function load($orderID){/*...*/}
    public function save($order){/*...*/}
    public function update($order){/*...*/}
    public function delete($order){/*...*/}
}

class OrderViewer
{
    public function printOrder($order){/*...*/}
    public function showOrder($order){/*...*/}
}
```
####2) OCP - Principio Aberto e Fechado (Single responsability principle)
Princípio Aberto-Fechado — Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação, ou seja, quando novos comportamentos e recursos precisam ser adicionados no software, devemos estender e não alterar o código fonte original.
```php
<?php

interface Remuneravel
{
    public function remuneracao();
}

class ContratoClt implements Remuneravel
{
    public function remuneracao()
    {
        //...
    }
}

class Estagio implements Remuneravel
{
    public function remuneracao()
    {
        //...
    }
}

class FolhaDePagamento
{
    protected $saldo;
    
    public function calcular(Remuneravel $funcionario)
    {
        $this->saldo = $funcionario->remuneracao();
    }
}
```

####3) LSP - Principio da substituição de Liskov (Liskov substitution principle)
Princípio da substituição de Liskov — Uma classe derivada deve ser substituível por sua classe base.

O princípio da substituição de Liskov foi introduzido por Barbara Liskov em sua conferência "Data abstraction" em 1987. A definição formal de Liskov diz que:

> "Se para cada objeto o1 do tipo S há um objeto o2 do tipo T de forma que, para todos os programas P definidos em termos de T, o comportamento de P é inalterado quando o1 é substituído por o2 então S é um subtipo de T"

Um exemplo mais simples e de fácil compreensão dessa definição. Seria:
>se S é um subtipo de T, então os objetos do tipo T, em um programa, podem ser substituídos pelos objetos de tipo S sem que seja necessário alterar as propriedades deste programa [Wikipedia](https://pt.wikipedia.org/wiki/Princ%C3%ADpio_da_substitui%C3%A7%C3%A3o_de_Liskov "Wikipedia")

Para facilitar o entendimento, veja esse princípio na prática neste simples exemplo:


```php
<?php

class A 
{
    public function getNome()
    {
        echo 'Meu nome é A';
    }
}

class B extends A 
{ 
    public function getNome()
    {
        echo 'Meu nome é B';
    }
}

$objeto1 = new A;
$objeto2 = new B;

function imprimeNome(A $objeto)
{
    return $objeto->getNome();
}

imprimeNome($objeto1); // Meu nome é A
imprimeNome($objeto2); // Meu nome é B
```

####4) ISP - Principio da substituição de Liskov (Interface segregation principle)
Princípio da Segregação da Interface — Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar.

Esse princípio basicamente diz que é melhor criar interfaces mais específicas ao invés de termos uma única interface genérica.

Vamos ver o ISP na prática através de códigos:

Em um cenário fictício para criação de um game de animais, teremos algumas aves que serão tratadas como personagens dentro do jogo. Sendo assim, criaremos uma interface Aves para abstrair o comportamento desses animais, depois faremos que nossas classes implementem essa interface, veja:

```php
<?php

interface Aves
{
    public function setLocalizacao($longitude, $latitude);
    public function renderizar();
}

interface AvesQueVoam extends Aves
{
    public function setAltitude($altitude);
}

class Papagaio implements AvesQueVoam
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    public function setAltitude($altitude)
    {
        //Faz alguma coisa   
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}

class Pinguim implements Aves
{
    public function setLocalizacao($longitude, $latitude)
    {
        //Faz alguma coisa
    }
    
    public function renderizar()
    {
        //Faz alguma coisa
    }
}
```

####4) DIP - Princípio da Inversão de Dependência (Dependency Inversion Principle)
Princípio da Inversão de Dependência — Dependa de abstrações e não de implementações.

De acordo com Uncle Bob, esse princípio pode ser definido da seguinte forma:
> 1. Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender da abstração.

> 2. Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.

No contexto da programação orientada a objetos, é comum que as pessoas confundam a Inversão de Dependência com a Injeção de Dependência, porém são coisas distintas, mas que relacionam entre si com um proposito em comum, deixar o código desacoplado.

> Importante: Inversão de Dependência não é igual a Injeção de Dependência, fique ciente disso! A Inversão de Dependência é um princípio (Conceito) e a Injeção de Dependência é um padrão de projeto (Design Pattern).

Vamos na prática através de códigos:

Em um cenário fictício para criação de um game de animais, teremos algumas aves que serão tratadas como personagens dentro do jogo. Sendo assim, criaremos uma interface Aves para abstrair o comportamento desses animais, depois faremos que nossas classes implementem essa interface, veja:

```php
<?php

interface DBConnectionInterface
{
    public function connect();
}


class MySQLConnection implements DBConnectionInterface
{
    public function connect()
    {
        // ...
    }
}

class OracleConnection implements DBConnectionInterface
{
    public function connect()
    {
        // ...
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
  
    // Faz alguma coisa
}
```

###End
