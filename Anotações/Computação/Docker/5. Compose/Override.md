
#Doker

[[0. Docker|Início]]

O conceito de **override** no `docker-compose` está diretamente ligado à ideia de **sobreposição de configurações**. Isso permite que você divida a configuração do `docker-compose` em múltiplos arquivos, onde arquivos adicionais _complementam ou substituem_ partes da configuração base (`docker-compose.yml`).

---

### 📌 Conceito de Override

Por padrão, ao executar:

```bash
docker-compose up
```

o Docker Compose procura por:

1. `docker-compose.yml` → arquivo base
    
2. `docker-compose.override.yml` → arquivo opcional que **sobrescreve** ou **adiciona** à configuração do arquivo base
    

Ambos os arquivos são automaticamente lidos juntos se você **não** especificar outros arquivos com `-f`.

---

### 🔧 Como funciona na prática?

#### Exemplo 1: Arquivo base `docker-compose.yml`

```yaml
services:
  web:
    image: myapp:latest
    ports:
      - "80:80"
    environment:
      - ENV=production
```

#### Exemplo 2: Arquivo `docker-compose.override.yml` (não precisa ser chamado com `-f`)

```yaml
services:
  web:
    environment:
      - DEBUG=true
    volumes:
      - ./src:/app
```

➡️ O serviço `web` agora terá:

- `image: myapp:latest`
    
- `ports: "80:80"`
    
- `environment: ENV=production`, `DEBUG=true`
    
- `volumes: ./src:/app`
    

> O conteúdo do `override` é **mesclado** com o original. Se o valor for escalar (como `image`), ele **substitui**. Se for uma lista (como `environment`), ele **adiciona**.

---

### 🧩 Vantagens e utilidade em projetos grandes

1. **Separação de ambientes**
    
    - Arquivo base: produção
        
    - Override: desenvolvimento, testes ou debug
        
2. **Evita duplicação**
    
    - Você reaproveita a base e sobrescreve só o que muda.
        
3. **Controle de versões por ambiente**
    
    - `docker-compose.override.yml` pode ser ignorado no Git (.gitignore), permitindo ajustes locais.
        
4. **Customização com múltiplos arquivos**
    
    ```bash
    docker-compose -f docker-compose.yml -f docker-compose.dev.yml up
    ```
    
    Permite combinar override de forma mais controlada e com nomes customizados.
    

---

### 🧱 Estrutura comum em projetos grandes

```
docker-compose.yml              # base comum a todos os ambientes
docker-compose.override.yml     # override automático para dev
docker-compose.prod.yml         # override para produção
docker-compose.test.yml         # override para testes automatizados
```

---
