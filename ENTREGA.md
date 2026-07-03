# Tech Challenge – Fase 2: Deploy da Plataforma ToggleMaster na AWS

## Autor

Julio Victor Benes Damasceno

---

# Objetivo

O objetivo desta etapa foi realizar o deploy da aplicação **ToggleMaster** em ambiente de nuvem utilizando serviços da AWS, substituindo o banco de dados executado em container Docker por um banco gerenciado no **Amazon RDS PostgreSQL**.

Além disso, a aplicação foi disponibilizada publicamente através de uma instância **Amazon EC2**, executando em containers Docker e utilizando **Gunicorn** como servidor de aplicação.

---

# Tecnologias Utilizadas

- Amazon EC2
- Amazon RDS PostgreSQL
- Docker
- Docker Compose
- Python 3
- Flask
- Gunicorn
- PostgreSQL
- GitHub
- Draw.io
- AWS Pricing Calculator

---

# Estrutura do Projeto

O projeto é composto por uma aplicação Flask responsável pelo gerenciamento de Feature Flags.

A aplicação é executada através do Gunicorn dentro de um container Docker e realiza toda a comunicação com um banco PostgreSQL hospedado no Amazon RDS.

O código-fonte encontra-se versionado no GitHub.

---

# Arquitetura da Solução

> https://drive.google.com/file/d/1nxF65WQhUFYISJOR_c4BHPAtsYDt934-/view?usp=sharing

A arquitetura implementada é composta por:

- Internet
- Internet Gateway
- Amazon VPC
- Public Subnet
- Private Subnet
- Amazon EC2
- Amazon RDS PostgreSQL

A comunicação ocorre da seguinte forma:

Internet

↓

Internet Gateway

↓

Amazon EC2 (Flask + Gunicorn + Docker)

↓

Amazon RDS PostgreSQL

Toda a comunicação entre a aplicação e o banco de dados ocorre utilizando a porta **5432**, sendo permitido acesso apenas através do **Security Group da EC2**, mantendo o banco privado.

---

# Infraestrutura AWS

Foi criada uma instância Amazon EC2 executando Ubuntu Server.

Nesta instância foram instalados:

- Docker
- Docker Compose
- PostgreSQL Client
- Aplicação ToggleMaster

A aplicação foi publicada utilizando Gunicorn escutando na porta **5000**.

Foi criado também um banco de dados Amazon RDS PostgreSQL responsável por armazenar todas as Feature Flags da aplicação.

O acesso ao banco foi restringido através dos Security Groups da AWS.

---

# Banco de Dados

O banco de dados utilizado é um Amazon RDS PostgreSQL.

Após sua criação, foi realizada a migração da aplicação para utilizar o endpoint do RDS ao invés do banco PostgreSQL executado localmente em Docker.

Foi validada a conexão utilizando o cliente **psql** diretamente pela instância EC2.

Também foi executada a criação automática da tabela **flags** durante a inicialização da aplicação.

---

# Docker

A aplicação foi executada utilizando Docker Compose.

Durante o deploy foi realizada a reconstrução da imagem da aplicação para utilizar o banco hospedado no Amazon RDS.

O container inicia automaticamente executando:

- Verificação da disponibilidade do banco
- Inicialização da tabela
- Inicialização do Gunicorn

---

# Testes Realizados

Após o deploy foram realizados testes para validar o funcionamento da aplicação.

Os seguintes endpoints foram testados com sucesso:

- GET /health
- GET /flags
- POST /flags

Os testes confirmaram que:

- A aplicação estava acessível pela Internet.
- O Gunicorn estava funcionando corretamente.
- A aplicação conseguia conectar ao Amazon RDS.
- As Feature Flags eram gravadas corretamente no banco de dados.

---

# Segurança

Foram utilizados Security Groups para controlar os acessos aos serviços.

### EC2

- SSH (22): permitido apenas para meu endereço IP.
- HTTP (80): acesso público.
- TCP 5000: acesso público para a aplicação.

### Amazon RDS

- PostgreSQL (5432): permitido apenas para o Security Group da EC2.

Dessa forma, o banco de dados permanece inacessível diretamente pela Internet.

---

# Estimativa de Custos

Foi utilizada a ferramenta **AWS Pricing Calculator** para estimar os custos da infraestrutura.

Os serviços considerados foram:

- Amazon EC2
- Amazon RDS PostgreSQL

A estimativa foi baseada na região **US East (N. Virginia)** e contempla o custo mensal da infraestrutura utilizada durante o desenvolvimento.

---

# Repositório

Repositório do projeto:

**https://github.com/JulioBenes/toggle-master-monolith**

---

# Conclusão

O objetivo do desafio foi concluído com sucesso.

Foi realizada a implantação da aplicação ToggleMaster em ambiente AWS utilizando Amazon EC2 e Amazon RDS PostgreSQL.

A aplicação passou a utilizar um banco de dados gerenciado, mantendo a comunicação segura entre os serviços através de uma VPC e Security Groups.

Todos os testes executados demonstraram o funcionamento correto da aplicação, incluindo a comunicação com o banco de dados, a criação das Feature Flags e a disponibilidade da API através da Internet.

As evidências da implantação e dos testes realizados encontram-se demonstradas no vídeo de entrega.