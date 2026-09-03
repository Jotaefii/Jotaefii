# 🏫 Sistema de Gestão Escolar

Sistema de gestão escolar desenvolvido em **Java**, com persistência via **JDBC puro** e banco de dados **PostgreSQL**. O projeto simula o dia a dia de uma instituição de ensino, com autenticação por CPF/senha e menus distintos para **Gestor**, **Professor** e **Aluno**.

![Java](https://img.shields.io/badge/Java-2f54d1?style=for-the-badge&logo=openjdk&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-1a2b6b?style=for-the-badge&logo=postgresql&logoColor=white)
![JDBC](https://img.shields.io/badge/JDBC-0f1730?style=for-the-badge&logo=java&logoColor=white)
![Git](https://img.shields.io/badge/Git-2f54d1?style=for-the-badge&logo=git&logoColor=white)
![IntelliJ IDEA](https://img.shields.io/badge/IntelliJ%20IDEA-1a2b6b?style=for-the-badge&logo=intellijidea&logoColor=white)

<br>

## 📖 Sobre o projeto

O sistema permite o gerenciamento completo de uma escola, contemplando três tipos de usuário: **Gestor**, **Professor** e **Aluno**. Cada usuário se autentica com **CPF e senha**, sendo direcionado automaticamente para o menu correspondente ao seu tipo de acesso.

A aplicação cobre desde o cadastro de pessoas até o controle de notas, frequência e situação final do aluno, com toda a persistência de dados feita diretamente no **PostgreSQL** via **JDBC**.

<br>

## ⚙️ Funcionalidades

- 🔐 Autenticação de usuários (CPF e senha)
- 👨‍🎓 Cadastro, atualização, exclusão e consulta de alunos
- 👨‍🏫 Cadastro, atualização, exclusão e consulta de professores
- 🧑‍💼 Cadastro de gestores
- 🏷️ Gerenciamento de turmas
- 📝 Lançamento de notas
- 📅 Controle de frequência
- 📊 Cálculo da média final
- 📈 Cálculo da porcentagem de frequência
- ❌ Cálculo de faltas
- ✅ Definição da situação do aluno

A situação final do aluno pode ser:

| Situação | Descrição |
|---|---|
| ✅ APROVADO | Atingiu média e frequência mínimas |
| 📉 REPROVADO POR NOTA | Não atingiu a média mínima |
| 🚫 REPROVADO POR FALTA | Não atingiu a frequência mínima |
| 🔁 RECUPERAÇÃO | Situação intermediária, sujeita a nova avaliação |

<br>

## 🛠️ Tecnologias

| Tecnologia | Uso no projeto |
|---|---|
| ☕ Java | Linguagem principal do sistema |
| 🔌 JDBC | Comunicação direta com o banco de dados (sem ORM) |
| 🐘 PostgreSQL | Banco de dados relacional |
| 🌱 Git / GitHub | Controle de versão do código |
| 💡 IntelliJ IDEA | IDE utilizada no desenvolvimento |

> ℹ️ O projeto **não utiliza Spring Boot nem Spring Data JPA**. Toda a persistência é feita com **JDBC puro**, com SQL escrito manualmente.

<br>

## 🏗️ Arquitetura

O projeto segue uma **arquitetura em camadas**, separando claramente as responsabilidades:

- **Entity** — representação das entidades do domínio
- **Repository** — contratos de acesso a dados
- **Repository Impl** — implementação do acesso a dados via JDBC
- **Service** — contratos das regras de negócio
- **Service Impl** — implementação das regras de negócio
- **Exception** — Trata exceções 
- **Util** — Valida entradas de dados
- **Menu** — camada de interação com o usuário (console)
- **Config** — configuração da conexão com o banco

**Fluxo geral da aplicação:**

```
Usuário
   ↓
Main
   ↓
Menu
   ↓
Service
   ↓
Repository
   ↓
JDBC
   ↓
PostgreSQL
```

<br>

## 🗂️ Estrutura do projeto

```
src/
├── entity/
│   ├── User.java
│   ├── Student.java
│   ├── Teacher.java
│   ├── SchoolClass.java
│   └── Bulletin.java
│
├── repository/
│   ├── imp/
│      └── ...
│   ├── BulletinRepository.java
│   ├── SchoolRepository.java
│   ├── StudentRepository.java
│   ├── ...
│
├── service/
│   ├── imp/
│      └── ...
│   ├── BulletinService.java
│   ├── SchoolService.java
│   ├── StudentService.java
│   ├── TeacherService.java
│
├── menu/
│   ├── ManagementMenu.java
│   ├── TeacherMenu.java
│   └── StudentMenu.java
│
├── config/
│   └── ConnectionFactory.java
│
└── Main.java
```

<br>

## 🗄️ Banco de dados

Banco utilizado: **PostgreSQL**

**Principais tabelas:**

| Tabela | Descrição |
|---|---|
| `users` | Dados de login e informações gerais de todo usuário |
| `manager` | Dados específicos dos gestores |
| `students` | Dados específicos dos alunos |
| `teachers` | Dados específicos dos professores |
| `classes` | Turmas cadastradas |
| `report_cards` | Boletim de cada aluno |

**Relacionamentos principais:**

- `users → managers` — cada gestor está vinculado a um usuário
- `users → students` — cada aluno está vinculado a um usuário
- `users → teachers` — cada professor está vinculado a um usuário
- `teachers → classes` — cada turma possui um professor responsável
- `classes → students` — cada aluno pertence a uma turma
- `students → report_cards` — cada aluno possui um boletim associado

<br>

## 📋 Boletim

O boletim é dividido em **quatro bimestres**, e cada aluno possui:

- Notas do 1º ao 4º bimestre (`firstGrade`, `secondGrade`, `thirdGrade`, `fourthGrade`)
- Média final (`finalAverage`), calculada com base nas notas disponíveis
- Total de aulas (`totalClasses`) e aulas frequentadas (`attendance`)
- Faltas (`absences`) e porcentagem de frequência (`attendancePercentage`)
- Situação final do aluno (`situation`)

> 📌 A frequência mínima considerada para aprovação é **75%**.

<br>

## 🔐 Fluxo de autenticação

Exemplo do fluxo de login de um **aluno**:

```
Aluno informa CPF e senha
        ↓
Main realiza autenticação
        ↓
UserRepository consulta o banco
        ↓
Sistema identifica USER_TYPE = STUDENT
        ↓
Student é carregado através do CPF
        ↓
StudentMenu é iniciado
        ↓
Aluno pode consultar seu boletim
```

<br>

## 🔌 JDBC

A comunicação com o PostgreSQL é feita com **JDBC puro**, sem uso de ORM. O fluxo básico de uma consulta envolve:

- `Connection` — abre a conexão com o banco de dados
- `PreparedStatement` — monta a query SQL de forma segura, evitando SQL Injection
- `ResultSet` — percorre o resultado retornado pela consulta

Exemplo conceitual:

```java
Connection connection = ConnectionFactory.getConnection();
PreparedStatement statement = connection.prepareStatement(
    "SELECT * FROM users WHERE cpf = ?"
);
statement.setString(1, cpf);
ResultSet resultSet = statement.executeQuery();
```

<br>

## 🚀 Como executar

### Pré-requisitos
- [JDK 17+](https://www.oracle.com/java/technologies/downloads/) instalado
- [PostgreSQL](https://www.postgresql.org/download/) instalado e em execução

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/Jotaefii/NOME-DO-REPOSITORIO.git

# 2. Acesse a pasta do projeto
cd NOME-DO-REPOSITORIO

# 3. Crie o banco de dados no PostgreSQL
CREATE DATABASE NOME_DO_BANCO;
```

Configure as credenciais de acesso ao banco (por exemplo, em `ConnectionFactory.java` ou em um arquivo de configuração):

```
DB_URL=jdbc:postgresql://localhost:5432/NOME_DO_BANCO
DB_USER=SEU_USUARIO
DB_PASSWORD=SUA_SENHA
```

Por fim, execute o projeto a partir da classe `Main`:

```bash
java Main
```

<br>

## 🌱 Versionamento

O projeto utiliza **Git** para controle de versão. O desenvolvimento pode ser feito através de **branches** para novas funcionalidades, que posteriormente passam por **merge** na branch `main`.

<br>

## 🎯 Objetivo do projeto

Este projeto foi desenvolvido como forma de estudo e prática dos seguintes conceitos:

- Java
- JDBC
- SQL
- PostgreSQL
- Programação Orientada a Objetos (POO)
- Arquitetura em camadas
- Repository Pattern
- Service Layer
- Git/GitHub

<br>

## 👤 Autor

<div align="center">

**João Felipe**

Estudante de Análise e Desenvolvimento de Sistemas, com foco em back-end Java.

<a href="https://linkedin.com/in/jotaefi" target="_blank">
<img src="https://img.shields.io/badge/LinkedIn-2f54d1?style=for-the-badge&logo=linkedin&logoColor=white" />
</a>
<a href="https://instagram.com/eujotaefi" target="_blank">
<img src="https://img.shields.io/badge/Instagram-1a2b6b?style=for-the-badge&logo=instagram&logoColor=white" />
</a>
<a href="mailto:joaofelipecode@gmail.com" target="_blank">
<img src="https://img.shields.io/badge/Gmail-0f1730?style=for-the-badge&logo=gmail&logoColor=white" />
</a>

</div>
