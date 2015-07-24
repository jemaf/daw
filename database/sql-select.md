#Fazendo consultas no SQL

A consulta de informações representa uma das principais funcionalidades em um banco de dados. Uma consulta é capaz de fornecer informações para os mais diversos propósitos, desde os dados de um cliente específico, até o lucro mensal da empresa. Portanto, é fundamental que o usuário do banco de dados entenda corretamente como funciona as funcionalidades de consultas disponíveis na linguagem SQL.

Este documento tem como objetivo demonstrar os principais recursos disponíveis para consulta de dados na linguagem SQL.


##O Comando SELECT

Para realizar consultas ao banco de dados na liguagem SQL, utiliza-se o comando `SELECT`. A sintaxe simplificada do comando `SELECT` pode ser vista no trecho de código abaixo:

```
SELECT campos FROM tabela
WHERE restricoes;
```

Basicamente, o comando `SELECT` admite dois parâmetros: `campos` e `tabela`. O parâmetro `campos` informa a lista de campos que serão recuperados para cada registro encontrado na execução do comando `SELECT`. Já o parâmetro `tabela` informa a tabela na qual os registros serão buscados.

A cláusula `WHERE` restringe os registros que serão selecionados, de forma que os registros serão selecionados somente se seus dados estiverem de acordo com as restrições declaradas na cláusula `WHERE`. Para selecionar todos os registros basta **não** declarar as restrições.

O trecho de código abaixo mostra um exemplo de utilização do comando `SELECT`:

```
/* seleciona todas as categorias */
SELECT * FROM categorias;

/*
	seleciona a descrição e a data limite das
	tarefas que ainda não foram concluídas
*/
SELECT descricao, data_limite FROM tarefas
WHERE data_limite > NOW();
```

###A Cláusula WHERE

Quando o comando `SELECT` é utilizado, ele recupera todos os dados da tabela. No entanto, é comum o usuário do banco de dados desejar filtrar os dados provenientes da consulta. Dessa forma ele pode analisar somente os registros que lhe interessa. A linguagem SQL disponibiliza uma cláusula específica para fazer esse tipo de filtragem: `WHERE`. A sintaxe da cláusula `WHERE` pode ser vista abaixo:

```
SELECT ...
WHERE restricoes
```

O funcionamento da cláusula `WHERE` é extremamente simples. Para cada registro, ela verifica se esse registro obedece as restrições declaradas e, caso afirmativo, esse registro é filtrado e selecionado para exibição.

A cláusula é composta por uma série de **restrições**. Tais restrições são utilizadas justamente para a filtragem dos registros. Essas restrições devem ser declaradas na forma comparativa, similar as expressões condicionais de uma linguagem de programação. O trecho de código abaixo mostra o exemplo de um `SELECT` restringido por meio de uma restrição declarada no `WHERE`:

```
/* Seleciona usuários que possuem João em seu nome */
SELECT * FROM usuarios
WHERE nome LIKE '%João%';
```

A linguagem SQL traz consigo uma série de operadores que podem ser utilizados para estabelecer restrições na cláusua `WHERE`:

| Operador     | Descrição                           | Exemplo                          |
|--------------|-------------------------------------|----------------------------------|
| `=`            | Igual                               | `WHERE id = 2`                     |
| `<>` ou `!=`     | Diferente                           | `WHERE data <> CURDATE()`          |
| `>`            | Maior que                           | `WHERE idade > 18`                 |
| `<`            | Menor que                           | `WHERE renda < 1800.00`            |
| `>=`           | Maior ou igual a                    | `WHERE peso >= 70`                 |
| `<=`           | Menor ou igual a                    | `WHERE altura <= 170`              |
| `BETWEEN`      | Em um intervalo incluso             | `WHERE idade BETWEEN 20 AND 30`    |
| `LIKE`         | Padrão textual                      | `WHERE nome LIKE '%Alfredo%'`      |
| `IN` ou `EXISTS` | Faz parte de um conjunto de valores | `WHERE id IN (20, 21, 25, 37, 40)` |


Ainda, é possível combinar restrições por meio de operadores lógicos. A linguagem SQL possui dois operadores lógicos para isso: `AND`e `OR`. O operador `AND` filtra o registro caso atenda a todas as restrições. Já o operador `OR` filtra o registro caso atenda a pelo menos uma restrição. O trecho de código abaixo mostra dois exemplos de consulta, uma utilizando cada tipo de operador:

```
SELECT * FROM usuarios
WHERE idade >= 18 AND sexo = 'M';

SELECT * FROM usuarios
WHERE nome LIKE '%José%' OR email LIKE '%José%';
```


Como podemos observar, o comando `SELECT` permite que se selecione dados de uma tabela de forma simples e direta. No entanto, a maioria dos sistemas possui uma base de dados composta por várias tabelas, e muitas vezes a informação que o usuário deseja buscar é obtida apenas a partir da combinação dos dados de várias tabelas. Neste caso, é necessário fazer um cruzamento dos dados dessas tabelas para obter a informação desejada.
