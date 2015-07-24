# Ordenando os registros de uma consulta

Em determinadas situações, o usuário precisa analisar os dados resultantes de uma consulta do banco de dados de acordo com uma determinada ordenação. Na linguagem SQL, essa ordenação é feita por meio da cláusula `ORDER BY`. A sintaxe da cláusula pode vista no trecho de código abaixo:

```
SELECT ...
ORDER BY campo1, campo2, campoN [DESC];
```

A ordenação é feita automaticamente de forma crescente, priorizando a ordenação com base na sequência com que os campos são declarados. Caso o usuário deseja ordenar os dados de forma decrescente, deve-se adicionar o parâmetro `DESC` ao final da declaração da cláusula. O trecho de código abaixo mosta um exemplo real de utilização da cláusula `ORDER BY`:

```
/* ordena os usuários por nome e e-mail. */
SELECT * FROM usuarios
ORDER BY nome, email;

/*
orderna as tarefas com base em seu código
e código da categoria de forma descrescente
*/
SELECT * FROM tarefas
WHERE usuarios_id BETWEEN 01 AND 10
ORDER BY id, categorias_id DESC;
```
