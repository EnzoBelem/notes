# Healthchecks

#Docker

Um **healthcheck** é um comando que o Docker executa periodicamente dentro do container para saber se o serviço está realmente funcionando, e não apenas com o processo rodando. O container passa por três estados: `starting`, `healthy` e `unhealthy`.

## Exemplo no Compose

```yaml
services:
  db:
    image: postgres:16
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 30s
```

- `test`: comando executado. Exit code `0` = saudável, `1` = não saudável.
- `interval`: tempo entre cada verificação.
- `timeout`: tempo máximo que o comando pode levar antes de contar como falha.
- `retries`: quantas falhas consecutivas são necessárias para marcar como `unhealthy`.
- `start_period`: tempo de carência na inicialização, em que falhas não contam.

## Esperar um serviço ficar saudável

O `depends_on` por padrão só espera o container **iniciar**. Para esperar o healthcheck passar, use `condition`:

```yaml
services:
  app:
    build: .
    depends_on:
      db:
        condition: service_healthy
```

## Verificar o estado

```bash
docker ps                                                    # coluna STATUS mostra (healthy)
docker inspect --format '{{json .State.Health}}' <container>  # histórico das verificações
```
