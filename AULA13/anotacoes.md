## Normalização de banco de dados 
2 Objetivo: Normalizar um banco de dados com problemas de organização de registros.

1.primeiro, criamos um banco de dados:
```sql
CREATE DATABASE empresa_hdz (
    CREATE TABLE cadastro_original 
    funcionario_id INTEGER PRIMARY KEY,
    nome TEXT,
    setor_id INTEGER,
    setor_nome TEXT,
    treinamentos TEXT
);

```
2 inserimos os dados na tabela:
```sql
INSERT INTO cadastro_original VALUES 
(1,'victor',10,'montagem','seguranca;ferramentas'),
(2,'miguel',20,'manutencao','seguranca'),
(3,'lucas',10,'montagem','ferramentas');
```

3.ao realizar a primeira consulta o victor estao com o mesmo erros na linha




4. criamos uma nova tabela:
```sql
CREATE TABLE cadastro_1fn AS 
SELECT funcionario_id,nome,setor_id,setor_nome,
    string_to_table(treinamentos, ';') AS treinamento
FROM cadastro_original;
```

5. criamos novamente e modificamos ela para ajustar o erros:
```sql
 ALTER TABLE cadastro_1fn
 ADD PRIMARY KEY
(funcionario_id,
treinamento);
```
### 2FN
1. primeiro criamos a tabela de funcionarios:
```sql
CREATE TABLE funcionarios AS 
SELECT DISTINCT funcionario_id,nome,setor_id,setor_nome
FROM cadastro_1fn;
```
> O DISTINCT evita copiar linhas iguais mais de uma vez. ou seja: os dois treinamentos do victor 

2. o comando CREATE TABLE AS  não tars uma chaves primarias automaticamente sosinha:
```sql
ALTER TABLE funcionarios
ADD PRIMARY KEY
(funcionario_id);
```
3. criamos a tabela  participacoes, trazendo funcionarios e os seus treinamentos.
```sql
CREATE TABLE participacoes AS SELECT funcionario_id, treinamento FROM cadastro_1fn;
```

4. ateracao da tabela
```sql
ALTER TABLE participacoes 
ADD PRIMARY KEY
(funcionario_id,treinamento);
---
ALTER TABLE participacoes 
ADD FOREIGN KEY (funcionario_id) REFERENCES funcionarios (funcionario_id);
```

6. Verificar:
```sql
![alt text](image-1.png)
```
---

## FN3
1. criamos a tabela setores:
```sql
CREATE TABLE setores AS SELECT DISTINCT setor_id,setor_nome
FROM funcionarios;
```
2. adicionando a chave primaria:
```SQL
ALTER TABLE pg_stat_get_progress_info
ADD PRIMARY KEY 
(setor_id);
```
3.
```sql
ALTER TABLE funcionarios
ADD FOREIGN KEY 
(setor_id) REFERENCES setores(setor_id);
```



