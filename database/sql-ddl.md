##Os Comandos DDL

Os comandos DDL são responsáveis por **manipular** objetos em um banco de dados. Mais especificamente, por meio dos comandos DDL, é possível criar, alterar e excluir toda a estrutura necessária para o banco de dados funcionar, tais como tabelas, índices, visões, etc. Esta seção dedica-se a mostrar o funcionamento básico de três comandos DDL: `CREATE`, `DELETE` e `ALTER`.

###Comando CREATE

O comando `CREATE` é responsável pela criação de objetos no Banco de Dados. O comando `CREATE` possui a sintaxe descrita abaixo.

```
CREATE objeto parametros
```

O trecho `objeto` representa o objeto do banco a ser criado (database, table, view, etc.), e o trecho `parametros` descreve os parâmetros necessários para criação de cada objeto.

####CREATE DATABASE

O comando `CREATE DATABASE` permite a criação de um banco de dados para se trabalhar. O comando possui a seguinte sintaxe: `CREATE DATABASE nome_do_banco`.

O trecho de código a seguir exemplifica a utilização do comando para criação do banco de dados *TodoList*.

```
/*  Criando um banco de dados... */
CREATE DATABASE TodoList;

/* Listando os banco de dados... */
SHOW DATABASES;

/* Selecionando o banco de dados... */
USE TodoList;

/* Saindo do banco de dados... */
QUIT;
```

####CREATE TABLE

O comando `CREATE TABLE` permite a criação de tabelas em um banco de dados. O comando para criação da tabela possui a seguinte sintaxe:

```
CREATE TABLE nome-da-tabela
(nome-do-campo1 tipo-do-campo1,
nome-do-campo2 tipo-do-campo2);
```

Os parâmetros do comando estão descritos em detalhes abaixo:
* **nome-da-tabela:** O nome da da tabela a ser criada
* **nome-do-campoN:** Nome de uma coluna da tabela
* **nome-da-tabela:** Tipo da coluna da tabela

É importante ressaltar que os tipos das colunas são pre-definidas pelo próprio SGBD. Esses tipos são considerados *tipos primários*, uma vez que são disponibilizados pelo próprio SGBD. A Tabela abaixo descreve os principais tipos primitivos existentes:

| Tipo            | Comando            	   | Descrição                                                      |
|-----------------|------------------------|----------------------------------------------------------------|
| Inteiro         | `integer` ou `int`     | Números entre -2.147.483.648 e 2.147.483.647                   |
| Inteiro Pequeno | `smallint`             | Números entre -32.768 e 32.767                                 |
| Decimal         | `decimal` ou `numeric` | Números com 30 casas decimais                                  |
| Dupla precisão  | `double`               | Números com dupla precisa de até 30 casas decimais             |
| Data            | `date`                 | Datas no formato AAAA-MM-DD                                    |
| Hora            | `time`                 | Para tempo no formado HH:MM:SS                                 |
| Caracteres      | `char(tamanho)`        | Para armazenar uma sequência de caracteres                     |
| Caracteres      | `varchar(tamanho)`     | Armazena uma sequência de caracteres, porém de forma otimizada |

Outros tipos de dados podem ser armazenados, tais como matrizes e arquivos binários (vídeos, imagens e documentos). Bancos de dados também podem apresentar tipos de dados exclusivos, como por exemplo o tipo booleano, onde em alguns bancos de dados existe o tipo boolean, enquanto em outros o tipo é smallint(1).

Ainda, é possível definir valores padrão para as colunas de uma Tabela no momento de sua criação. Para isso, basta utilizar a palavra `DEFAULT` no final da declaração da coluna, junto com o valor *default* propriamente dito.

O trecho de código abaixo contém um exemplo completo do comando para a criação da tabela *tasks* no banco de dados *TodoList*. É importante observar os demais comandos do trecho. Para criar uma tabela, antes de tudo é preciso selecionar em qual banco de dados essa tabela será criada. Essa informação pode ser dada por meio do comando `USE`. O comando `SHOW TABLES` mostra todas as tabelas existentes no banco de dados. Por fim, o comando `DESC` descreve as características de uma determinada tabela.

```
/* Selecionando o banco de dados... */
USE TodoList;

/* Criando uma tabela... */
CREATE TABLE usuarios (
codigo INT,
nome VARCHAR(40) NOT NULL,
endereco VARCHAR(60),
idade INT DEFAULT '18',
saldo DECIMAL(10,2));

/* Listando as tabelas... */
SHOW TABLES;

/* Descrevendo uma tabela... */
DESC usuarios;
```


###ALTER TABLE

Este comando permite alterar as características de uma determinada tabela, tais como renomear seu nome ou de suas colunas, alterar o tipo das colunas, ou mesmo adicionar novas colunas na tabela. O código abaixo mostra a sintaxe básica do comando `ALTER TABLE`.

```
ALTER TABLE nome_tabela
ACAO
```

O parâmetro `nome_tabela` representa o nome da tabela que sofrerá as alterações, e o parâmetro `ACAO` representa a ação de modificação a ser realizada na tabela. Mais especificamente, o comando `ALTER TABLE` admite as seguintes ações:

* Renomear nome da tabela
```
ALTER TABLE nome_tabela
CHANGE coluna_antiga coluna_nova tipo;
```
* Adicionar uma nova coluna
```
ALTER TABLE nome_tabela
ADD nome_coluna tipo_coluna;
```
* Remover uma coluna existente
```
ALTER TABLE nome_tabela
DROP nome_coluna;
```
* Alterar o tipo de uma coluna
```
ALTER TABLE nome_tabela
MODIFY COLUMN coluna novo_tipo;
```

O trecho de código abaixo exemplifica a utilização de cada uma das ações possíveis para o comando `ALTER TABLE`:

```
/* altera o nome de uma coluna */
ALTER TABLE usuarios
CHANGE codigo id INT;

/* adiciona uma nova coluna */
ALTER TABLE usuarios
ADD sexo VARCHAR(1);

/* remove uma determinada coluna */
ALTER TABLE usuarios
DROP saldo;

/* muda o tipo de uma determinada coluna */
ALTER TABLE usuarios
MODIFY COLUMN sexo BIT;
```

É preciso tomar um cuidado especial ao alterar as colunas quando a tabela já possui dados armazenados, uma vez que - ao alterar uma determinada coluna - seus dados poderão ser perdidos. Neste caso, recomenda-se a execução dos seguintes passos:

1. Identificar todas as restrições existentes onde esta tabela esteja envolvida
2. Remover todas as restrições
3. Adicionar uma nova coluna com nome diferente
4. Copiar os dados da coluna original para a nova usando uma função de conversão de tipos de dados
5. Remover a coluna original
6. Renomear a nova coluna para que fique com o mesmo nome que a original
7. Recriar as restrições identificadas inicialmente tendo em atenção que o tipo de dados mudou


###DROP

O comando `DROP` faz a remoção de objetos do banco de dados. Mais especificamente, por meio do `DROP` é possível remover índices, visões, tabelas e até mesmo um banco de dados inteiro, se necessário. O comando `DROP` possui a seguinte sintaxe: `DROP objeto nome_objeto`, onde o parâmetro `objeto` representa o tipo do objeto que será excluído, e o parâmetro `nome_objeto` representa o nome do objeto que será excluído.

O trecho de código abaixo mostra a utilização do comando `DROP` para remoção de alguns objetos de um banco de dados:

```
/* Remove tabela usuarios */
DROP TABLE usuarios;

/* Remove banco de dados TodoList */
DROP database TodoList;
```

###CONSTRAINT

O comando `CONSTRAINT` permite a criação de restrições no banco de dados. Essas restrições podem ser adicionadas de duas formas: (a) no momento da criação da tabela no banco de dados, ou após a criação da tabela, por meio do comando `ALTER TABLE`. Em ambos os casos, a criação de uma `CONSTRAINT` deve seguir o exemplo abaixo:

```
CONSTRAINT nome_constraint constraint parametros
```

O parâmetro `nome_constraint` representa o nome que será dado a nova `CONSTRAINT` criada. O parâmetro `constraint` representa qual restrição será aplicada. Por fim, o parâmetro `parametros` contém os parâmetros necessários para criação daquela restrição

Mais especificamente, este comando permite a adição das seguintes restrições:

* Chaves Primárias
```
CONSTRAINT nome_constraint PRIMARY KEY (colunas)
```
* Chaves Estrangeiras
```
CONSTRAINT nome_constraint FOREIGN KEY (coluna_local)
REFERENCES Persons(coluna_referenciada)
[ON UPDATE acao]
[ON DELETE acao]
```
* Chaves alternativas
```
CONSTRAINT nome_constraint UNIQUE (colunas)
```
* Campos obrigatórios: Exceção aos demais comandos, uma vez que se utiliza o comando `MODIFY` para adicionar tal restrição.
```
MODIFY coluna tipo_coluna NOT NULL;
```

O trecho de código abaixo exemplifica o uso de tais restrições em um banco de dados:

```
/* adiciona uma chave primária a tabela usuários */
ALTER TABLE usuarios
ADD CONSTRAINT pk_usuarios PRIMARY KEY (id);

/* cria tabela categoria */
CREATE TABLE categorias(
id INT AUTO_INCREMENT NOT NULL,
nome VARCHAR(50),

CONSTRAINT pk_categoria PRIMARY KEY (id),

CONSTRAINT uk_categoria UNIQUE (nome)
);

/* adiciona uma restrição a tabela categorias */
ALTER TABLE categorias
MODIFY nome VARCHAR(50) NOT NULL;

/* cria tabela tarefas */
CREATE TABLE tarefas (
id INT AUTO_INCREMENT NOT NULL,
descricao VARCHAR(200),
data_limite DATE,
usuario_id INT,
categoria_id INT,

CONSTRAINT pk_tarefa PRIMARY KEY (id),

CONSTRAINT fk_tarefa_categoria FOREIGN KEY (categoria_id)
	REFERENCES categorias(id),

CONSTRAINT fk_tarefa_usuario FOREIGN KEY (usuario_id)
	REFERENCES usuarios(id)
	ON DELETE CASCADE
);
```
