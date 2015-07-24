# Funções SQL

A linguagem SQL disponibiliza diversas funções que auxiliam na realização de determinadas consultas ao banco de dados. Mais especificamente, essas funções são divididas em quatro grupos principais:

* Funções de Data e Tempo
* Funções para Dados Nulos
* Funções Escalares
* Funções de Agregação


Como tais funções não fazem parte da especificação da linguagem SQL, fica a cargo do SGBD sua implementação. Portanto, os nomes, e a forma de utilização dessas funções variam de SGBD para SGBD. Este documento irá mostrar as funções dispníveis no MySQL.

## Funções de Data e Tempo

As funções de data e tempo são muito utilizadas para recuperar auditar e recuperar registros com base em um intervalo de tempo. O MySQL disponibiliza várias funções para manipulação de datas e períodos de tempo. A tabela abaixo sumariza as principais funções existentes:

| Função     | Descrição                                 |
|------------|-------------------------------------------|
| CURDATE()  | Retorna a data atual                      |
| DATE_ADD() | Adiciona um intervalo de tempo a uma data |
| DATE_SUB() | Subtrai um intervalo de tempo a uma data  |
| DATE()     | Extrai uma data a partir de um DATETIME   |
| DATEDIFF() | Subtrai duas datas                        |
| EXTRACT()  | Extrai parte de uma data                  |
| HOUR()     | Extrai a hora de uma data                 |
| NOW()      | Retorna o DATETIME atual                  |
| TIME()     | extrai o TIMESTAMP de um DATETIME         |
| YEAR()     | Extrai a hora de um DATETIME              |

## Funções para Dados Nulos

Essas funções tem como propósito lidar com situações onde o dado a ser manipulado possui o valor `NULL`. Por meio dessas funções, é possível fazer o tratamento de valores `NULL`, tais como substituir um `NULL` para um valor default.

Para isso, o MySQL disponibiliza a função `IFNULL`. Essa função verifica se um campo é nulo, e caso afirmativo, substitui seu valor por um *default*, informado na própria função. O trecho de código abaixo mostra um exemplo de utilização dessa função:

```
SELECT email, IFNULL(nome, 'Desconhecido') AS nome
from usuarios;
```

## Funções Escalares

As funções escalares tem como finalidade atuar sobre os diferentes valores e tipos de dados, de forma a modificá-los e processá-los de alguma forma. O MySQL possuí várias funções voltadas para esse propósito, tais como manipulação de strings, arrendondamento de valores, entre outros. Dentre as principais, podemos citar:

* **UCASE** e **LCASE**: Convete uma string para caixa alta e caixa baixa, respectivamente.

```
SELECT UCASE(nome), LCASE(email)
FROM usuarios;
```

* **MID**: Obtém uma subsequencia de caracteres de uma string.

```
SELECT MID(descricao, 0, 10)
FROM tarefas;
```

* **LENGTH**: Obtém o tamanho de uma string.

```
SELECT nome, LENGTH(nome)
FROM usuarios;
```

* **ROUND**: Arredonda um número com base na quantidade de casas decimais especificadas.

```
SELECT nota, ROUND(nota, 0) AS nota_final
FROM alunos;
```

* **NOW**: Obtém data e hora atuais do sistema.

```
SELECT * FROM tarefas
WHERE data_limite > NOW();
```

* **DATE_FORMAT**: Altera o formato de exibição de um DATETIME.

```
SELECT descricao, DATE_FORMAT(data_limite, '%d/%m/%Y') FROM tarefas
WHERE data_limite > NOW();
```

## Funções de Agregação

As funções de agregação são de grande importância para a consulta no banco de dados. Tais funções agregam os valores de determinadas coluas, produzindo um único valor como resposta. Por meio das funções de agregação é possível obter a média, quantidade, valor máximo e mínimo de determinados campos. A tabela abaixo mostra as principais funções de agregação existentes no MySQL:

| Função  | Descrição                                 | Exemplo                                          |
|---------|-------------------------------------------|--------------------------------------------------|
| AVG()   | Calcula a média dos valores de uma coluna | ``` SELECT AVG(salario) FROM trabalhadores;  ``` |
| COUNT() | Calcula a quantidade de registros         | ``` SELECT COUNT(id) FROM tarefas; ```            |
| FIRST() | Retorna o primeiro campo da consulta      | ``` SELECT FIRST(nome) FROM usuarios; ```        |
| LAST()  | Retorna o último campo da consulta        | ``` SELECT LAST(nome) FROM usuarios; ```         |
| MAX()   | Retorna o valo máximo da coluna           | ``` SELECT MAX(idade) FROM alunos; ```           |
| MIN()   | Retorna o valor mínimo da coluna          | ``` SELECT MIN(salario) FROM trabalhadores; ```  |
| SUM()   | Retorna a soma dos valores de uma coluna  | ``` SELECT SUM(salario) FROM trabalhadores; ```  |

Geralmente, não se utiliza funções de agregação em consultas simples. Tais funções possuem grande utilidade quando utilizadas em conjunto com comandos de agrupamento (`GROUP BY`).