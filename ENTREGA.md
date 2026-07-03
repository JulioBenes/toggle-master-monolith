# ToggleMaster - Documento de Entrega

## Aluno

**Nome:** Julio Victor Benes Damasceno

---

# Objetivo

Esta implementação consiste em uma API REST para gerenciamento de Feature Flags utilizando:

- Flask
- PostgreSQL
- Docker
- Docker Compose

A aplicação permite criar, consultar e atualizar Feature Flags persistidas em banco de dados PostgreSQL.

---

# Tecnologias Utilizadas

- Python 3.9
- Flask
- PostgreSQL 13
- psycopg2
- Gunicorn
- Docker
- Docker Compose

---

## Estrutura do Projeto

```
toggle-master-monolith/
│
├── .gitignore
├── app.py
├── docker-compose.yaml
├── Dockerfile
├── ENTREGA.md
├── entrypoint.sh
├── README.md
└── requirements.txt
```

---

# Arquitetura

A solução é composta por dois containers:

## App

Container responsável pela API Flask.

Responsabilidades:

- receber requisições HTTP
- conectar ao PostgreSQL
- criar a tabela automaticamente
- executar as operações CRUD das Feature Flags

---

## Banco de Dados

Container PostgreSQL responsável pelo armazenamento persistente das Feature Flags.

A persistência é realizada através do volume Docker:

```
postgres_data
```

---

# Inicialização Automática

O arquivo `entrypoint.sh` aguarda o PostgreSQL ficar disponível antes de iniciar a aplicação.

Após a conexão:

1. verifica se o banco está acessível;
2. executa:

```
flask init-db
```

3. cria automaticamente a tabela caso ela não exista;
4. inicia o Gunicorn.

---

# Banco de Dados

Tabela criada automaticamente:

```sql
CREATE TABLE IF NOT EXISTS flags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    is_enabled BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

---

# Endpoints Implementados

## Health Check

GET

```
/health
```

Resposta:

```json
{
    "status":"ok"
}
```

---

## Criar Feature Flag

POST

```
/flags
```

Exemplo:

```json
{
    "name":"nova-feature",
    "is_enabled":true
}
```

---

## Listar Feature Flags

GET

```
/flags
```

Resposta:

```json
[
    {
        "name":"nova-feature",
        "is_enabled":true
    }
]
```

---

## Consultar uma Feature Flag

GET

```
/flags/nova-feature
```

Resposta:

```json
{
    "name":"nova-feature",
    "is_enabled":true
}
```

---

## Atualizar Feature Flag

PUT

```
/flags/nova-feature
```

Exemplo:

```json
{
    "is_enabled":false
}
```

---

# Docker

A aplicação pode ser executada com apenas um comando:

```bash
docker compose up --build
```

Os containers iniciados são:

- app
- postgres

O banco é criado automaticamente.

---

# Variáveis de Ambiente

A aplicação utiliza as seguintes variáveis:

```
DB_HOST
DB_NAME
DB_USER
DB_PASSWORD
DB_PORT
```

Configuradas no arquivo:

```
docker-compose.yaml
```

---

# Testes Realizados

Durante o desenvolvimento foram realizados os seguintes testes:

- criação do banco PostgreSQL;
- inicialização automática da tabela;
- criação de Feature Flag;
- consulta de todas as Feature Flags;
- consulta individual;
- atualização de Feature Flag;
- verificação do endpoint `/health`;
- validação da persistência utilizando Docker.

Todos os testes foram executados com sucesso.

---

# Considerações

A implementação atende aos requisitos propostos utilizando uma arquitetura simples baseada em containers Docker.

A inicialização é automática, não sendo necessária nenhuma criação manual de tabelas ou configuração adicional do banco de dados após executar:

```bash
docker compose up --build
```

A API permanece disponível na porta:

```
http://localhost:5000
```

---

# Repositório

Projeto desenvolvido para a atividade prática da disciplina utilizando o repositório base disponibilizado pelo professor.