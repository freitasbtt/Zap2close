# 📄 Introdução

Este diretório contém a documentação detalhada do sistema **WhatsApp SDR**.

---

## 🏗 Arquitetura Geral
- **Servidor**: Hostinger VPS
- **Containerização**: Docker
- **Orquestração de Fluxos**: N8N
- **Integração WhatsApp**: Evolution API
- **Banco de Dados**: PostgreSQL
- **Redis**: Cache e Fila

---

## 🔄 Fluxo Básico de Mensagens
1. O cliente envia mensagem via WhatsApp.
2. A Evolution API recebe e processa a mensagem.
3. O N8N executa o fluxo configurado.
4. Resposta é enviada de volta pelo WhatsApp.

---

## 📂 Próximos Arquivos
- `fluxo-n8n.md` → Diagrama e detalhes do fluxo N8N.
- `infraestrutura.md` → Configuração de servidores e Docker.
- `api-evolution.md` → Documentação sobre uso da Evolution API.
