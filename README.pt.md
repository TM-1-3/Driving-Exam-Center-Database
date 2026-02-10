<img src='https://sigarra.up.pt/feup/pt/imagens/LogotipoSI' width="30%"/>

<div align="center">
🌍 <a href="README.md">Inglês</a> | 🇵🇹 <a href="README.pt.md">Português</a>
</div>

<h3 align="center">Licenciatura em Engenharia Informática e Computação<br> L.EIC012 - Bases de Dados<br> 2024/2025 </h3>

---
<h3 align="center"> Colaboradores &#129309 </h2>

<div align="center">

| Nome | Número |
|--------------------|-------------|
| Elton Vaz          | up202309925 |
| Henrique Vilarinho | up202307037 |
| Tomás Morais       | up202304692 |

Nota: 15,6

</div>

# Relatório da Base de Dados Relacional SQL do Driving Exam Center


Para ver o relatório completo clique aqui: <a href="reports/901_1ªSubmissão.pdf">Primeiro relatório</a>  &  <a href="reports/901_2ªSubmissão.pdf">Segundo Relatório</a>

- [Diagrama UML](#1) 
- [Esquema Relacional](#2)
- [Análise de Dependências Funcionais e Formas Normais](#3)
- [Criação e População da Base de Dados](#4)
  - [Script de criação SQL](#4.1)
  - [Script de população SQL](#4.2)

<a id="1"></a>
## Diagrama UML

<img width="1274" height="800" alt="Captura de ecrã de 2025-09-17 11-27-35" src="https://github.com/user-attachments/assets/f1ecd6e8-78d3-4aac-bebf-234f0eaf0896" />

<a id="2"></a>
## Esquema Relacional

Escola (<ins>IDEscola</ins>, Nome, Morada, CodigoPostal, AlvaraDeFuncionamento)

Instrutor(<ins>Instrutor de identificação</ins>,Nome,DataDeNascimento,NIF,NumeroCartaoCidadao, CodigoPostal, Morada, LicencaDeInstrucao,IDEscola-->Escola,IDCarta-->CartadeConducao)

Proprietário (<ins>Instrutor de identificação</ins>-->Instrutor, <ins>IDEscola</ins>-->Escola, Proprietária?)

Veiculo (<ins>ID do veículo</ins>, Matricula, Marca, NumeroKmPercorridos,DataDeInspecao, Seguro, IDEscola-->Escola, IDInstrutor-->Instrutor,IDCategoria-->Categoria)

Categoria (<ins>Categoria ID</ins>, Designação)

CategoriaCartão (<ins>IDCarta</ins>-->CartaDeConducao, <ins>Categoria ID</ins>-->Categoria)

CartaDeConducao (<ins>IDCarta</ins>, Número, DataDeEmissão)

Percurso (<ins>ID do caminho</ins>, Nome, Perimetro, PontoDeTroca)

Examinador(<ins>IDExaminador</ins>,Nome,DataDeNascimento,NIF,CartaoCidadao,CodigoPostal, Morada, CredencialDeExaminador,IDCarta-->CartaDeConducao)

Examinando (<ins>IDExaminando</ins>, Nome, DataDeNascimento, NIF,NumeroCartaoCidadao, CodigoPostal, Morada, LicencaDeAprendizagem,IDEscola-->Escola, IDInstrutor-->Instrutor)

Exame (<ins>IDExame</ins>, Data, Hora, IDExaminando--> Examinando,IDExaminador-->Examinador, IDPercurso-->Percurso)

Aprovacao (<ins>IDAprovacao</ins>,Duracao, Avaliacao, Ordem, <ins>IDExaminando</ins>-->Examinando, <ins>IDExame</ins>-->Exame,IDCarta-->CartaDeConducao)

Reprovacao (<ins>IDReprovação</ins>,Duracao, Motivo, Ordem, <ins>IDExaminando</ins>-->Examinando, <ins>IDExame</ins>-->Exame)

<a id="3"></a>
## Análise de Dependências Funcionais e Formas Normais

| Relation | Pessoa | Escola | Veículo | Percurso |
| :--- | :--- | :--- | :--- | :--- | 
| **Keys** | IDPessoa (PrimaryKey), CartaoDeCidadao, NIF | IDEscola (PrimaryKey), Nome, AlvaraFuncionamento | IDVeiculo (Primary Key), Matricula | **IDPercurso** (Primary Key), Nome | 
| **Functional Dependencies** | **IDPessoa** → Nome, DataDeNascimento, NIF, CartaoDeCidadao, CodigoPostal, Morada.<br>**CartaoDeCidadao** → Nome, DataDeNascimento, NIF, CodigoPostal, Morada.<br>**NIF** → Nome, DataDeNascimento, CartaoDeCidadao, CodigoPostal, Morada. | **IDEscola** → Nome, Morada, CodigoPostal, AlvaraFuncionamento.<br>**Nome** → Morada, CodigoPostal, AlvaraFuncionamento.<br>**AlvaraFuncionamento** → Nome. | **IDVeiculo** → Matricula, Marca, Seguro, KmPercorridos, DataInspecção.<br>**Matricula** → Marca, Seguro, DataInspecção | **IDPercurso** → Nome, Perimetro, PontoDeTroca.<br>**Nome** → Perimetro, PontoDeTroca. |
| **Justificação e decomposição** | Não há necessidade de decompor a classe Person porque ID, CartaodeCidado e NIF são superchaves desta classe. | Não há necessidade de decompor a classe Escola porque Name e AlvaraFuncionamento são superchaves. Cada autoescola só pode ter uma carteira de funcionamento, portanto é uma superchave da turma Escola. | Não há necessidade de decompor a classe Veiculo pois ID e Matricula são superchaves desta classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. |

| Relation | Exame | Instrutor | Examinador | Examinando |
| :--- | :--- | :--- | :--- | :--- |
| **Keys** | **IDExame** (Primary Key) | **IDInstrutor** (Primary Key), LicençaDeInstrucao | **IDExaminador** (Primary Key), CredencialDeExaminador | **IDExaminando** (Primary Key), LicençaDeAprendizagem |
| **Functional Dependencies** | **IDExame** → Data, Hora. | **IDInstrutor** → LicençaDeInstrucao | **IDExaminador** → CredencialDeExaminador | **IDExaminando** → LicençaDeAprendizagem |
| **Justificação e decomposição** | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. |

| Relation | Aprovação | Categoria | CartaDeConducao | Reprovacao |
| :--- | :--- | :--- | :--- | :--- |
| **Chaves** | **IDAprovacao** (Chave Primária) | **IDCategoria** (Chave Primária), Designação | **IDCarta** (Chave Primária), Número | **IDReprovação** (Chave Primária) |
| **Functional Dependencies** | **IDAprovacao** → Duracao, Avaliacao | **IDCategoria** → Designacao | **IDCarta** → Numero, DataDeEmissao | **IDReprovacao** → Duracao, Motivo |
| **Justificação e decomposição** | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. | Existe apenas uma Dependência Funcional com o elemento esquerdo sendo uma superchave. Portanto, não há necessidade de decompor a classe. |

<a id="4"></a>
## Criação e População da Base de Dados

<a id="4.1"></a>
### Script de criação SQL

O script SQL **create.sql** para criação da base de dados está disponível [aqui](https://github.com/TM-1-3/Driving-Exam-Center-Database/blob/master/create.sql).

```sql
PRAGMA foreign_keys=ON;

DROP TABLE IF EXISTS Categoria;

CREATE TABLE Categoria(
    IDCategoria INTEGER NOT NULL,
    Designacao VARCHAR(3) NOT NULL DEFAULT('B') CHECK(Designacao IN ('A','A1','A2','AM','B','B1','C','C1','D','D1','BE','C1E','CE','D1E','DE','T')),
    PRIMARY KEY(IDCategoria)
);

DROP TABLE IF EXISTS CartaDeConducao;

CREATE TABLE CartaDeConducao(
    IDCarta INTEGER NOT NULL, 
    Numero INTEGER NOT NULL CHECK(Numero>=100000000 && Numero<=999999999),
    DataDeEmissao DATE NOT NULL,
    PRIMARY KEY(IDCarta)
);

DROP TABLE IF EXISTS CategoriaCarta;

CREATE TABLE CategoriaCarta(
    IDCarta INTEGER,
    IDCategoria INTEGER,
    PRIMARY KEY(IDCarta,IDCategoria),
    FOREIGN KEY(IDCarta) REFERENCES CartaDeConducao(IDCarta) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDCategoria) REFERENCES Categoria(IDCategoria) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Escola;

CREATE TABLE Escola(
    IDEscola INTEGER NOT NULL,
    Nome TEXT NOT NULL,
    Morada TEXT NOT NULL,
    CodigoPostal TEXT NOT NULL CHECK(LENGTH(CodigoPostal)=8 AND SUBSTR(CodigoPostal,1,4) GLOB '[0-9][0-9][0-9][0,9]' AND SUBSTR(CodigoPostal,5,1) GLOB '-' AND SUBSTR(CodigoPostal,6,3) GLOB '[0-9][0-9][0-9]'),
    AlvaraDeFuncionamento TEXT NOT NULL,
    PRIMARY KEY(IDEscola)
);

DROP TABLE IF EXISTS Instrutor;

CREATE TABLE Instrutor(
    IDInstrutor INTEGER NOT NULL,
    IDEscola INTEGER,
    IDCarta INTEGER,
    Nome TEXT NOT NULL,
    DatadeNascimento DATE NOT NULL CHECK(DatadeNascimento<=('now','-21 years')),
    NIF INTEGER NOT NULL CHECK(NIF>=100000000 AND NIF<=299999999),
    CartaoDeCidadao INTEGER NOT NULL CHECK(CartaoDeCidadao>=10000000 AND CartaoDeCidadao<=99999999),
    CodigoPostal TEXT NOT NULL CHECK(LENGTH(CodigoPostal)=8 AND SUBSTR(CodigoPostal,1,4) GLOB '[0-9][0-9][0-9][0,9]' AND SUBSTR(CodigoPostal,5,1) = '-' AND SUBSTR(CodigoPostal,6,3) GLOB '[0-9][0-9][0-9]'),
    Morada TEXT NOT NULL,
    LicencaDeInstrucao TEXT NOT NULL,
    PRIMARY KEY(IDInstrutor),
    FOREIGN KEY(IDEscola) REFERENCES Escola(IDEscola) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDCarta) REFERENCES CartaDeConducao(IDCarta) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Examinando;

CREATE TABLE Examinando(
    IDExaminando INTEGER NOT NULL,
    IDInstrutor INTEGER,
    IDEscola INTEGER,
    Nome TEXT NOT NULL,
    DatadeNascimento DATE NOT NULL CHECK(DatadeNascimento<=('now','-18 years')),
    NIF INTEGER NOT NULL CHECK(NIF>=100000000 AND NIF<=299999999),
    CartaoDeCidadao INTEGER NOT NULL CHECK(CartaoDeCidadao>=10000000 AND CartaoDeCidadao<=99999999),
    CodigoPostal TEXT NOT NULL CHECK(LENGTH(CodigoPostal)=8 AND SUBSTR(CodigoPostal,1,4) GLOB '[0-9][0-9][0-9][0,9]' AND SUBSTR(CodigoPostal,5,1) = '-' AND SUBSTR(CodigoPostal,6,3) GLOB '[0-9][0-9][0-9]'),
    Morada TEXT NOT NULL,
    LicencaDeAprendizagem TEXT NOT NULL,
    PRIMARY KEY(IDExaminando),
    FOREIGN KEY(IDInstrutor) REFERENCES Instrutor(IDInstrutor) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDEscola) REFERENCES Escola(IDEscola) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Examinador;

CREATE TABLE Examinador(
    IDExaminador INTEGER NOT NULL,
    IDCarta INTEGER,
    Nome TEXT NOT NULL,
    DatadeNascimento DATE NOT NULL CHECK(DatadeNascimento<=('now','-25 years')),
    NIF INTEGER NOT NULL CHECK(NIF>=100000000 AND NIF<=299999999),
    CartaoDeCidadao INTEGER NOT NULL CHECK(CartaoDeCidadao>=10000000 AND CartaoDeCidadao<=99999999),
    CodigoPostal TEXT NOT NULL CHECK(LENGTH(CodigoPostal)=8 AND SUBSTR(CodigoPostal,1,4) GLOB '[0-9][0-9][0-9][0,9]' AND SUBSTR(CodigoPostal,5,1) = '-' AND SUBSTR(CodigoPostal,6,3) GLOB '[0-9][0-9][0-9]'),
    Morada TEXT NOT NULL,
    CredencialdeExaminador TEXT NOT NULL,
    PRIMARY KEY(IDExaminador),
    FOREIGN KEY(IDCarta) REFERENCES CartaDeConducao(IDCarta) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Proprietario;

CREATE TABLE Proprietario(
    IDEscola INTEGER,
    IDInstrutor INTEGER,
    Proprietario BOOLEAN,
    PRIMARY KEY(IDEscola,IDInstrutor),
    FOREIGN KEY(IDEscola) REFERENCES Escola(IDEscola) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDInstrutor) REFERENCES Instrutor(IDInstrutor) ON DELETE SET NULL ON UPDATE CASCADE
);


DROP TABLE IF EXISTS Veiculo;

CREATE TABLE Veiculo(
    IDVeiculo INTEGER NOT NULL,
    Matricula TEXT NOT NULL CHECK(LENGTH(Matricula)=6 AND ((SUBSTR(Matricula, 1, 2) GLOB '[A-Z][A-Z]' AND SUBSTR(Matricula, 3, 2) GLOB '[0-9][0-9]' AND SUBSTR(Matricula, 5, 2) GLOB '[A-Z][A-Z]') OR (SUBSTR(Matricula, 1, 2) GLOB '[0-9][0-9]' AND SUBSTR(Matricula, 3, 2) GLOB '[A-Z][A-Z]' AND SUBSTR(Matricula, 5, 2) GLOB '[0-9][0-9]') OR (SUBSTR(Matricula, 1, 2) GLOB '[A-Z][A-Z]' AND SUBSTR(Matricula, 3, 2) GLOB '[A-Z][A-Z]' AND SUBSTR(Matricula, 5, 2) GLOB '[0-9][0-9]') OR (SUBSTR(Matricula, 1, 2) GLOB '[0-9][0-9]' AND SUBSTR(Matricula, 3, 2) GLOB '[0-9][0-9]' AND SUBSTR(Matricula, 5, 2) GLOB '[A-Z][A-Z]'))),
    Marca VARCHAR(50) NOT NULL,
    NKMPercorridos INTEGER NOT NULL CHECK(NKMPercorridos>0),
    DataDeInspecao DATE NOT NULL CHECK(DataDeInspecao>=DATE('now','-1 years')),
    Seguro TEXT NOT NULL,
    IDInstrutor INTEGER,
    IDCategoria INTEGER,
    IDEscola INTEGER,
    PRIMARY KEY(IDVeiculo),
    FOREIGN KEY(IDInstrutor) REFERENCES Instrutor(IDInstrutor) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDCategoria) REFERENCES Categoria(IDCategoria) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDEscola) REFERENCES Escola(IDEscola) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Percurso;

CREATE TABLE Percurso(
    IDPercurso INTEGER NOT NULL,
    Nome TEXT NOT NULL,
    Perimetro INTEGER NOT NULL CHECK(Perimetro>0),
    PontoDeTroca TEXT NOT NULL,
    PRIMARY KEY(IDPercurso)
);


DROP TABLE IF EXISTS Exame;

CREATE TABLE Exame(
    IDExame INTEGER NOT NULL,
    IDPercurso INTEGER,
    IDExaminador INTEGER,
    Data DATE NOT NULL CHECK (strftime('%w', Data) BETWEEN '1' AND '5'),
    Hora TIME NOT NULL CHECK (Hora BETWEEN '11:00:00' AND '14:00:00'),
    PRIMARY KEY(IDExame),
    FOREIGN KEY(IDPercurso) REFERENCES Percurso(IDPercurso) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDExaminador) REFERENCES Examinador(IDExaminador) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Aprovacao;

CREATE TABLE Aprovacao(
    IDAprovacao INTEGER NOT NULL,
    IDExaminando INTEGER,
    IDExame INTEGER,
    IDCarta INTEGER,
    Duracao INTEGER NOT NULL CHECK(Duracao>0 AND Duracao<=40),
    Ordem TEXT NOT NULL,
    Avaliacao INTEGER NOT NULL CHECK(Avaliacao>=0 AND Avaliacao<=10),
    PRIMARY KEY(IDAprovacao),
    FOREIGN KEY(IDExaminando) REFERENCES Examinando(IDExaminando) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDExame) REFERENCES Exame(IDExame) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDCarta) REFERENCES CartaDeConducao(IDCarta) ON DELETE SET NULL ON UPDATE CASCADE
);

DROP TABLE IF EXISTS Reprovacao;

CREATE TABLE Reprovacao(
    IDReprovacao INTEGER NOT NULL,
    IDExaminando INTEGER,
    IDExame INTEGER,
    Duracao INTEGER NOT NULL CHECK(Duracao>0 AND Duracao<=40),
    Ordem TEXT NOT NULL,
    Motivo TEXT NOT NULL,
    PRIMARY KEY(IDReprovacao),
    FOREIGN KEY(IDExaminando) REFERENCES Examinando(IDExaminando) ON DELETE SET NULL ON UPDATE CASCADE,
    FOREIGN KEY(IDExame) REFERENCES Exame(IDExame) ON DELETE SET NULL ON UPDATE CASCADE
);
```

<a id="4.2"></a>
### Script de população SQL

O script SQL **populate.sql** para preencher a base de dados está disponível [aqui](https://github.com/TM-1-3/Driving-Exam-Center-Database/blob/master/populate.sql).

```sql
PRAGMA foreign_keys=ON;

INSERT INTO Categoria (IDCategoria,Designacao) VALUES (1,'A');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (2,'A1');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (3,'A2');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (4,'AM');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (5,'B');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (6,'B1');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (7,'C');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (8,'C1');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (9,'D');
INSERT INTO Categoria (IDCategoria,Designacao) VALUES (10,'D1');

INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (45321,120345678,'2017-05-11');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (98765,198234567,'2015-08-23');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (12345,150987654,'2013-02-18');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (56789,178654321,'2014-10-09');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (67890,167890123,'2016-01-30');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (23456,156789432,'2012-07-14');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (34567,134678901,'2011-11-26');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (89012,163489765,'2010-03-05');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (87654,145678903,'2014-06-02');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (45678,182345678,'2013-04-17');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (54321,155432198,'2015-10-12');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (65432,139876543,'2012-09-30');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (78901,172349065,'2017-03-08');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (23401,160234987,'2016-07-21');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (32109,168765432,'2011-05-04');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (65410,177654320,'2010-09-18');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (87631,159876234,'2015-06-27');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (98712,148765019,'2014-12-12');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (54367,125678902,'2013-11-01');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (13579,169432876,'2012-04-25');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (24680,131234875,'2011-02-14');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (12437,158392476,'2024-11-18');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (38592,274519683,'2024-11-20');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (74106,395827164,'2024-11-19');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (49827,421763958,'2024-11-19');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (56318,537291846,'2024-11-20');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (67249,684215739,'2024-11-20');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (81235,763924581,'2024-11-24');
INSERT INTO CartaDeConducao (IDCarta,Numero,DataDeEmissao) VALUES (93471,895317642,'2024-11-29');

INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (45321,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (98765,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (12345,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (56789,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (67890,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (23456,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (34567,9);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (89012,7);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (87654,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (45678,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (54321,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (65432,9);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (78901,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (23401,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (32109,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (65410,7);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (87631,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (98712,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (54367,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (13579,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (24680,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (12437,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (38592,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (74106,7);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (49827,7);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (56318,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (67249,5);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (81235,1);
INSERT INTO CategoriaCarta (IDCarta,IDCategoria) VALUES (93471,9);

INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (123,'Roda Viva','Rua das Flores, 15','1000-001','Alvará nº12');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (547,'Volante Seguro','Avenida do Horizonte, 98','2005-205','Alvará nº23');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (892,'Estrada Real','Travessa dos Pinheiros, 27','3000-325','Alvará nº37');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (301,'Nova Direção','Largo da Esperança, 10','4004-543','Alvará nº45');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (678,'Luz Verde','Rua do Sol Nascente, 62','5000-876','Alvará nº56');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (235,'Horizonte Seguro','Praça da Liberdade, 8','7002-333','Alvará nº74');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (765,'Guiarte','Rua dos Girassóis, 44','8008-456','Alvará nº82');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (980,'Lidador','Alameda dos Jacarandás, 73','9003-210','Alvará nº91');
INSERT INTO Escola (IDEscola,Nome,Morada,CodigoPostal,AlvaraDeFuncionamento) VALUES (456,'Lagoncinha','Beco do Amanhecer, 19','2805-207','Alvará nº99');

INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (184,123,45321,'George Washington','1990-01-25',100000000,10000000,'5678-530','Rua Aldeias de Baixo, 47','Licença nº1');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (253,547,98765,'Abraham Lincoln','1989-11-11',100000001,10000001,'5678-531','Rua Aldeias de Baixo, 48','Licença nº2');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (746,892,12345,'Thomas Jefferson','1990-01-11',100000002,10000002,'5678-533','Rua Aldeias de Baixo, 49','Licença nº3');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (592,301,56789,'Andrew Jackson','1990-03-29',100000003,10000003,'5678-534','Rua Aldeias de Baixo, 50','Licença nº4');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (876,678,67890,'Martin Van Buren','1987-01-02',100000004,10000004,'5678-535','Rua Aldeias de Baixo, 51','Licença nº5');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (435,301,23456,'James Polk','1982-12-25',100000005,10000005,'5678-536','Rua Aldeias de Baixo, 52','Licença nº6');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (912,235,34567,'Franklin Pierce','1989-12-19',100000006,10000006,'5678-537','Rua Aldeias de Baixo, 53','Licença nº7');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (678,765,89012,'John Adams','1991-09-24',100000007,10000007,'5678-538','Rua Aldeias de Baixo, 54','Licença nº8');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (325,980,87654,'John Quincy Adams','1975-02-13',100000008,10000008,'5678-539','Rua Aldeias de Baixo, 55','Licença nº9');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (508,456,45678,'William Henry Harrison','1987-11-17',100000009,10000009,'5678-540','Rua Aldeias de Baixo, 56','Licença nº10');
INSERT INTO Instrutor (IDInstrutor,IDEscola,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeInstrucao) VALUES (630,456,54321,'Milliard Fillmore','1989-12-18',100000010,10000010,'5678-541','Rua Aldeias de Baixo, 57','Licença nº11');

INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (2154,253,547,'Ulysses S Grant','1980-02-01',200000000,20000000,'6398-740','Rua Mouzinho da Silveira,30','Licença nº1020');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (4731,184,123,'Grover Cleveland','1980-02-02',200000001,20000001,'6398-741','Rua Mouzinho da Silveira,31','Licença nº1021');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (8369,678,765,'James Garfield','1980-02-03',200000002,20000002,'6398-742','Rua Mouzinho da Silveira,32','Licença nº1022');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (1258,746,892,'James Buchanan','1980-02-04',200000003,20000003,'6398-743','Rua Mouzinho da Silveira,33','Licença nº1023');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (9123,435,201,'Woodrow Wilson','1980-02-05',200000004,20000004,'6398-744','Rua Mouzinho da Silveira,34','Licença nº1024');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (7594,678,765,'Teddy Roosevelt','1980-02-06',200000005,20000005,'6398-745','Rua Mouzinho da Silveira,35','Licença nº1025');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (6407,592,301,'William Howard Taft','1980-02-07',200000006,20000006,'6398-746','Rua Mouzinho da Silveira,36','Licença nº1026');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (3492,435,201,'William McKinley','1980-02-08',200000007,20000007,'6398-747','Rua Mouzinho da Silveira,37','Licença nº1027');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (2875,912,235,'Calvin Coolidge','1980-02-09',200000008,20000008,'6398-748','Rua Mouzinho da Silveira,38','Licença nº1028');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (6081,912,235,'Herbert Hoover','1980-02-10',200000009,20000009,'6398-749','Rua Mouzinho da Silveira,39','Licença nº1029');
INSERT INTO Examinando (IDExaminando,IDInstrutor,IDEscola,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,LicencaDeAprendizagem) VALUES (1000,876,678,'Warren G Harding','1980-02-11',200000010,20000010,'6398-750','Rua Mouzinho da Silveira,40','Licença nº1030');

INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (12,65432,'Franklin D Roosevelt','1998-02-02',300000000,30000000,'6398-740','Rua Abade Açúcar e Lua,27','Credencial nº50');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (15,65410,'Harry Truman','1987-07-02',300000001,30000001,'6398-740','Rua Abade Açúcar e Lua,31','Credencial nº51');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (18,32109,'Dwight Eisenhower','1993-12-21',300000002,30000002,'6398-740','Rua Abade Açúcar e Lua,89','Credencial nº52');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (21,23401,'John F Kennedy','1976-11-30',300000003,30000003,'6398-740','Rua Abade Açúcar e Lua,65','Credencial nº53');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (23,98712,'Lyndon B Johnson','1985-09-08',300000004,30000004,'6398-740','Rua Abade Açúcar e Lua,90','Credencial nº54');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (26,78901,'Richard Nixon','1982-11-09',300000005,30000005,'6398-740','Rua Abade Açúcar e Lua,65','Credencial nº55');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (14,54367,'Gerald Ford','1981-02-04',3000000006,30000006,'6398-740','Rua Abade Açúcar e Lua,38','Credencial nº56');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (19,13579,'Jimmy Carter','1979-09-06',300000007,30000007,'6398-740','Rua Abade Açúcar e Lua,19','Credencial nº57');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (30,87631,'Ronald Reagan','1995-04-12',300000008,30000008,'6398-740','Rua Abade Açúcar e Lua,87','Credencial nº58');
INSERT INTO Examinador (IDExaminador,IDCarta,Nome,DatadeNascimento,NIF,CartaoDeCidadao,CodigoPostal,Morada,CredencialDeExaminador) VALUES (22,24680,'George H W Bush','1992-09-17',300000009,30000009,'6398-740','Rua Abade Açúcar e Lua,52','Credencial nº59');

INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (123,184,1);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (547,253,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (892,746,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (301,592,1);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (678,876,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (301,435,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (235,912,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (765,678,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (980,325,1);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (456,508,0);
INSERT INTO Proprietario (IDEscola, IDInstrutor, Proprietario) VALUES (456,630,0);

INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (247,'AB12CD','Toyota',12450,'2024-11-30','SeguroProtec',184,5,123);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (364,'34XY56','Volkswagen',25600,'2024-10-31','SeguroProtec',253,1,547);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (518,'DE78FG','Ford',37890,'2024-10-01','ProtecDrive',746,5,892);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (682,'98GH12','Mercedes-Benz',45300,'2024-09-01','AutoSegura',435,1,301);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (739,'MN34OP','Honda',58720,'2024-08-02','ProtecDrive',876,5,678);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (846,'56PQ78','Hyundai',67150,'2024-07-03','SeguroProtec',184,5,123);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (913,'QR90ST','BMW',78360,'2024-06-03','AutoSegura',435,1,301);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (721,'12TU34','Audi',84950,'2024-05-04','SafeCar',912,9,235);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (574,'VW56XY','Nissan',99430,'2024-04-04','ProtecDrive',678,7,765);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (956,'78ZA90','Ford',112800,'2024-03-05','SeguroProtec',325,5,980);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (865,'72ZB91','Toyota',125670,'2024-02-04','SafeCar',508,7,456);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (834,'54GH87','BMW',143520,'2024-01-05','SafeCar',630,5,456);
INSERT INTO Veiculo (IDVeiculo, Matricula, Marca, NKMPercorridos, DataDeInspecao, Seguro, IDInstrutor, IDCategoria, IDEscola) VALUES (178,'TM65OI','Volkswagen',158900,'2023-12-06','AutoSegura',592,5,301);

INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (27,'ACP-Santa Catarina-Aliados-Boavista','Aliados');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (34,'ACP-Constituição-Gomes da Costa-Circunvalação','Gomes da Costa');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (45,'ACP-Cedofeita-Batalha-Taipas','Batalha');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (22,'ACP-Aliados-Boavista-Dom João IV','Boavista');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (39,'ACP-Rodrigues Samapaio-Heroísmo-Boavista','Heroísmo');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (41,'ACP-Santa Catarina-Ribeira-D.João I','Ribeira');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (31,'ACP-Gomes da Costa-José Falcão-Aliados','José Falcão');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (26,'ACP-Boavista-Cedofeita-Avenida da Liberdade','Cedofeita');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (49,'ACP-Santa Catarina-Praça da República-José Falcão','Praça da República');
INSERT INTO Percurso (IDPercurso,Nome,Perimetro,PontoDeTroca) VALUES (28,'ACP-Boavista-Aliados-Cedofeita','Aliados');

INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (2487,27,12,'2024-11-18','11:05:00');
INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (3615,34,15,'2024-11-19','11:15:00');
INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (4926,45,18,'2024-11-20','12:30:00');
INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (5783,22,21,'2024-11-20','12:45:00');
INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (9372,27,26,'2024-11-24','12:50:00');
INSERT INTO Exame (IDExame,IDPercurso,IDExaminador,Data,Hora) VALUES (6538,28,30,'2024-11-29','13:10:00');

INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (8412,2154,2487,12437,40,'Ulysses S Grant - Woodrow Wilson',8);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (3791,4731,5783,38592,32,'James Buchanan - Grover Cleveland',7);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (9267,8369,3615,74106,35,'James Garfield-Teddy Roosevelt',8);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (1584,7594,3615,49827,37,'James Garfield-Teddy Roosevelt',10);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (4926,1000,4926,56318,39,'Warren G Harding - William Howard Taft',6);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (7638,6407,4926,67249,40,'Warren G Harding - William Howard Taft',9);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (2379,3492,9372,81235,36'William McKinley - Woodrow Wilson',7);
INSERT INTO Aprovacao (IDAprovacao,IDExaminando,IDExame,IDCarta,Duracao,Ordem,Avaliacao) VALUES (6894,2875,6538,93471,39,'Calvin Coolidge - Herbert Hoover',9);

INSERT INTO Reprovacao (IDReprovacao, IDExaminando, IDExame, Duracao, Ordem, Motivo) VALUES (4172,9123,2487,10,'Ulysses S Grant - Woodrow Wilson','Corte de prioridade');
INSERT INTO Reprovacao (IDReprovacao, IDExaminando, IDExame, Duracao, Ordem, Motivo) VALUES (8543,1258,5783,30,'James Buchanan - Grover Cleveland','Circulação em contramão');
INSERT INTO Reprovacao (IDReprovacao, IDExaminando, IDExame, Duracao, Ordem, Motivo) VALUES (6317,9123,9372,15,'William McKinley - Woodrow Wilson','Queda do motociclo');
INSERT INTO Reprovacao (IDReprovacao, IDExaminando, IDExame, Duracao, Ordem, Motivo) VALUES (2948,6081,6538,20,'Calvin Coolidge - Herbert Hoover','Corte de prioridade');
```
