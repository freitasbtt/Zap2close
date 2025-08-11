\# Modelo Relacional – WhatsApp SDR



Este documento descreve as entidades do banco e seus relacionamentos.



\## Visão geral (ERD)

!\[ERD](images/modelo\_relacional\_whatsapp\_sdr.png)



\## Tabelas



\### clientes

\- `id\\\_cliente` (PK)

\- `nome` (varchar)

\- `numero\\\_whatsapp` (unique)

\- `token\\\_api` (unique)

\- `criado\\\_em` (timestamp)



\### mensagens

\- `id\\\_mensagem` (PK)

\- `id\\\_cliente` (FK → clientes.id\_cliente)

\- `conteudo` (text)

\- `direcao` (varchar: 'entrada'|'saida')

\- `data\\\_envio` (timestamp)

\- `status` (varchar)



\*\*Relação\*\*: `clientes (1) — (N) mensagens`

Cada mensagem pertence a exatamente um cliente.



\### execucoes\_clientes

\- `execution\\\_id` (PK, FK → n8n.execution\_entity.id)

\- `id\\\_cliente` (FK → clientes.id\_cliente)

\- `workflow\\\_id` (opcional – id do fluxo no n8n)

\- `status` (running|success|error)

\- `started\\\_at` (timestamp)

\- `finished\\\_at` (timestamp)



\*\*Relações\*\*:

\- `clientes (1) — (N) execucoes\\\_clientes`

\- `execution\\\_entity (n8n) (1) — (1) execucoes\\\_clientes` (mapeamento 1:1 de execução para cliente)



\## Integração com n8n

\- Use `{{$execution.id}}` para preencher `execucoes\\\_clientes.execution\\\_id`.

\- No início do workflow: descubra `id\\\_cliente` e `INSERT` em `execucoes\\\_clientes` com status `running`.

\- No fim do workflow (sucesso/erro): `UPDATE` em `execucoes\\\_clientes` setando `status` e `finished\\\_at = NOW()`.



\## SQL de referência



```sql

-- clientes

CREATE TABLE clientes (

\&nbsp; id\\\_cliente SERIAL PRIMARY KEY,

\&nbsp; nome VARCHAR(100) NOT NULL,

\&nbsp; numero\\\_whatsapp VARCHAR(20) UNIQUE NOT NULL,

\&nbsp; token\\\_api VARCHAR(255) UNIQUE NOT NULL,

\&nbsp; criado\\\_em TIMESTAMP DEFAULT CURRENT\\\_TIMESTAMP

);



-- mensagens

CREATE TABLE mensagens (

\&nbsp; id\\\_mensagem SERIAL PRIMARY KEY,

\&nbsp; id\\\_cliente  INT NOT NULL,

\&nbsp; conteudo    TEXT NOT NULL,

\&nbsp; direcao     VARCHAR(10) CHECK (direcao IN ('entrada','saida')),

\&nbsp; data\\\_envio  TIMESTAMP DEFAULT CURRENT\\\_TIMESTAMP,

\&nbsp; status      VARCHAR(20),

\&nbsp; FOREIGN KEY (id\\\_cliente) REFERENCES clientes(id\\\_cliente) ON DELETE CASCADE

);



-- vínculo execução (n8n) ↔ cliente

CREATE TABLE execucoes\\\_clientes (

\&nbsp; execution\\\_id BIGINT PRIMARY KEY, -- n8n.execution\\\_entity.id

\&nbsp; id\\\_cliente   INT NOT NULL,

\&nbsp; workflow\\\_id  BIGINT,

\&nbsp; status       VARCHAR(20) DEFAULT 'running',

\&nbsp; started\\\_at   TIMESTAMP DEFAULT NOW(),

\&nbsp; finished\\\_at  TIMESTAMP,

\&nbsp; FOREIGN KEY (execution\\\_id) REFERENCES execution\\\_entity(id) ON DELETE CASCADE,

\&nbsp; FOREIGN KEY (id\\\_cliente)   REFERENCES clientes(id\\\_cliente) ON DELETE CASCADE

);



CREATE INDEX idx\\\_execucoes\\\_clientes\\\_id\\\_cliente ON execucoes\\\_clientes(id\\\_cliente);



