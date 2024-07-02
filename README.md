# Momento & Seus Funcionários

## O que vamos fazer? 🎲

- Inclua suas próprias informações no departamento de tecnologia da empresa

```
INSERT INTO funcionarios(funcionario_id,primeiro_nome,sobrenome,email,senha,telefone,data_contratacao,cargo_id,salario,gerente_id,departamento_id) 
VALUES (208,'Vitor','Hugo da Silva','vitor_hsilva@momento.org','123456','11987492156','2024-06-30',9,24000.00,NULL,13);
```

- Agora diga, quantos funcionários temos ao total na empresa?

```
SELECT COUNT(*) from momento.funcionarios;
```

- Quantos funcionários temos no departamento de finanças?

```
SELECT COUNT(*) from momento.funcionarios WHERE departamento_id = 10;
```

- Qual a média salarial do departamento de tecnologia?

```
SELECT SUM(salario) FROM funcionarios WHERE departamento_id = 5;
```

- Um novo departamento foi criado. O departamento de inovações. 
Ele será locado no Brasil. Por favor, adicione-o no banco de dados.

```
INSERT INTO departamentos (departamento_id, departamento_nome, escritorio_id)
VALUES (14, 'Inovações',  (SELECT escritorio_id FROM escritorios WHERE pais_id = 'BR'));
```

- Três novos funcionários foram contratados para o departamento de inovações. 
Por favor, adicione-os:
 - William Ferreira, casado com Inara Ferreira, 
possuem uma filha chamada Maria Antônia que tem 1 anos e adora brincar com cachorros. 
Ele será programador.
 - Já a Fernanda Lima, que é casada com o Rodrigo, não possui filhos. Ela vai ocupar a posição de desenvolvedora.  
 - Por último, a Gerente do departamento será Sumaia Azevedo. Casada, duas filhas (Tainã e Nathalia).
O salário de todos eles será a média salarial dos departamentos de administração e finanças.

```
SET @avg_salario = (SELECT AVG(salario) FROM funcionarios WHERE departamento_id = 1);

INSERT INTO funcionarios (funcionario_id, primeiro_nome, sobrenome, email, senha, telefone, data_contratacao, cargo_id, salario, gerente_id, departamento_id)
VALUES 
(NULL, 'William', 'Ferreira', 'william.ferreira@momento.com', 'password123', '1234567890', '2022-01-01', 9, @avg_salario, NULL, 14),
(NULL, 'Fernanda', 'Lima', 'fernanda.lima@momento.com', 'password123', '1234567890', '2022-01-01', 9, @avg_salario, NULL, 14),
(NULL, 'Sumaia', 'Azevedo', 'umaia.azevedo@momento.com', 'password123', '1234567890', '2022-01-01', 14, @avg_salario, NULL, 14);

INSERT INTO dependentes (dependente_id, primeiro_nome, sobrenome, relacionamento, funcionario_id)
VALUES 
(NULL, 'Inara', 'Ferreira', 'Cônjuge', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'William' AND sobrenome = 'Ferreira')),
(NULL, 'Maria Antônia', 'Ferreira', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'William' AND sobrenome = 'Ferreira')),
(NULL, 'Rodrigo', 'Lima', 'Cônjuge', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Fernanda' AND sobrenome = 'Lima')),
(NULL, 'Tainã', 'Azevedo', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Sumaia' AND sobrenome = 'Azevedo')),
(NULL, 'Nathalia', 'Azevedo', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Sumaia' AND sobrenome = 'Azevedo'));

```

- Informe todas as regiões em que a empresa atua acompanhadas de seus países.

```
SELECT r.regiao_nome, p.pais_nome
FROM regioes r
JOIN paises p ON r.regiao_id = p.regiao_id
ORDER BY r.regiao_nome, p.pais_nome;

```

- Joe Sciarra é filho de quem?

```
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id WHERE d.primeiro_nome = 'Joe' AND d.sobrenome = 'Sciarra';
```

- Jose Manuel possui filhos?

```
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id WHERE f.primeiro_nome = 'Jose Manuel';
```

- Qual região possui mais países?

```
SELECT r.regiao_nome, COUNT(p.pais_nome) AS num_paises
FROM regioes r
JOIN paises p ON r.regiao_id = p.regiao_id
GROUP BY r.regiao_nome
ORDER BY num_paises DESC LIMIT 1;

```

- Exiba o nome cada funcionário acompanhado de seus dependentes.

```
SELECT f.primeiro_nome AS nome_responsavel, f.sobrenome AS sobrenome_responsavel, d.primeiro_nome AS nome_dependente, d.sobrenome AS sobrenome_dependente, d.relacionamento
FROM funcionarios f
INNER JOIN dependentes d ON f.funcionario_id = d.funcionario_id;
```

- Karen Partners possui um(a) cônjuge?

```
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id WHERE d.relacionamento = 'Cônjuge' AND f.primeiro_nome = "Karen" AND f.sobrenome = "Partners";
```



