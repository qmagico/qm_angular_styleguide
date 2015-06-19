# Guia de estilo para Javascript e AngularJS

Este documento contém um resumo das padronizações de estilo que devem ser seguidas no código do QMágico.

1. Javascript
 1. [O que é importante saber (de javascript)](#o-que-é-importante-saber-de-javascript)
 1. [Favoreça "POJOs" (feijão com arroz)](#favoreça-pojos-feijão-com-arroz)
 1. [Classes e herança](#classes-e-herança)
 1. [Quanto menos this melhor](#quanto-menos-this-melhor)
 1. [API primeiro](#api-primeiro)
1. AngularJS
 1. [Tudo é um componente](#tudo-é-um-componente)
 1. [Modelo como serviço](#modelo-como-serviço)
 1. [Modelos singleton vs. "classes"](#modelos-singleton-vs-classes)
 
## O que é importante saber (de javascript)

Javascript é uma linguagem que normalmente a gente "empurra com a barriga", e aprende o mínimo necessário pra conseguir os resultados desejados.
Bom, aqui acabou essa história :-)

Por favor, certifique-se de que vc entende como a linguagem funciona, inclusive os recursos mais avançados, tipo:

* O que acontece com o escopo de variáveis acessadas via closure
* Como funciona o sistema de prtótipos da linguagem, e como usar isso pra criar "pseudoclasses" e fazer "pseudo-herança".
* Como funciona o this, e quais a armadilhas envolvidas em usá-lo

Ter segurança nos pontos acima vai te ajudar a fazer código com mais qualidade e menos bugs

## Favoreça "POJOs" (feijão com arroz)

Em javascript a gente pode criar objetos não tipados (Plain Old Javascript Objects = POJOs) e obter vários dos benefícios da orientação a objetos sem a necessidade de usar "classes", ou tipos. (Classe entre aspas porque, tecnicamente, JS não tem classes - ainda, anyway).

Ao criar novos objetos em javascript, mesmo que esses objetos tenham métodos, prefira usar o feijão-com-arroz.

```javascript
var tarefas_manager = {
    tarefas: [],
    add_tarefa: function(nova_tarefa){ // calma que isso aqui ainda não tá bom, segue lendo o documento aí
        tarefas_manager.tarefas.push(nova_tarefa);
    },
    remove_tarefa: function(indice){
        tarefas_manager.tarefas.splice(indice, 1);
    }
};
```

## Classes e herança

Quando vc tem um tipo de objeto que vai ser instanciado mais de uma vez na aplicação, e/ou quando vc precisa fazer herança, faz sentido usar o recurso de "classes" do javascript.

```javascript

// definição da "classe"
function Animal(nome){
    this.nome = nome;
}
Animal.prototype.barulho = function(){
    console.log(this.nome + 'fazendo barulho desconhecido');
}

// criando instâncias e chamando métodos:
var a1 = new Animal('felix');
var a2 = new Animal('garfield');
a1.barulho();
a2.barulho();
```


## Quanto menos this melhor
## API primeiro
## Tudo é um componente
## Modelo como serviço
## Modelos singleton vs. "classes"

