# 🧾 Projeto de Logs e Anonimização — Sprint 4

Este projeto implementa uma API de registro de logs e um script de anonimização de dados em um banco SQLite, com controle de permissões por usuários específicos no Linux.

---

## 📦 Estrutura do Projeto

- `/usr/local/bin/anonimize.sh` — Script de anonimização  
- `/srv/sqlite/app.db` — Banco de dados SQLite  
- `/var/log/xp_access.log` — Log de acessos via API  
- `/var/log/xp_operacao.log` — Log de operações via API  
- `/var/log/xp_anonimize.log` — Log da execução do script de anonimização  
- `index.js` — API Node.js com endpoint `/log`

---

## 🚀 Etapas Implementadas

### Etapa 1 — API de Logs

Criado endpoint `POST /log` que recebe:

```json
{
  "tipo": "acesso" | "operacao",
  "mensagem": "Texto da mensagem"
}
```

Grava os logs em:
- `/var/log/xp_access.log` se `tipo = acesso`
- `/var/log/xp_operacao.log` se `tipo = operacao`

---

### Etapa 2 — Script de Anonimização

O script `/usr/local/bin/anonimize.sh` executa:
- Anonimização dos campos `name`, `cpf` e `email` na tabela `users`
- Registro da execução no log `/var/log/xp_anonimize.log`

**Exemplo de log:**
```
[qua 15 out 2025 23:46:08 -03] Script de anonimização executado com sucesso
```

---

### Etapa 3 — Usuários Específicos

- **loguser** → responsável por rodar a API e gravar logs  
- **anonuser** → responsável por executar o script de anonimização  

Permissões ajustadas com:

```bash
sudo chown loguser:loguser /var/log/xp_access.log /var/log/xp_operacao.log
sudo chown anonuser:anonuser /usr/local/bin/anonimize.sh /srv/sqlite/app.db /var/log/xp_anonimize.log
```

---

## 🧪 Testes

### Testar API de log:
```bash
curl -X POST http://127.0.0.1:3000/log   -H 'Content-Type: application/json'   -d '{"tipo":"acesso","mensagem":"Usuário Guilherme acessou a API"}'
```

Verificar log:
```bash
cat /var/log/xp_access.log
```

---

### Testar script de anonimização:
```bash
sudo su - anonuser
/usr/local/bin/anonimize.sh
```

Verificar log:
```bash
cat /var/log/xp_anonimize.log
```

Verificar dados anonimizados:
```bash
sqlite3 /srv/sqlite/app.db "SELECT name, cpf, email FROM users LIMIT 5;"
```

---

## 👤 Grupo:

- **Guilherme Doretto Sobreiro — RM: 99674**

- **Guilherme Fazito Ziolli Sordili - RM: 550539**

- **Raí Gumieri dos Santos - RM: 98287**
