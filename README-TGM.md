# Tech Challenge – Fase 1: Plataforma ToggleMaster

Bem-vindo ao repositório da primeira fase do Tech Challenge do curso de DevOps. O objetivo deste projeto foi implantar a aplicação **ToggleMaster**, uma plataforma simples de gerenciamento de Feature Flags, utilizando serviços da Amazon Web Services (AWS).

---

# 📖 Sobre o Projeto

A ToggleMaster é uma aplicação monolítica desenvolvida em Python/Flask que permite criar, consultar e atualizar Feature Flags.

Durante o desenvolvimento inicial, a aplicação utilizava um banco PostgreSQL executando em um container Docker. Na entrega final do Tech Challenge, a arquitetura foi evoluída para utilizar um Amazon RDS PostgreSQL, mantendo apenas a aplicação executando em Docker na instância Amazon EC2.

---

# 🎯 Objetivos

- Compreender uma aplicação monolítica.
- Implantar a aplicação na AWS.
- Provisionar uma instância Amazon EC2.
- Provisionar um banco Amazon RDS PostgreSQL.
- Configurar Security Groups.
- Migrar o banco PostgreSQL para o Amazon RDS.
- Validar o funcionamento da API.

---

# 🏗️ Arquitetura Final

A infraestrutura implementada é composta por:

- Amazon EC2
- Amazon RDS PostgreSQL
- Docker
- Docker Compose
- Gunicorn
- AWS Security Groups

Fluxo:

Internet → Amazon EC2 → Amazon RDS PostgreSQL

A aplicação permanece acessível pela porta 5000 enquanto o banco permanece privado, aceitando conexões apenas da EC2.

---

# 🛠️ Tecnologias Utilizadas

- Python
- Flask
- PostgreSQL
- Docker
- Docker Compose
- Gunicorn
- Amazon EC2
- Amazon RDS PostgreSQL
- GitHub

---

# 🚀 Execução Local

```bash
git clone https://github.com/JulioBenes/toggle-master-monolith.git
cd toggle-master-monolith
docker compose up --build
```

Teste:

```bash
curl http://localhost:5000/health
```

---

# 🌐 Deploy na AWS

A entrega final utiliza:

- Amazon EC2 Ubuntu Server
- Docker Compose
- Gunicorn
- Amazon RDS PostgreSQL

A conexão com o banco é realizada por variáveis de ambiente (DB_HOST, DB_NAME, DB_USER, DB_PASSWORD e DB_PORT).

---

# 📌 Endpoints

| Método | Endpoint |
|--------|----------|
| GET | /health |
| POST | /flags |
| GET | /flags |
| GET | /flags/{nome} |
| PUT | /flags/{nome} |

---

# 🔐 Segurança

- SSH (22): apenas IP do administrador.
- HTTP (80): público.
- TCP (5000): aplicação.
- PostgreSQL (5432): apenas EC2 → RDS.

---
# 📐 Diagrama da Arquitetura
[Arquitetura](https://drive.google.com/file/d/1nxF65WQhUFYISJOR_c4BHPAtsYDt934-/view?usp=sharing)

---

# 🎥 Vídeo da Demonstração

[Video de demonstração em funcionamento](https://www.youtube.com/watch?v=QEtYE_zAONI)

---

# 📄 Relatório

O relatório encontra-se no arquivo **ENTREGA.pdf** disponível neste repositório.

---

# 👤 Autor

Julio Victor Benes Damasceno
