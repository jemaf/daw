# Consultas de Agrupamento

Em determinadas situações o usuário pode desejar fazer consultas no banco de dados de forma agrupada. Essa forma de consulta permite ao usuário visualizar as informações dos registros de forma mais geral, possibilitando que o mesmo capture informações resumidas, porém de grande importância, do sistema.

Por meio das consultas de agrupamento, o usuário pode, por exemplo:

* Descobrir a média de vendas de seus funcionários no mês
* Visualizar o total de vendas em cada semana da sua loja
* Visualizar quantos usuários pertencem a um determinado grupo
* Descobrir quais alunos passaram em uma determinada disciplina
* Verificar quantas tarefas cada usuário possui em aberto

A consulta por agrupamento é feita por meio de um comando `SELECT`, porém adicionado da cláusula `GROUP BY`. O trecho de código abaixo descreve a sintaxe básica da cláusula `GROUP BY`:

```
SELECT campo1, FUNCAO(campo2), ...
...
GROUP BY campo1
```

O parâmetro `campo1` representa o campo no qual a cláusula `GROUP BY` irá agrupar os registros resultantes do `SELECT`. É possível fazer consultas que agrupem por um ou mais campos, a depender das necessidades do usuário. O parâmetro `FUNCAO(campo2)` representa uma função agregadora, que irá agrupar todos os valores de `campo2` de acordo com o valor de `campo1`.

É importante ressaltar que o uso de uma função agregadora é **obrigatória** para consultas de agrupamento. Pois caso não seja fornecida, o SGBD não saberá como ele irá agrupar os registros que tem o mesmo valor para o campo de agrupamento, porém valores distintos para os demais grupos. O trecho de código abaixo mostra alguns exemplos de utilização:

```
/* busca pelo número de tarefas de cada usuário */
SELECT usuarios.nome, COUNT(tarefas.id)
FROM usuarios
JOIN tarefas ON tarefas.usuario_id = usuarios.id
GROUP BY usuarios.nome;

/* busca pelo número de tarefas abertas por categoria */
SELECT tarefas.categoria_id, COUNT(tarefas.id)
FROM tarefas
WHERE tarefas.data_limite > NOW()
GROUP BY tarefas.categoria_id;

/* busca pela quantidade de tarefas abertas por categoria para cada usuário */
SELECT usuarios.nome, categorias.nome, COUNT(tarefas.id)
FROM usuarios
JOIN tarefas ON tarefas.usuario_id = usuarios.id
JOIN categorias ON tarefas.categoria_id = categorias.id
WHERE tarefas.data_limite > NOW()
GROUP BY usuarios.nome, categorias.nome
```

## A Cláusula HAVING

Como podemos observar, a execução do `GROUP BY` acontece após a filtragem realizada pelo `WHERE`. Em outras palavras, não é possível fazer filtragens dos valores de agrupamento diretamente na cláusula `WHERE`. Para isso, a linguagem SQL disponibilizou um comando específico para filtragem de valores resultantes de uma função de agregação: a cláusula `HAVING`. O trecho de código abaixo descreve a sintaxe básica de utilização da filtragem por meio do `HAVING`:

```
SELECT campo1, FUNCAO(campo2), ...
...
GROUP BY campo1
HAVING restricao;
```

Como podemos observar, a declaração da cláusula `HAVING` deve vir após o `GROUP BY`. Ainda, o parâmetro `restricao` deve contemplar restrições que fazem o filtro com base no valor resultante da função agregadora. O trecho de código abaixo mostra como ficaria algumas das consultas descritas anteriormente com a implementação da cláusula `HAVING`:

 ```
/* busca por usuários que tem pelo menos uma tarefas em aberto */
SELECT usuarios.nome
FROM usuarios
JOIN tarefas ON tarefas.usuario_id = usuarios.id
GROUP BY usuarios.nome
HAVING COUNT(tarefas.id) > 0;

/* busca por categorias que tem pelo menos 5 tarefas */
SELECT tarefas.categoria_id, COUNT(tarefas.id)
FROM tarefas
WHERE tarefas.data_limite > NOW()
GROUP BY tarefas.categoria_id
HAVING COUNT(tarefas.id) >= 5;
```