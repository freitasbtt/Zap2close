# 🏗 Infraestrutura do Sistema

## Servidor
- **Provedor**: Hostinger VPS
- **Sistema Operacional**: Ubuntu 22.04
- **Gerenciamento**: SSH + Docker Compose

---

## Containerização
- **Docker**: Todos os serviços rodam em containers isolados.
- **Serviços**:
  - Evolution API
  - N8N
  - Redis
  - PostgreSQL

---

## Redis (Cache e Fila)
- Fila de mensagens entre Evolution API e N8N.
- Armazena dados temporários (QR Codes, sessões).
- Evita sobrecarga e perda de mensagens.

---

## Banco de Dados
- **PostgreSQL**: Armazena logs, conversas e configuração dos clientes.

---

## Fluxo Resumido
