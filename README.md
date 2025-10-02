Projeto Conceitual – Oficina Mecânica

1️⃣ Objetivo

Criar o esquema conceitual de um sistema de controle e gerenciamento de ordens de serviço em uma oficina mecânica, permitindo registrar informações de clientes, veículos, mecânicos, serviços executados e cálculo de valores com base em mão-de-obra e peças.

2️⃣ Narrativa do Sistema

Clientes levam veículos à oficina para consertos ou revisões periódicas.

Cada veículo é designado a uma equipe de mecânicos.

A equipe identifica os serviços necessários e preenche uma Ordem de Serviço (OS), informando a data de entrega.

O valor de cada serviço é calculado com base na tabela de referência de mão-de-obra.

O valor das peças também compõe o valor final da OS.

O cliente autoriza a execução dos serviços.

A mesma equipe avalia e executa os serviços.

Cada OS possui: número, data de emissão, valor total, status e data prevista de conclusão.

Mecânicos possuem: código, nome, endereço e especialidade.

3️⃣ Entidades e Atributos

Entidade	Atributos Principais	Observações
Cliente	id_cliente (PK), nome, endereço, telefone, email	Cada cliente pode ter múltiplos veículos
Veículo	id_veiculo (PK), placa, modelo, ano, id_cliente (FK)	Cada veículo pertence a um cliente
Mecânico	id_mecanico (PK), nome, endereço, especialidade	Cada mecânico pertence a uma equipe
Equipe	id_equipe (PK), nome	Grupo de mecânicos que trabalha junto
OrdemServico	nro_os (PK), id_veiculo (FK), id_equipe (FK), data_emissao, data_prevista, valor_total, status	Representa cada OS emitida
Servico	id_servico (PK), descricao, valor_mao_obra	Tabela de referência para cálculo
Peca	id_peca (PK), descricao, valor	Valor das peças utilizadas na OS
ExecucaoOS	nro_os (FK), id_servico (FK), id_peca (FK), quantidade, valor_total_item	Detalhamento dos serviços e peças usados

4️⃣ Relacionamentos

Cliente → Veículo: 1:N

Veículo → OrdemServico: 1:N

Equipe → OrdemServico: 1:N

Mecânico → Equipe: N:M

OrdemServico → ExecucaoOS → Servico/Peca: N:M

Cada OS deve ter pelo menos um serviço registrado antes da execução.

5️⃣ Regras de Negócio

Cada veículo pertence a apenas um cliente.

Cada OS deve ser atribuída a uma única equipe.

O valor da OS é calculado somando o valor de mão-de-obra e peças utilizadas.

O status da OS deve indicar progresso: 'Aberta', 'Em Execução', 'Aguardando Aprovação', 'Concluída'.

Cada mecânico pode estar em mais de uma equipe se necessário.

O cliente deve autorizar a execução antes da conclusão da OS.

6️⃣ Exemplo de Estrutura em SQL

-- Tabela Cliente
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nome VARCHAR(255),
    endereco VARCHAR(255),
    telefone VARCHAR(20),
    email VARCHAR(100)
);

-- Tabela Veículo
CREATE TABLE Veiculo (
    id_veiculo INT PRIMARY KEY,
    placa VARCHAR(10),
    modelo VARCHAR(100),
    ano INT,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);

-- Tabela Mecânico
CREATE TABLE Mecanico (
    id_mecanico INT PRIMARY KEY,
    nome VARCHAR(255),
    endereco VARCHAR(255),
    especialidade VARCHAR(100)
);

-- Tabela Equipe
CREATE TABLE Equipe (
    id_equipe INT PRIMARY KEY,
    nome VARCHAR(100)
);

-- Tabela Ordem de Serviço
CREATE TABLE OrdemServico (
    nro_os INT PRIMARY KEY,
    id_veiculo INT,
    id_equipe INT,
    data_emissao DATE,
    data_prevista DATE,
    valor_total DECIMAL(10,2),
    status VARCHAR(50),
    FOREIGN KEY (id_veiculo) REFERENCES Veiculo(id_veiculo),
    FOREIGN KEY (id_equipe) REFERENCES Equipe(id_equipe)
);

-- Tabela Serviço
CREATE TABLE Servico (
    id_servico INT PRIMARY KEY,
    descricao VARCHAR(255),
    valor_mao_obra DECIMAL(10,2)
);

-- Tabela Peça
CREATE TABLE Peca (
    id_peca INT PRIMARY KEY,
    descricao VARCHAR(255),
    valor DECIMAL(10,2)
);

-- Tabela Execução da OS
CREATE TABLE ExecucaoOS (
    nro_os INT,
    id_servico INT,
    id_peca INT,
    quantidade INT,
    valor_total_item DECIMAL(10,2),
    PRIMARY KEY (nro_os, id_servico, id_peca),
    FOREIGN KEY (nro_os) REFERENCES OrdemServico(nro_os),
    FOREIGN KEY (id_servico) REFERENCES Servico(id_servico),
    FOREIGN KEY (id_peca) REFERENCES Peca(id_peca)
);
