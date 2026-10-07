# Variáveis de Ambiente no Compose

#Docker 

O Docker Compose tem nuances importantes em relação a variáveis de ambiente e atualizações: ele opera com dois tipos de arquivos `.env` com comportamentos distintos:

### 1. **Arquivo `.env` do Docker Compose (nível de projeto)**
- **Localização**: Na mesma pasta do `docker-compose.yml`
- **Função**: Substitui variáveis **no próprio arquivo compose** antes da execução
- **Exemplo**: 
  ```env
  # .env (do Compose)
  PROJECT_NAME=meuapp
  DB_PORT=5432
  ```
  ```yaml
  # docker-compose.yml
  services:
    db:
      image: postgres
      ports:
        - "${DB_PORT}:5432"
  ```

### 2. **Arquivos `env_file` (nível de serviço)**
- **Especificado por**: `env_file:` no serviço
- **Função**: Injeta variáveis **no container** durante a inicialização
- **Exemplo**:
  ```yaml
  services:
    app:
      env_file: 
        - app-vars.env
  ```

---

### 🔄 Comportamento de Atualização

| Cenário                                  | Comportamento                                                                 | Recria Container? |
|------------------------------------------|-------------------------------------------------------------------------------|-------------------|
| Modificação no **`.env` do projeto**     | Compose regenera a configuração (substitui variáveis no YML)                  | **Sim**           |
| Modificação no **`env_file` do serviço** | Novos valores são injetados no container na próxima execução                  | **Sim**           |
| Modificação em **variáveis de host**     | Só afeta novos containers (valores são capturados no momento do `up`)         | Sim (se usadas)   |

---

### 🧪 Teste Prático

**app-vars.env:**
```env
DEBUG_MODE=False
```

**docker-compose.yml:**
```yaml
services:
  app:
    image: python:3.9
    env_file: app-vars.env
    command: bash -c "echo 'Debug: $$DEBUG_MODE' && sleep 3600"
```

1. **Execução inicial:**
   ```bash
   docker-compose up  # Mostra "Debug: False"
   ```

2. **Modifique `app-vars.env`:**
   ```env
   DEBUG_MODE=True
   ```

3. **Execute novamente:**
   ```bash
   docker-compose up
   ```
   - **Saída:** "Debug: True"
   - O container anterior é destruído e um novo é criado

---

### ⚠️ Por que isso funciona? (Explicação técnica)

1. **Imutabilidade de containers**: 
   - Containers **nunca** são modificados após criação
   - Qualquer alteração na configuração exige **recriação do container**

2. **Inteligência do Docker Compose:**
   - Ao detectar mudanças em `env_file` ou no `.env` do projeto:
     - Para containers existentes
     - Remove o container antigo
     - Cria um novo container com as novas variáveis

3. **Fluxo simplificado:**
   ```mermaid
   graph TD
     A[Modifica arquivo .env] --> B{docker-compose up}
     B --> C[Compose lê novos valores]
     C --> D[Destrói container antigo]
     D --> E[Cria novo container com vars atualizadas]
   ```

---

### 🎯 Conclusão Importante

Seu entendimento estava **parcialmente correto**: 
- Containers existentes **não são atualizados** (são imutáveis)
- Mas o Docker Compose **automaticamente recria containers** quando detecta mudanças em:
  - Arquivos `env_file` 
  - Arquivo `.env` do projeto
  - Próprio `docker-compose.yml`

👉 **Sempre que modificar variáveis de ambiente, execute `docker-compose up` novamente** - o Compose cuidará da recriação necessária para aplicar as novas variáveis.