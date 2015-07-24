#A Linguagem SQL

Todo SGBD requer uma linguagem específica para consulta e manipulação de seus dados. No intuito de desenvolver uma linguagem onde a manipulação dos dados fosse direta e natural, a IBM desenvolveu -- em meados dos anos 70 -- a *Strucutured Query Language*, conhecida como SQL.

Projetada para atender um público das áreas administrativa e econômica, a linguagem possui uma sintaxe simples e declarativa. Tal sintaxe permite que tanto funcionários especializados, quanto leigos na área de tecnologia se utilizem da linguagem para recuperar e analisar os dados persistidos no SGBD de forma direta.

As principais características do SQL são:

* Sintaxe dos comandos o mais próximo possível da língua natural inglesa
* Não procedimental: indica-se a informação que se pretende obter sem qualquer preocupação em "como se vai obter".
* Trabalha com conjuntos de registros. Não existem comandos como "Next record" ou "Previous record" (comuns nas linguagens de acesso a dados anteriores a 1970)
* É utilizada tanto pelos usuários normais quanto pelo DBA (Database Administrator)

O trecho de código abaixo mostra um exemplo de comando SQL, utilizado para fazer consultas em eum banco de dados.

```
SELECT nome, CPF, cidade
FROM usuarios
WHERE cidade = 'Belo Horizonte';
```

Como pode-se observar, a sintaxe básica da linguagem é extremamente simples. Os comandos podem ser multi-linhas, no entanto devem sempre terminar com ponto-e-vírgula.

Ainda, o SQL **não** é *case sensitive*: a linguagem não distingue letras maiúsculas e minúsculas. No entanto, padroniza-se na indústria a utilização de CAIXA ALTA toda vez que o comando SQL se referir a uma de suas palavras reservadas, e caixa baixa caso contrário.

O SQL permite aos seus usuários implementar diversas ações no SGBD, desde de a criação/manipulação da estrutura do banco de dados até o controle/auditoria dos dados presentes no banco de dados.

##Os Comandos SQL

Os comandos SQL são divididos em três categorias principais:

 * **DML - Data Manipulation Language:** Instruções que trabalham com os dados (linhas, tuplas, registros) propriamente ditos, permitindo buscá-los e alterá-los.
 * **DDL - Data Definition Language** Instruções que trabalham com objetos do banco de dados, permitindo definí-los, removê-los.
 * **DCL - Data Control Language** Instruções que trabalham com usuários e controle de acesso, adicionando ou removendo permissões de usuários sobre tabelas e bancos de dados.

 A Tabela abaixo mostra os principais comandos existentes no SQL, junto de suas respectivas categorias:

 | Comando | Descrição                                          | Categoria |
 |---------|----------------------------------------------------|-----------|
 | SELECT  | Consulta de dados existetnes                       | DML       |
 | INSERT  | Insere novos dados ao Banco de Dados               | DML       |
 | UPDATE  | Altera os dados existentes do Banco de Dados       | DML       |
 | DELETE  | Apaga dados existentes no Banco de Dados           | DML       |
 | CREATE  | Criação de novos objetos no Banco de Dados         | DDL       |
 | ALTER   | Alteração dos objetos existentes no Banco de Dados | DDL       |
 | DROP    | Exclusão de objetos existentes no Banco de Dados   | DDL       |
 | GRANT   | Conceder acesso a objetos no Banco de Dados        | DCL       |
 | REVOKE  | Retirar acesso a objetos no Banco de Dados         | DCL       |
