##Comandos DML

Os comandos DML tem como função principal **manipular** os dados presentes no banco de dados. Mais especificamente, os comandos DML permitem inserir, alterar e excluir os registros de um banco de dados. Para executar tais ações, os SGBDs trabalham com quatro tipos de comandos: `INSERT`, `DELETE`, `UPDATE` e `SELECT`.

###O Comando INSERT

Este comando permite a inserção de dados em uma determinada tabela. O trecho de código abaixo mostra a sintaxe básica deste comando:

```
INSERT INTO tabela (campos) VALUES (valores);
```

O parâmetro `tabela` contém o nome da tabela no qual o novo registro será inserido. O parâmetro `campos` contém o nome dos campos que serão preenchidos. Por fim, o parâmetro `valores` contém os valores de cada um dos campos declarados no parâmetro anterior. A ordem de declaração dos campos e seus valores garante que cada valor seja inserido corretamente em seu respectivo campo. Em outras palavras, a ordem dos campos e valores deve ser exatamente a mesma. Caso contrário, poderá haver inconsistência dos dados inseridos (Um campo recebe como valor o dado de outro campo, e vice-versa).

O trecho de código abaixo exemplifica a utilização do comando `INSERT` para inserir um novo registro na tabela:

```
/* insere novos dados nas categorias */
INSERT INTO categorias (nome) VALUES ("Outros");
INSERT INTO categorias (nome) VALUES ("Pessoal");
INSERT INTO categorias (nome) VALUES ("Trabalho");

/* insere um novo dado nos usuários */
INSERT INTO usuarios (nome, endereco, idade) VALUES ("João", "Rua do João, 1222", 27);

/* insere uma nova tarefa */
INSERT INTO tarefas (descricao, usuario_id, categoria_id) VALUES ("Corrigir trabalhos práticos", 1, 3);
```

###O Comando UPDATE

Este comando permite a atualização dos registros presentes no banco de dados. A sintaxe básica de utilização do comando `UPDATE` é:

```
UPDATE tabela
SET col1=val1,col2=val2
WHERE restricoes;
```

O parâmetro `tabela` contém o nome da tabela no qual seu registro será atualizado. Difrentemente dos outros comandos, o comando `UPDATE` vem acompanhado de duas cláusulas: `SET` e `WHERE`.

A cláusula `SET` é utilzada para definir quais colunas, e com quais valores elas serão atualizadas. Por isso, declara-se uma lista de atribuições `colN=valN` após a declaração da cláusula SET, de forma a informar o nome da coluna e seu novo valor, respectivamente.

A cláusula `WHERE` é utilzada para restringir quais registros sofrerão atualizações. Esta cláusula é extremamente importante pois, sem sua especificação, o comando UPDATE iria atualizar todos os registros presentes na tabela fornecida. As restrições são expressadas na forma de condições, onde todo o registro que atender a estas condições terá seus dados atualizados.

O trecho de código abaixo mostra um exemplo de utilização do comando `UPDATE`:

```
/* atualiza tarefas que já "venceram" */
UPDATE tarefas
SET descricao = 'DONE: ' + descricao
WHERE data_limita < NOW()
```

###O Comando DELETE

Este comando exclui um registro existente do banco de dados. A sintaxe básica de utilização do comando `DELETE` está descrita abaixo:

```
DELETE FROM tabela
WHERE restricoes;
```

O `DELETE` possui um parâmetro que informa a tabela na qual o registro será excluído, representado por `tabela`. Além disso, este comando também possui uma cláusula `WHERE`, que tem como função restringir quais registros serão excluídos. Da mesma forma como acontece no comando `UPDATE`, todos os registros podem ser excluídos se a cláusula `WHERE` não for especificada.

O trecho de código abaixo mostra um exemplo de utilização do comando `DELETE`:

```
/* exclui todas as tarefas */
DELETE FROM tarefas;

/* remove uma categoria específica */
DELETE FROM categorias
WHERE nome = 'Outros';
```

###O Comando SELECT

O comando `SELECT` representa um dos comandos mais importantes da linguagem SQL. É por meio do `SELECT` que o SGBD faz a busca e filtragem de registros existentes no banco de dados. Em outras palavras, o comando `SELECT` é o único comando existente para a consultda de dados em um banco de dados.

A sintaxe de utilização do comando está descrita abaixo:

```
SELECT campos FROM tabela
WHERE restricoes;
```

Basicamente, o comando `SELECT` admite dois parâmetros: `campos` e `tabela`. O parâmetro `campos` informa a lista de campos que serão recuperados para cada registro encontrado na execução do comando `SELECT`. Já o parâmetro `tabela` informa a tabela na qual os registros serão buscados.

Da mesma forma com que ocorre nos comandos `UPDATE` e `DELETE`, a cláusula `WHERE` restringe os registros que serão selecionados, de forma que os registros serão selecionados somente se seus dados estiverem de acordo com as restrições declaradas na cláusula `WHERE`. Para selecionar todos os registros basta **não** declarar as restrições.

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
