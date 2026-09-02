### ANALISE DE DADOS
**aula06**
para sabermos se o comando exite iremos usar o comando:
```sql

DROP DATABASE IF EXISTS 

```
---

para criar um tabela:
```sql
CREATE TABLE produtos(
    id SERIAL PRIMARY KEY,
    nome varchar(100) NOT NULL,
    categoria VARCHAR(50) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    estoque INTEGER NOT NULL
);
```
---
para verificar a quantidade de linhas da nossa base de dados
```sql
SELECT COUNT(*) FROM produtos;
```
---

filtro de colunas e limitacao de saida:
```SQL
SELECT nome,preco FROM produtos LIMIT 10;
```
---para verificar a quantidade e tipos diferentes de categoria:
```sql 
SELECT DISTINCT categoria FROM produtos;
```
---

filtro de colunas e categorias:
```sql
SELECT nome,preco,estoque 
FROM produtos
WHERE categoria = 'Monitores';
```
---
filtrando produtos que custam mais de R$1000
```sql
SELECT nome,preco
FROM produtos 
WHERE preco >= 1000;
```
---
ultilizando o comando or:
```sql
SELECT nome,preco
FROM produtos
WHERE categoria = 'Monitores' or categoria = 'Notebooks';
```
---
para acharmos um prouto especifico usaremos o comando:
```sql
SELECT nome,preco
FROM produtos
WHERE nome = 'Monitor Gamer 144Hz';
```
---
>% Operador coringa!



## atividade

vamos criar a a biblioteca primeiro, iremos ao moba e usar o comando:
```sql
CREATE DATABASE biblioteca;

![alt text](image.png)
```
---
agora vamos voltar ao vscode e criaremos a biblioteca:
```sql 
![alt text](image-1.png)
```
---
agora iremos criar a tabela com o os seguintes comando:
```sql 
CREATE TABLE biblioteca(
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    autor VARCHAR(150) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    genero VARCHAR(50) NOT NULL,
    estoque INT NOT NULL DEFAULT 0,
    ano_publicacao INT NOT NULL
);

![alt text](image-2.png)

![alt text](image-3.png)
```

1. Exiba todos os dados da tabela, mas limitando o resultado aos 10 primeiros registros:
```SQL
SELECT * FROM biblioteca LIMIT 10;
```
![alt text](image-4.png)

---
2. Exiba apenas as colunas titulo, autor e preco de todos os livros:
```SQL
SELECT nome, autor, preco FROM biblioteca;
```
![alt text](image-5.png)

---
3. Liste os gêneros distintos existentes na base, em ordem alfabética:
```SQL
SELECT DISTINCT genero FROM BIBLIOTECA ORDER BY genero ASC;
```
![alt text](image-6.png)


4. Descubra quantos autores diferentes existem:
```SQL
SELECT COUNT(DISTINCT autor) AS total_autores FROM biblioteca;
```
![alt text](image-7.png)
---
5. Liste os 5 livros mais caros da base (título e preço):
```sql 
SELECT nome, preco FROM biblioteca ORDER BY preco DESC LIMIT 5;
```
![alt text](image-8.png)

---
6. Liste os 5 livros com menor estoque (título e estoque):
```sql
SELECT nome, estoque FROM biblioteca ORDER BY estoque ASC LIMIT 5;
```
![alt text](image-9.png)

---
 ## bloco 2 
 
Objetivo: dominar os operadores de comparação e o BETWEEN.

7. Mostre titulo e estoque de todos os livros do gênero Técnico:
```sql

SELECT nome, estoque FROM biblioteca ORDER BY estoque ASC LIMIT 5;
```
![alt text](image-10.png)

8. Mostre titulo e preco dos livros que custam mais de R$ 200,00:
```sql

SELECT nome, preco FROM biblioteca WHERE preco > 200.00;

```
![alt text](image-12.png)

9. Mostre titulo e preco dos livros com preço entre R$ 40,00 e R$ 70,00:
```sql

SELECT nome, preco FROM biblioteca WHERE preco BETWEEN 40.00 AND 70.00;

```
![alt text](image-13.png)


10. Mostre os livros com estoque abaixo de 5 unidades (situação de reposição urgente):
```sql

SELECT nome, estoque FROM biblioteca WHERE estoque < 5;

```
![alt text](image-14.png)

11. Liste os livros publicados antes de 1900, ordenados do mais antigo para o mais recente:
```sql

SELECT nome, ano_publicacao FROM biblioteca WHERE ano_publicacao < 1900 ORDER BY ano_publicacao ASC;

```
![alt text](image-15.png)

12. Liste os livros publicados entre 2010 e 2020, mostrando título, ano e gênero:
```sql

SELECT nome, ano_publicacao, genero FROM biblioteca WHERE ano_publicacao BETWEEN 2010 AND 2020;

```
![alt text](image-16.png)


