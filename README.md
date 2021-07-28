# Pratica : Meli-toolkits & Microsserviços

## Exercicio 01:

O timeout da minha API seria de 150ms apenas. Isso permitiria que a mesma tenha 1 retry simples. Dessa maneira como temos um P95 de 150ms, mesmo que apos as duas requisições falha (a primeira e o retry) o processo todo demorou somente 300ms, ainda tem-se 200ms para procurar em um cache o ultimo valor registrado e continuar o processamento e responder a requisicão.

Como a taxa de erro da discount-api já é de 0,9% em caso de falha dela, a minha api iria usar os dados previamentes cacheados da consulta dela para sanar essa taxa de erro. Para manter tudo menor que 0.5% a minha API vai ficar planejada para o pior cenario da discount-api, além de caso a mesma demore processar algo que possa estrapolar o limite total de 500ms, será retornado o valor mais recente presente na cache para evitar travamento do processo e um eventual retorno de erro.

## Exercicio 02:

Isso pode ocorrer porque estamos fazendo muitas requisiçoes (a original e as 2 tentativas) em um pequeno tempo, e com isso o servidor bloqueia o nosso acesso à ele. Para evitar isso pode-se aumentar o timeout e/ou adicionar um tempo de espera entre os retries (timeout exponencial). 	E mesmo assim caso o erro ainda persista, para evitar jogar um erro de 500, podemos usar partial content e avisar para o usuario/cliente que o dado esta indisponivel no momento e pedir para ele reacessar (ex.: recarregar a pagina web) o conteudo. Outro cenario é verificar se no header de resposta tem o "retry-afeter" indicando o tempo de espera para a nova tentativa, sendo ela passada para a aplicação antes de fazer a nova tentativa.
