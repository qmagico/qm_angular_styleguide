# Guia de estilo para Javascript e AngularJS

Este documento contém um resumo das padronizações de estilo que devem ser seguidas no código do QMágico.

1. Javascript
  1. a
  1. b
1. Javascript

 * 1.1 O que é importante saber (de javascript)
 * 1.2 Favoreça "POJOs" (feijão com arroz)
 * 1.3 Classes e herança
 * 1.4 Quanto menos this melhor
 * 1.5 API primeiro
* 2. AngularJS
 * 2.1 Tudo é um componente
 * 2.2 Modelo como serviço
 * 2.3 Modelos singleton vs. "classes"
 
## 1.1 O que é importante saber

Javascript é uma linguagem que normalmente a gente "empurra com a barriga", e aprende o mínimo necessário pra conseguir os resultados desejados.
Bom, aqui acabou essa história :-)

Por favor, certifique-se de que vc entende como a linguagem funciona, inclusive os recursos mais avançados, tipo:

* O que acontece com o escopo de variáveis acessadas via closure
* Como funciona o sistema de prtótipos da linguagem, e como usar isso pra criar "pseudoclasses" e fazer "pseudo-herança".
* Como funciona o this, e quais a armadilhas envolvidas em usá-lo

Ter segurança nos pontos acima vai te ajudar a fazer código com mais qualidade e menos bugs
