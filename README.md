# Guia de estilo para Javascript e AngularJS

Este documento contém um resumo das padronizações de estilo que devem ser seguidas no código do QMágico.

1. Javascript
 1. O que é importante saber (de javascript)
 1. Favoreça "POJOs" (feijão com arroz)
 1. Classes e herança
 1. Quanto menos this melhor
 1. API primeiro
1. AngularJS
 1. Tudo é um componente
 1. Modelo como serviço
 1. Modelos singleton vs. "classes"
 
## O que é importante saber (de javascript)
## Favoreça "POJOs" (feijão com arroz)
## Classes e herança
## Quanto menos this melhor
## API primeiro
## Tudo é um componente
## Modelo como serviço
## Modelos singleton vs. "classes"

Javascript é uma linguagem que normalmente a gente "empurra com a barriga", e aprende o mínimo necessário pra conseguir os resultados desejados.
Bom, aqui acabou essa história :-)

Por favor, certifique-se de que vc entende como a linguagem funciona, inclusive os recursos mais avançados, tipo:

* O que acontece com o escopo de variáveis acessadas via closure
* Como funciona o sistema de prtótipos da linguagem, e como usar isso pra criar "pseudoclasses" e fazer "pseudo-herança".
* Como funciona o this, e quais a armadilhas envolvidas em usá-lo

Ter segurança nos pontos acima vai te ajudar a fazer código com mais qualidade e menos bugs
