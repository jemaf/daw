#Consultas em Várias Tabelas

Grande parte das consultas realizadas em um banco de dados dependem de informações presentes em mais de uma tabela. Por exemplo: *"Desejo recuperar todas as tarefas e o nome de seus respectivos usuários"*. Para conseguir recuperar tal informação, é necessário acessar a tabela **usuarios** para recuperar o nome do usuário, e a tabela **tarefas**, para recuperar as tarefas existentes.

Para conseguir recuperar essas informações, é necessário fazer uma consulta que combine os dados de várias tabelas, de forma a obter a informação desejada. O comando `SELECT` permite fazer esse cruzamento de dados por meio da junção de tabelas. Mais especificamente, esta junção pode ser feita de duas formas:

1. Utilizando a cláusula `FROM`
2. Utilizando a cláusula `JOIN`

##Utilizando FROM para junção de tabelas

A utilização da cláusula `FROM` era a forma padrão para junção de tabelas em antigas versões da linguagem SQL. Basicamente, a junção é feita através da declaração de mais de uma tabela na cláusula `FROM` de um comando `SELECT`. Ao se utilizar a cláusula `FROM` para fazer junção de tabelas, o SGBD faz um produto cartesiano entre as tabelas selecionadas<!--, gerando todas as combinações possíveis entre os registros das tabelas declaradas no `FROM`-->. Posteriormente, a filtragem desses registros é feito por meio das restrições declaradas na cláusula `WHERE`.

O trecho de código abaixo mostra a sintaxe básica de junção por meio do `FROM`:

```
SELECT campos FROM tabela1, tabela2, tabelaN
WHERE tabela1.pkey = tabela2.fkey AND tabela2.pkey = tabelaN.fkey
```

Como se pode observar, a relação entre as tabelas é feita por meio da comparação entre as chaves primárias e estrangeiras que cada uma possui. Nota-se também que a referência dos campos deve estar sempre atrelado a tabela na qual aquele campo pertence. Sem essa relação, o SGBD não saberia distinguir qual campo pertence a qual tabela, inviabilizando assim as comparações de suas chaves. O trecho de código abaixo representa a consulta descrita no início dessa seção:

```
SELECT tarefas.*, usuarios.nome FROM tarefas, usuarios
WHERE tarefas.user_id = usuarios.id
```

É possível atribuir nome as tabelas, de forma a facilitar a escrita de consultas mais elaboradas. O exemplo abaixo mostra como ficaria a consulta anterior com as tabelas devidamente nomeadas:

```
SELECT t.*, u.nome FROM tarefas t, usuarios u
WHERE t.user_id = u.id
```

### O problema do Produto Cartesiano

Como já dito anteriormente, a junção por meio do `FROM` gera um produto cartesiano de todas os registros das tabelas envolvidas na consulta. O produto cartesiano nada mais é do que a combinação de um registro de uma tabela com todas as demais. Como se pode imaginar, tal ação implica um overhead de processamento, uma vez que o SGBD seleciona todas as combinações possíveis, e somente então descarta as que não se encaixam nas restrições impostas pela cláusula `WHERE`.

Para exemplificar o funcionamento do produto cartesiano, vamos analisar o exemplo a seguir:

<table>
  <tr>
    <th colspan="4">usuarios</th>
  </tr>
  <tr>
    <td>id</td>
    <td>nome</td>
    <td>email</td>
    <td>senha</td>
  </tr>
  <tr>
    <td>01</td>
    <td>Joao</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>02</td>
    <td>Pedro</td>
    <td>pedro@email.com</td>
    <td>123</td>
  </tr>
</table>

<table>
  <tr>
    <th colspan="4">tarefas</th>
  </tr>
  <tr>
    <td>id</td>
    <td>descricao</td>
    <td>usuarios_id</td>
    <td>categorias_id</td>
  </tr>
  <tr>
    <td>01</td>
    <td>Fazer dever</td>
    <td>01</td>
    <td>01</td>
  </tr>
  <tr>
    <td>02</td>
    <td>Lavar carro</td>
    <td>01</td>
    <td>02</td>
  </tr>
  <tr>
    <td>03</td>
    <td>Comprar presente</td>
    <td>02</td>
    <td>02</td>
  </tr>
  <tr>
    <td>04</td>
    <td>Fazer jantar</td>
    <td>01</td>
    <td>02</td>
  </tr>
</table>

A Tabela **usuarios** contém os dados dos usuários cadastrados no sistema. Já a tabela **tarefas** contém as tarefas de todos os usuários cadastrados. Em um determinado momento, o usuário do banco de dados deseja recuperar todas as tarefas do João. Para isso, o usuário escreve a seguinte consulta:

```
SELECT tarefas.*, usuarios.* FROM tarefas, usuarios
WHERE tarefas.usuarios_id = usuarios.id AND usuarios.nome LIKE '%João%'
```

Ao executar essa consulta, o SGBD primeiro faz o produto cartesiano, e somente então filtras as tarefas. Neste exemplo, a consulta geraria a seguinte tabela:

<table>
  <tr>
    <th colspan="8">tarefas_usuarios</th>
  </tr>
  <tr>
    <td>id</td>
    <td>descricao</td>
    <td>usuarios_id</td>
    <td>categorias_id</td>
    <td>id</td>
    <td>nome</td>
    <td>email</td>
    <td>senha</td>
  </tr>
  <tr>
    <td>01</td>
    <td>Fazer dever</td>
    <td>01</td>
    <td>01</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>02</td>
    <td>Lavar carro</td>
    <td>01</td>
    <td>02</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>03</td>
    <td>Comprar presente</td>
    <td>02</td>
    <td>02</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>04</td>
    <td>Fazer jantar</td>
    <td>01</td>
    <td>02</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>01</td>
    <td>Fazer dever</td>
    <td>01</td>
    <td>01</td>
    <td>02</td>
    <td>Pedro</td>
    <td>pedro@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>02</td>
    <td>Lavar carro</td>
    <td>01</td>
    <td>02</td>
    <td>02</td>
    <td>Pedro</td>
    <td>pedro@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>03</td>
    <td>Comprar presente</td>
    <td>02</td>
    <td>02</td>
    <td>02</td>
    <td>Pedro</td>
    <td>pedro@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>04</td>
    <td>Fazer jantar</td>
    <td>01</td>
    <td>02</td>
    <td>02</td>
    <td>Pedro</td>
    <td>pedro@email.com</td>
    <td>123</td>
  </tr>
</table>

Esta tabela então sofreria a filtragem dos dados por meio da cláusula `WHERE`, gerando o seguinte resultado:

<table>
  <tr>
    <th colspan="8">tarefas_usuarios</th>
  </tr>
  <tr>
    <td>id</td>
    <td>descricao</td>
    <td>usuarios_id</td>
    <td>categorias_id</td>
    <td>id</td>
    <td>nome</td>
    <td>email</td>
    <td>senha</td>
  </tr>
  <tr>
    <td>01</td>
    <td>Fazer dever</td>
    <td>01</td>
    <td>01</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>02</td>
    <td>Lavar carro</td>
    <td>01</td>
    <td>02</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
  <tr>
    <td>04</td>
    <td>Fazer jantar</td>
    <td>01</td>
    <td>02</td>
    <td>01</td>
    <td>João</td>
    <td>joao@email.com</td>
    <td>123</td>
  </tr>
</table>

##Junção de tabelas com a cláusula JOIN

A cláusula `JOIN` surgiu como solução para prover a junção de tabelas de forma mais eficiente. Diferentemente do que é feito na cláusula `FROM`, o `JOIN` seleciona os registros com base nas chaves antes de executar a filtragem do `WHERE`. Desta forma, a filtragem é feita em uma quantidade muito menor de registros, tornando a consulta mais rápida.

O `JOIN` faz a junção das tabelas com base na lógica de conjuntos. Imagine dois conjuntos distintos, onde cada conjunto representa uma determinada tabela. A cláusula `JOIN` fornece funcionalidades para realizar operações de conjuntos essas tabelas. Assim é possível recuperar dados que são em comum entre as duas tabelas (interseção), ou todos os dados das duas tabelas (união). A figura abaixo mostra graficamente as possibilidades que a cláusula `JOIN` é capaz de fornecer:

![A cláusula JOIN](sql_joins.png)

Mais especificamente, a cláusula `JOIN` disponibiliza quatro tipos de junções, seguida da sintaxe básica de cada uma:

* **JOIN:** Listar os registros que possuem relacionamento nas duas tabelas.

```
SELECT campos FROM tabela1
JOIN tabela2 ON tabela1.pkey = tabela2.fkey
[WHERE restricoes]
```

* **LEFT JOIN:** Listar todos os registros da tabela da esquerda relacionando com os registros da tabela da direita, e caso não haja relacionamento, preencher os campos com `null`.

```
SELECT campos FROM tabela1
LEFT JOIN tabela2 ON tabela1.pkey = tabela2.fkey
[WHERE restricoes]
```

* **RIGHT JOIN:** Listar todos os registros da tabela da direita relacionando com os registros da tabela da esquerda, e caso não haja relacionamento, preencher os campos com `null`.

```
SELECT campos FROM tabela1
RIGHT JOIN tabela2 ON tabela1.pkey = tabela2.fkey
[WHERE restricoes]
```

* **FULL JOIN**: Listar todos os registros, e caso não haja relacionamento (tanto para esquerda quanto para direita), preencher os campos com `null`. *OBS: O MySQL não possui  FULL JOIN*.

```
SELECT campos FROM tabela1
FULL JOIN tabela2 ON tabela1.pkey = tabela2.fkey
[WHERE restricoes]
```

Naturalmente, a cláusula mais utilizada é o `JOIN`, uma vez que raramente é necessário ter conhecimento de registros que não possuem relação entre as tabelas utilizadas na junção.