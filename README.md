# 1) Projeto-Logico-Oficina-DIO
Projeto Logico de um oficina fictício, com Script de criação, Inserts e Querys, todos testados no PostgreSQL versão 14. 
Esse esquema de banco de dados modela a situação de uma oficina mecânica ou oficina de serviços automotivos, onde são registrados diversos processos relacionados aos clientes, veículos e serviços realizados

OBS: A tabela Condenacao_de_Servico é destinada a registrar as condenações relacionadas a serviços prestados, ou seja, a informação de um serviço específico que foi executado e o valor associado a essa execução, além de seu status e data de entrada.

---
## 2) Como Utilizar
1. Tenha instalado algum banco de dados relacional
2. Crie um Banco de Dados 
3. Acesse a Query Tool e coloque o Script para a criação das tabelas **(Terceiro Tópico)**
4. Depois Popule as tabelas com as instâncias do **Quarto tópico**
5. Depois Rode as Queries do **Quinto Tópico** uma por vez, além de outras mais que queira para explorar a schema.

## 3) Script SQL

~~~SQL

CREATE TABLE Cliente (
    Id_cliente INT PRIMARY KEY,
    Nome VARCHAR(100),
    Endereco VARCHAR(255)
);

CREATE TABLE Servico (
    Id_servico INT PRIMARY KEY,
    Especialidade VARCHAR(50),
    Descricao TEXT
);

CREATE TABLE Veiculo (
    Id_veiculo INT PRIMARY KEY,
    Ano INT,
    Modelo VARCHAR(50),
    Id_cliente INT,
    FOREIGN KEY (Id_cliente) REFERENCES Cliente(Id_cliente)
);

CREATE TABLE Equipe (
    Id_equipe INT PRIMARY KEY,
    Nome VARCHAR(100)
);

CREATE TABLE Ordem_de_Servico (
    Id_OS INT PRIMARY KEY,
    Data_emissao DATE,
    Id_equipe INT,
    Id_veiculo INT,
    FOREIGN KEY (Id_equipe) REFERENCES Equipe(Id_equipe),
    FOREIGN KEY (Id_veiculo) REFERENCES Veiculo(Id_veiculo)
);

CREATE TABLE Condenacao_de_Servico (
    Id_condicao_servico INT PRIMARY KEY,
    Valor DECIMAL(10, 2),
    Data_entrada DATE,
    Status VARCHAR(50),
    Id_OS INT,
    Id_servico INT,
    FOREIGN KEY (Id_OS) REFERENCES Ordem_de_Servico(Id_OS),
    FOREIGN KEY (Id_servico) REFERENCES Servico(Id_servico)
);



CREATE TABLE Mecanico (
    Id_mecanico INT PRIMARY KEY,
    Nome VARCHAR(100),
    Endereco VARCHAR(255)
);

CREATE TABLE Ordem_de_Servico_Mecanico (
    Id_OS INT,
    Id_mecanico INT,
    PRIMARY KEY (Id_OS, Id_mecanico),
    FOREIGN KEY (Id_OS) REFERENCES Ordem_de_Servico(Id_OS),
    FOREIGN KEY (Id_mecanico) REFERENCES Mecanico(Id_mecanico)
);

~~~

---

## 4) Inserts

~~~SQL

-- Inserindo dados na tabela Cliente
INSERT INTO Cliente (Id_cliente, Nome, Endereco) VALUES
(1, 'João Silva', 'Rua A, 123, São Paulo - SP'),
(2, 'Maria Oliveira', 'Avenida B, 456, Rio de Janeiro - RJ'),
(3, 'Carlos Souza', 'Rua C, 789, Belo Horizonte - MG'),
(4, 'Ana Pereira', 'Rua D, 101, Porto Alegre - RS'),
(5, 'Felipe Costa', 'Avenida E, 202, Salvador - BA');

-- Inserindo dados na tabela Serviço
INSERT INTO Servico (Id_servico, Especialidade, Descricao) VALUES
(1, 'Revisão', 'Revisão de motor e sistema de câmbio'),
(2, 'Pintura', 'Pintura de carroceria com acabamento premium'),
(3, 'Troca de óleo', 'Troca de óleo e filtros'),
(4, 'Suspensão', 'Verificação e substituição de peças da suspensão'),
(5, 'Alinhamento', 'Alinhamento de direção e balanceamento de rodas');

-- Inserindo dados na tabela Equipe
INSERT INTO Equipe (Id_equipe, Nome) VALUES
(1, 'Equipe A'),
(2, 'Equipe B'),
(3, 'Equipe C');


-- Inserindo dados na tabela Veiculo
INSERT INTO Veiculo (Id_veiculo, Ano, Modelo, Id_cliente) VALUES
(1, 2018, 'Fusca', 1),
(2, 2020, 'Civic', 2),
(3, 2021, 'Corolla', 3),
(4, 2019, 'Fiat Uno', 4),
(5, 2022, 'Onix', 5);


-- Inserindo dados na tabela Ordem_de_Servico
INSERT INTO Ordem_de_Servico (Id_OS, Data_emissao, Id_equipe, Id_veiculo) VALUES
(1, '2023-05-15', 1, 1),
(2, '2023-06-20', 2, 2),
(3, '2023-07-10', 3, 3),
(4, '2023-08-05', 1, 4),
(5, '2023-09-12', 2, 5);




-- Inserindo dados na tabela Condenacao_de_Servico
INSERT INTO Condenacao_de_Servico (Id_condicao_servico, Valor, Data_entrada, Status, Id_OS, Id_servico) VALUES
(1, 200.00, '2023-05-16', 'Pendente', 1, 1),
(2, 150.00, '2023-06-21', 'Concluído', 2, 2),
(3, 100.00, '2023-07-11', 'Pendente', 3, 3),
(4, 300.00, '2023-08-06', 'Concluído', 4, 4),
(5, 120.00, '2023-09-13', 'Pendente', 5, 5);

-- Inserindo dados na tabela Mecânico
INSERT INTO mecanico (Id_mecanico, Nome, Endereco) VALUES
(1, 'Roberto Alves', 'Rua F, 305, São Paulo - SP'),
(2, 'Cláudia Lima', 'Avenida G, 408, Rio de Janeiro - RJ'),
(3, 'Lucas Costa', 'Rua H, 509, Belo Horizonte - MG'),
(4, 'Fernanda Ribeiro', 'Avenida I, 612, Porto Alegre - RS'),
(5, 'Rafael Souza', 'Rua J, 713, Salvador - BA');

-- Inserindo dados na tabela Ordem_de_Servico_Mecanico
INSERT INTO Ordem_de_Servico_Mecanico (Id_OS, Id_mecanico) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(1, 2),
(2, 3),
(3, 4),
(4, 5),
(5, 1);

~~~

---

## 3) Queries

~~~SQL

-- Recupera todos os registros da tabela Cliente
SELECT * FROM Cliente;

-- Seleciona os clientes de São Paulo que possuem veículos com ano de fabricação superior a 2018
SELECT Nome, Endereco FROM Cliente
WHERE Endereco LIKE '%Porto Alegre%' AND cliente.id_cliente IN (
    SELECT Id_cliente FROM Veiculo WHERE Ano > 2018
);

-- Recupera os veículos ordenados pelo ano de fabricação de forma decrescente
SELECT Modelo, Ano FROM Veiculo
ORDER BY Ano DESC;


-- Faz uma junção entre as tabelas Ordem_de_Serviço, Veiculo, Equipe e Cliente para retornar o nome da equipe, o modelo do veículo e a data de emissão da ordem de serviço
SELECT E.Nome AS Equipe, V.Modelo AS Veiculo, OS.Data_emissao
FROM Ordem_de_Servico OS
JOIN Veiculo V ON OS.Id_veiculo = V.Id_veiculo
JOIN Equipe E ON OS.Id_equipe = E.Id_equipe
JOIN Cliente C ON V.Id_cliente = C.Id_cliente
ORDER BY OS.Data_emissao DESC;

-- Seleciona todos os clientes e conta quantas ordens de serviço cada cliente possui
SELECT C.Nome, COUNT(OS.Id_OS) AS Ordens_de_Servico
FROM Cliente C
LEFT JOIN Veiculo V ON C.Id_cliente = V.Id_cliente
LEFT JOIN Ordem_de_Servico OS ON V.Id_veiculo = OS.Id_veiculo
GROUP BY C.Id_cliente
ORDER BY Ordens_de_Servico DESC;

-- Seleciona todas as ordens de serviço que têm um valor de condenação superior a 100, junto com o nome do mecânico e do cliente
SELECT C.Nome AS Cliente, OS.Id_OS, M.Nome AS Mecanico, CS.Valor AS Valor_Condenacao
FROM Ordem_de_Servico OS
JOIN Veiculo V ON OS.Id_veiculo = V.Id_veiculo
JOIN Cliente C ON V.Id_cliente = C.Id_cliente
JOIN Condenacao_de_Servico CS ON OS.Id_OS = CS.Id_OS
JOIN Ordem_de_Servico_Mecanico OSM ON OS.Id_OS = OSM.Id_OS
JOIN Mecanico M ON OSM.Id_mecanico = M.Id_mecanico
WHERE CS.Valor > 100
ORDER BY CS.Valor DESC;

-- Calcula o valor total das condenações de serviço por ordem de serviço e filtra para exibir apenas aquelas onde o valor total é maior que 200
SELECT OS.Id_OS, SUM(CS.Valor) AS Total_Condenacao
FROM Ordem_de_Servico OS
JOIN Condenacao_de_Servico CS ON OS.Id_OS = CS.Id_OS
GROUP BY OS.Id_OS
HAVING SUM(CS.Valor) > 200;

-- Combina várias tabelas para fornecer uma visão completa das ordens de serviço, com dados sobre o cliente, equipe, veículo, serviço e mecânico
SELECT C.Nome AS Cliente, E.Nome AS Equipe, V.Modelo AS Veiculo, S.Especialidade AS Servico, M.Nome AS Mecanico, OS.Data_emissao
FROM Ordem_de_Servico OS
JOIN Veiculo V ON OS.Id_veiculo = V.Id_veiculo
JOIN Cliente C ON V.Id_cliente = C.Id_cliente
JOIN Equipe E ON OS.Id_equipe = E.Id_equipe
JOIN Condenacao_de_Servico CS ON OS.Id_OS = CS.Id_OS
JOIN Servico S ON CS.Id_servico = S.Id_servico
JOIN Ordem_de_Servico_Mecanico OSM ON OS.Id_OS = OSM.Id_OS
JOIN Mecanico M ON OSM.Id_mecanico = M.Id_mecanico
ORDER BY OS.Data_emissao DESC;

~~~

---

