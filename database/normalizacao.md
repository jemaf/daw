# Normalização

Como sabemos, o principal propósito de um banco de dados é prover uma forma de armazenamento de dados que seja confiável. Para isso, os Sistemas Gerenciadores de Banco de Dados (SGBDs) implementam uma série de restrições que auxiliam o Administrador do Banco de Dados (DBA) na manutenção da integridade dos dados existentes.

Contudo, tais restrições não garantem por completo a integridade dos dados. O mal planejamento do esquema do banco de dados pode, como consequência, gerar falhas de integridade sistêmicas em sua estrutura, tais como redundância de dados, falta de coesão entre as informações armazenadas, entre outras.

O processo de **normalização** tem como **objetivo identificar e corrigir** as falhas estruturais eventualmente ocasionadas pelo DBA no momento da criação do esquema do banco de dados. O processo de normalização é composto de três passos principais, denominadas *Formas Normais*, que devem ser executados de forma sequêncial. Cada Forma Normal comtém diretrizes para identificação e correção de determinadas falhas estruturais encontradas no esquema do banco de dados.

Após execução do processo de normalização, a estrutura do banco de dados, assim como suas informações, estarão livres de inconsistências. As Seções abaixo dedicam-se a explicar em detalhes o funcionamento de cada uma das Formas Normais.

## 1FN: Primeira Forma Normal

A Primeira Forma Normal cuida da representação dos valores contidos em cada campo de uma entidade. Uma entidade está na 1FN se, somente se, todos os seus campos forem atômicos. Ou seja, não possuir mais de um valor para o mesmo campo.

Após identificar as entidades que comtém campos que **não** são atômicos, devemos aplicar os seguintes passos:

1. Identificação da chave primária da entidade.
2. Identificar e remover a coluna não atômica.
3. Criar uma nova entidade que irá armazenar, separadamente, os dados da coluna não atômica.
4. Criar uma relação entre a entidade principal e a nova entidade gerada.

### Exemplo

| id | nome  | email           | tarefas                    |
|----|-------|-----------------|----------------------------|
| 01 | Joao  | joao@joao.com   | Fazer compras, lavar carro |
| 02 | Pedro | pedro@pedro.com | ligar para casa            |
| 03 | José  | jose@jose.com   | Preparar para viagem       |

Como podemos perceber, a tabela acima viola a 1FN. Mais especificamente, a coluna `tarefas` não é atômica, uma vez que ela possui todas as tarefas de cada usuário. Nesse caso, é preciso criar uma nova tabela que irá armazenar apenas as tarefas, e então relacioná-la com seus usuários. As tabelas abaixo mostramo resultado após a aplicação da 1FN.

| id | nome  | email           |
|----|-------|-----------------|
| 01 | Joao  | joao@joao.com   |
| 02 | Pedro | pedro@pedro.com |
| 03 | José  | jose@jose.com   |

| id | id_usuario | descricao            |
|----|------------|----------------------|
| 01 | 01         | Fazer Compras        |
| 02 | 01         | lavar carro          |
| 03 | 02         | ligar para casa      |
| 04 | 03         | Preparar para viagem |


## Dependência Funcional

Para entendermos melhor o propósito das duas Formas Normais a seguir, é preciso conhecer o que é uma  *dependência fucional*. Basicamente, a dependência funcional representa a relação de dependência que existe entre duas colunas da mesma tabela. Isto é, o valor da coluna de uma tabela é **determinado diretamente** pelo valor de outra coluna dessa mesma tabela.

| id | nome  | email           |
|----|-------|-----------------|
| 01 | Joao  | joao@joao.com   |
| 02 | Pedro | pedro@pedro.com |
| 03 | José  | jose@jose.com   |

Tomando como exemplo a tabela de usuários descrita acima, dizemos que há uma dependência funcional entre as colunas `nome` e `id`, uma vez que `id` determina diretamente o valor de `nome`. Em outras palavras, `nome` depende funcionalmente de `id`.

## 2FN: Segunda Forma Normal

A Segunda Forma Normal verifica a dependência que existe entre as colunas **chaves** e as colunas normais. Por padrão, toda coluna normal (não chave) deve depender diretamente da chave primária de sua tabela. Quando tal condição **não** é satisfeita, dizemos que existe uma **dependência parcial**.

Uma entidade está na 2FN se, somente se, estiver na 1FN e **não** possuir dependências parciais. Quando houver uma violação da 2FN, devemos aplicar o seguinte procedimento:

1. Identificar as colunas que possuem dependências parciais na tabela.
2. Remover a coluna da tabela para uma nova, junto da chave primária, se necessário.

Considere como exemplo a descrição textual da tabela tarefas, descrita abaixo:

```
tarefas(id_usuario, id_tarefa, descricao, data_limite, nome, email)
```

A tabela acima possui uma chave primária composta por duas colunas: `id_usuario` e `id_tarefa`. Como podemos observar, as colunas normais possuem dependências parciais com suas chaves:

* `descricao` e `data_limite` dependem de `id_tarefa`.
* `nome` e `email` dependem de `id_usuario`.

Neste caso, a tabela `tarefas` pode ser divididas em duas tabelas distintas:

```
/* tabela tarefas remodelada */
tarefas(id_tarefa, descricao, data_limite)

/* nova tabela usuarios */
usuarios(id_usuario, nome, email)
```

## 3FN: Terceira Forma Normal

A Terceira Forma Normal cuida de outro tipo de dependência: a **dependência transitiva**. Basicamente, essa dependência verifica se o valor de uma coluna normal depende do valor de outra coluna normal.

Uma entidade está na 3FN se, somente se, estiver na 2FN e **não** possuir dependências transitivas. Caso haja a violação dessa regra, é necessário aplicar os seguintes passos:

1. Identificar a as colunas que possuem essa dependência.
2. Remover as colunas para uma nova tabela.
3. Relacionar a nova tabela com a antiga, se for o caso.

Considere como exemplo a descrição textual da tabela funcionário, descrita abaixo:

```
funcionarios(id, nome, funcao, salario)
```

Como sabemos, o valor do salário de um funcionário não depende de si, mas sim da função que ele exerce. Em outras palavras, existe uma dependência transtivia entre `funcao` e `salario`.

Para eliminar essa dependência, devemos remover o salário para uma nova tabela `funcoes`, e então relacioná-la com `funcionarios`. O trecho de código abaixo mostra como ficou a transformação da tabela `funcionarios` após normaliza-lá na 3FN.

```
/* tabela funcionarios remodelada */
funcionarios(id, nome, id_funcao)

/* nova tabela para descrever as funções */
funcoes(id, nome, salario)
```
