<img src='https://sigarra.up.pt/feup/pt/imagens/LogotipoSI' width="30%"/>

<div align="center">
🌍 <a href="README.md">English</a> | 🇵🇹 <a href="README.pt.md">Português</a>
</div>

<h3 align="center">BSc in Informatics and Computing Engineering<br> L.EIC012 - Databases<br> 2024/2025 </h3>

---
<h3 align="center"> Collaborators &#129309 </h2>

<div align="center">

| Name               | Number      |
|--------------------|-------------|
| Elton Vaz          | up202309925 |
| Henrique Vilarinho | up202307037 |
| Tomás Morais       | up202304692 |

Grade : 15,6

</div>

# Driving Exam Center SQLite Relational Database Report


To see the full report click here: <a href="reports/901_1ªSubmissão.pdf">First Report</a>  &  <a href="reports/901_2ªSubmissão.pdf">Second Report</a>

- [UML Diagram](#1) 
- [Relational Schema](#2)
- [Functional Dependencies Analysis and Normal Forms](#3)

<a id="1"></a>
## UML Diagram

<img width="1274" height="800" alt="Captura de ecrã de 2025-09-17 11-27-35" src="https://github.com/user-attachments/assets/f1ecd6e8-78d3-4aac-bebf-234f0eaf0896" />

<a id="2"></a>
## Relational Schema

Escola (<ins>IDEscola</ins>, Nome, Morada, CodigoPostal, AlvaraDeFuncionamento)

Instrutor(<ins>IDInstrutor</ins>,Nome,DataDeNascimento,NIF,NumeroCartaoCidadao, CodigoPostal, Morada, LicencaDeInstrucao,IDEscola-->Escola,IDCarta-->CartadeConducao)

Proprietario (<ins>IDInstrutor</ins>-->Instrutor, <ins>IDEscola</ins>-->Escola, Proprietario ?)

Veiculo (<ins>IDVeiculo</ins>, Matricula, Marca, NumeroKmPercorridos,DataDeInspecao, Seguro, IDEscola-->Escola, IDInstrutor-->Instrutor,IDCategoria-->Categoria)

Categoria (<ins>IDCategoria</ins>, Designacao)

CategoriaCarta (<ins>IDCarta</ins>-->CartaDeConducao, <ins>IDCategoria</ins>-->Categoria)

CartaDeConducao (<ins>IDCarta</ins>, Numero, DataDeEmissao)

Percurso (<ins>IDPercurso</ins>, Nome, Perimetro, PontoDeTroca)

Examinador(<ins>IDExaminador</ins>,Nome,DataDeNascimento,NIF,CartaoCidadao,CodigoPostal, Morada, CredencialDeExaminador,IDCarta-->CartaDeConducao)

Examinando (<ins>IDExaminando</ins>, Nome, DataDeNascimento, NIF,NumeroCartaoCidadao, CodigoPostal, Morada, LicencaDeAprendizagem,IDEscola-->Escola, IDInstrutor-->Instrutor)

Exame (<ins>IDExame</ins>, Data, Hora, IDExaminando--> Examinando,IDExaminador-->Examinador, IDPercurso-->Percurso)

Aprovacao (<ins>IDAprovacao</ins>,Duracao, Avaliacao, Ordem, <ins>IDExaminando</ins>-->Examinando, <ins>IDExame</ins>-->Exame,IDCarta-->CartaDeConducao)

Reprovacao (<ins>IDReprovacao</ins>,Duracao, Motivo, Ordem, <ins>IDExaminando</ins>-->Examinando, <ins>IDExame</ins>-->Exame)

<a id="3"></a>
## Functional Dependencies Analysis and Normal Forms

| Relation | Pessoa | Escola | Veículo | Percurso |
| :--- | :--- | :--- | :--- | :--- | 
| **Keys** | IDPessoa (PrimaryKey), CartaoDeCidadao, NIF | IDEscola (PrimaryKey), Nome, AlvaraFuncionamento | IDVeiculo (Primary Key), Matricula | **IDPercurso** (Primary Key), Nome | 
| **Functional Dependencies** | **IDPessoa** → Nome, DataDeNascimento, NIF, CartaoDeCidadao, CodigoPostal, Morada.<br>**CartaoDeCidadao** → Nome, DataDeNascimento, NIF, CodigoPostal, Morada.<br>**NIF** → Nome, DataDeNascimento, CartaoDeCidadao, CodigoPostal, Morada. | **IDEscola** → Nome, Morada, CodigoPostal, AlvaraFuncionamento.<br>**Nome** → Morada, CodigoPostal, AlvaraFuncionamento.<br>**AlvaraFuncionamento** → Nome. | **IDVeiculo** → Matricula, Marca, Seguro, KmPercorridos, DataInspecção.<br>**Matricula** → Marca, Seguro, DataInspecção | **IDPercurso** → Nome, Perimetro, PontoDeTroca.<br>**Nome** → Perimetro, PontoDeTroca. |
| **Justification & Decomposition** | There is no need to decompose the Person class because ID, CartaodeCidado, and NIF are all superkeys of this class. | There is no need to decompose the Escola class because Name and AlvaraFuncionamento are superkeys. Each driving school can only have one operating license, so it is a superkey of the Escola class. | There is no need to decompose the Veiculo class because ID and Matricula are superkeys of this class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. |

| Relation | Exame | Instrutor | Examinador | Examinando |
| :--- | :--- | :--- | :--- | :--- |
| **Keys** | **IDExame** (Primary Key) | **IDInstrutor** (Primary Key), LicençaDeInstrucao | **IDExaminador** (Primary Key), CredencialDeExaminador | **IDExaminando** (Primary Key), LicençaDeAprendizagem |
| **Functional Dependencies** | **IDExame** → Data, Hora. | **IDInstrutor** → LicençaDeInstrucao | **IDExaminador** → CredencialDeExaminador | **IDExaminando** → LicençaDeAprendizagem |
| **Justification & Decomposition** | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. |

| Relation | Aprovação | Categoria | CartaDeConducao | Reprovacao |
| :--- | :--- | :--- | :--- | :--- |
| **Keys** | **IDAprovacao** (Primary Key) | **IDCategoria** (Primary Key), Designacao | **IDCarta** (Primary Key), Numero | **IDReprovacao** (Primary Key) |
| **Functional Dependencies** | **IDAprovacao** → Duracao, Avaliacao | **IDCategoria** → Designacao | **IDCarta** → Numero, DataDeEmissao | **IDReprovacao** → Duracao, Motivo |
| **Justification & Decomposition** | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. | There is only one Functional Dependency with the left element being a superkey. Therefore, there is no need to decompose the class. |



