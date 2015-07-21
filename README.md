# Guia de estilo para Javascript e AngularJS

Este documento contém um resumo das padronizações de estilo que devem ser seguidas no código do QMágico.

1. Javascript
 1. [O que é importante saber (de javascript)](#o-que-é-importante-saber-de-javascript)
 1. [Favoreça "POJOs" (feijão com arroz)](#favoreça-pojos-feijão-com-arroz)
 1. [Classes e herança](#classes-e-herança)
 1. [Quanto menos this melhor](#quanto-menos-this-melhor)
 1. [Jogue Errors, não Strings](#jogue-errors-não-strings)
 1. [API primeiro](#api-primeiro)
1. AngularJS
 1. [Regras de nomenclatura](#regras-de-nomenclatura)
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

E se precisar fazer herança, é assim:

```javascript
// definição da "classe"
function Gato(nome){
    Animal.call(this, nome); // se precisar chamar o construtor do pai é assim que faz
}
jsutils.class_extend(Animal, Gato); // ver [1]

Gato.prototype.barulho = function(){
    console.log('Gato '+this.name+': MIAAAAAU');
}

// criando instâncias e chamando métodos:
var g1 = new Animal('felix');
var g2 = new Animal('garfield');
g1.barulho();
g2.barulho();
```

[1] [Herança em javascript](http://stackoverflow.com/questions/4152931/javascript-inheritance-call-super-constructor-or-use-prototype-chain)

## Quanto menos this melhor

Ao usar "classes" como acima, inevitavelmente precisamos lidar com o **this**, que é um carinha pra lá de problemático em JS.
Por isso, padronizamos que, pra minimizar o uso de this, e com isso a quantidade de bugs que podem acontecer porque o this não é quem a gente pensa, todo método de "classe" deve ter como primeira instrução uma atribuição de this a uma variável alternativa:

```javascript
Gato.prototype.barulho = function(){
    var g = this;
    console.log('Gato '+g.name+': MIAAAAAU');
    // possivelmente usa "g" mais vezes aqui.
}
```

## Jogue Errors, não Strings

Ao lançar (throw) exceções em javascript, evite jogar strings ou objetos não tipados. Jogue sempre uma instância de Error.
Errors têm **stack**, e stacks são do bem.

```javascript
throw 'Deu pau';            // ruim
throw {message: 'Deu pau'}; // ruim

throw new Error('Deu pau'); // BOM
```

## API primeiro

Na nossa profissão, ler código é tão importante (talvez mais) quanto escrever.
E a primeira pergunta que a gente tem na cabeça quanto tá olhando pra algum componente é "*O que que esse negócio faz?*"

Bom, em se tratando de um objeto, responder essa pergunta é muito mais rápido se vc consegue ver, logo de cara, uma lista dos métodos que esse objeto tem.

E a gente faz isso assim:

* POJOs:

```javascript
var tarefas_manager = {
    tarefas: [],
};

angular.extend(tarefas_manager, { // OBA! Olha a API do objeto todinha aí!
    add_tarefa: add_tarefa,
    remove_tarefa: remove_tarefa,
});

function add_tarefa(nova_tarefa){
    tarefas_manager.tarefas.push(nova_tarefa);
}

function remove_tarefa(indice){
    tarefas_manager.tarefas.splice(indice, 1);
}
```

* "Classes":

```javascript
function MinhaClasseBemLegal(p1, p2, p3){
    var mcbl = this;
    mcbl.p1 = p1;
    mcbl.p2 = p2;
    mcbl.p3 = p3;
}

angular.extend(MinhaClasseBemLegal.prototype, { // OBA! Olha a API da "classe" todinha aí!
    metodo1: metodo1,
    metodo2: metodo2,
});

function metodo1(nova_tarefa){
    var mcbl = this;
    //faz as coisas do metodo 1
}

function metodo2(indice){
    var mcbl = this;
    //faz as coisas do metodo 2
}
```

Uma vantagem "bônus" de fazer isso é que desse jeito, os métodos não são mais funções anônimas - o que facilita um pouco a hora que vc precisa debugar.

## Regras de nomenclatura

//TODO

## Tudo é um componente

A aplicação deve ser uma composição de componentes (diretivas 'E'), que usam outros componentes e assim por diante.
Não deve haver páginas "soltas" com a lógica implementada dentro de um big controller. 
Componentes assim são bons porque podem ser desenvolvidos e testados isoladamente (e o nosso DOCS [2] é o melhor lugar pra isso)

[2] [Vídeo: Dicas Anguleiras do QMágico](https://www.youtube.com/watch?v=IEkBZYC_WWc)

## Modelo como serviço

Tanto quanto possível, controllers não devem armazenar o estado da tela no `$scope`.
Ao invés disso, as telas devem ter um **modelo**.

Um modelo de uma tela é um objeto javascript que armazena o estado da tela, e contém métodos que fazem algum tipo de transformação nesse estado.

A implementação dos modelos de telas devem estar dentro de serviços angular, criados com `angular.module('my_module').factory()`.

Os templates simplesmente fazem binding com as coisas do modelo, com coisas do tipo `ng-model="m.nome"` ou `ng-click="m.add_tarefa()"`.

Esse é o pattern que a gente chama de *modelo como serviço* e está melhor explicado aqui [3].

[3] [Vídeo: Como fazer TDD com AngularJS](https://www.youtube.com/watch?v=95KBRKdOt0c)

## Modelos singleton vs. "classes"

Na maioria dos casos, uma implementação de modelo que retorna um POJO (como essa abaixo), resolve o problema:

```javascript
angular.module('my_module').factory('TarefasModel', function(){
    var m = {
        tarefas: [],
    };

    angular.extend(m, {
        add_tarefa: add_tarefa,
        remove_tarefa: remove_tarefa,
    });

    function add_tarefa(nova_tarefa){
        m.tarefas.push(nova_tarefa);
    }

    function remove_tarefa(indice){
        m.tarefas.splice(indice, 1);
    }

    return m;
});

angular.module('my_module').directive('tarefas', function(){
    return {
        restrict: 'E',
        scope: {},
        templateURL: '/caminho/do/template/tarefas.html',
        controller: function($scope, TarefasModel){
            var m = $scope.m = TarefasModel;
        }
    };
});
```

Mas, isso não vai funcionar se eu precisar colocar duas diretivas `<tarefas></tarefas>` mostrando listas de tarefas diferentes.
Não funciona porque, como serviços são singleton, as duas diretivas têm o mesmo modelo, e por isso sempre compartilhariam o mesmo estado.

A maneira de resolver esse problema, é alterar o serviço pra que ele retorne uma "classe" ao invés de um objeto POJO:

```javascript
angular.module('my_module').factory('TarefasModel', function(){

    function TarefasModel(){
        var m = this;
        m.tarefas = [];
    }

    angular.extend(TarefasModel.prototype, {
        add_tarefa: add_tarefa,
        remove_tarefa: remove_tarefa,
    });

    function add_tarefa(nova_tarefa){
        var m = this;
        m.tarefas.push(nova_tarefa);
    }

    function remove_tarefa(indice){
        var m = this;
        m.tarefas.splice(indice, 1);
    }

    return TarefasModel;
});

angular.module('my_module').directive('tarefas', function(){
    return {
        restrict: 'E',
        scope: {},
        templateURL: '/caminho/do/template/tarefas.html',
        controller: function($scope, TarefasModel){
            var m = $scope.m = new TarefasModel();
        }
    };
});
```

Desse jeito a gente contorna o problema de ter um único modelo: o **serviço angular** continua sendo singleton (só existe uma function `TarefasModel`), mas cada diretiva instancia seu prório modelo.

Então, quando usar uma coisa e quando usar outra?

--> Favoreça o POJO singleton, que é mais simples, a menos que:

* Vc precise de mais de uma instância do modelo; e/ou
* Vc precisa fazer herança entre modelos
