

### Criar o banco de dados

```sql
CREATE DATABASE manutec_treinamentos;
```

![alt text](image-2.png)

### criar a tabela original e inserir os dados

```sql
CREATE TABLE cadastro_original (
    funcionario_id INTEGER PRIMARY KEY,
    nome TEXT,
    setor_id INTEGER,
    setor_nome TEXT,
    treinamentos TEXT
);

![alt text](image-3.png)

INSERT INTO cadastro_original VALUES
(11, 'Féboli', 1, 'Producao', 'Integracao;EPI'),
(12, 'Mayne', 2, 'Qualidade', 'EPI'),
(13, 'Yasmin', 1, 'Producao', 'Integracao');
```
![alt text](image-4.png)

resultado:

```sql
SELECT * FROM cadastro_original;
```

Resultado de SELECT * FROM cadastro_original;

![alt text](image-5.png)

---

## 1. Primeira Forma Normal (1FN)

Objetivo: cada linha com um único treinamento, sem perder nenhuma participação. Chave composta: `(funcionario_id, treinamento)`.

### Criar `cadastro_1fn` separando o campo `treinamentos` no `;`

```sql
CREATE TABLE cadastro_1fn AS
SELECT
    funcionario_id,
    nome,
    setor_id,
    setor_nome,
    TRIM(t.treinamento) AS treinamento
FROM cadastro_original,
     UNNEST(string_to_array(treinamentos, ';')) AS t(treinamento);
```

### Definir a chave primária composta

```sql
ALTER TABLE cadastro_1fn
    ADD PRIMARY KEY (funcionario_id, treinamento);
```

### Consultar o resultado

```sql
SELECT * FROM cadastro_1fn ORDER BY funcionario_id, treinamento;
```

 Criação de cadastro_1fn e definição da chave primária

![alt text](image-6.png)

Resultado de SELECT * FROM cadastro_1fn;

![alt text](image-7.png)

### Verificação 

```sql
SELECT COUNT(*) AS total_participacoes FROM cadastro_1fn;
```

Verificar Contagem de participações 

![alt text](image-8.png)

---

## 2. Segunda Forma Normal (2FN)

Objetivo: eliminar dependências parciais. Os dados do funcionário (que dependem só de `funcionario_id`) ficam separados das participações.

### Criar funcionarios com DISTINCT

```sql
CREATE TABLE funcionarios AS
SELECT DISTINCT
    funcionario_id,
    nome,
    setor_id,
    setor_nome
FROM cadastro_1fn;
```

### Criar `participacoes`

```sql
CREATE TABLE participacoes AS
SELECT
    funcionario_id,
    treinamento
FROM cadastro_1fn;
```

### Definir chaves primárias e chave estrangeira

```sql
ALTER TABLE funcionarios
    ADD PRIMARY KEY (funcionario_id);

ALTER TABLE participacoes
    ADD PRIMARY KEY (funcionario_id, treinamento);

ALTER TABLE participacoes
    ADD CONSTRAINT fk_participacoes_funcionario
    FOREIGN KEY (funcionario_id)
    REFERENCES funcionarios (funcionario_id);
```

### Consultar as duas tabelas

```sql
SELECT * FROM funcionarios ORDER BY funcionario_id;

SELECT * FROM participacoes ORDER BY funcionario_id, treinamento;
```

Criação de `funcionarios` e `participacoes`

![alt text](image-9.png) ![alt text](image-10.png)

Definição das chaves primárias e da chave estrangeira
![alt text](image-11.png)

Resultado de `SELECT * FROM funcionarios;

![alt text](image-12.png)

Resultado de SELECT * FROM participacoes;

![alt text](image-13.png)

---

## 3. Terceira Forma Normal (3FN)

objetivo: eliminar dependência transitiva. `setor_nome` depende de `setor_id`, e não de `funcionario_id`, então vai para uma tabela própria.

### Criar `setores` (sem linhas repetidas)

```sql
CREATE TABLE setores AS
SELECT DISTINCT
    setor_id,
    setor_nome
FROM funcionarios;
```

### Definir a chave primária de setores

```sql
ALTER TABLE setores
    ADD PRIMARY KEY (setor_id);
```

### Consultar os setores antes de continuar

```sql
SELECT * FROM setores ORDER BY setor_id;
```

Criação de 'setores' e chave primária:

![alt text](image-14.png)

 Resultado de SELECT * FROM setores;

![alt text](image-15.png)

### Adicionar a chave estrangeira em funcionarios e exigir setor_id

```sql
ALTER TABLE funcionarios
    ADD CONSTRAINT fk_funcionarios_setor
    FOREIGN KEY (setor_id)
    REFERENCES setores (setor_id);

ALTER TABLE funcionarios
    ALTER COLUMN setor_id SET NOT NULL;
```

### Retirar somente a coluna setor_nome de funcionarios

```sql
ALTER TABLE funcionarios
    DROP COLUMN setor_nome;
```

Chave estrangeira, `NOT NULL` e remoção de `setor_nome`**

![alt text](image-16.png)

### Consultar as três tabelas finais

```sql
SELECT * FROM setores ORDER BY setor_id;

SELECT * FROM funcionarios ORDER BY funcionario_id;

SELECT * FROM participacoes ORDER BY funcionario_id, treinamento;
```

verificação do resultado: SELECT * FROM setores;

![alt text](image-17.png)

SELECT * FROM funcionarios;

![alt text](image-18.png)

SELECT * FROM participacoes;

![alt text](image-19.png)
