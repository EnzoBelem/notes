#Linux

Para resolver o problema de downtime e garantir que seu servidor fique sempre disponível durante a atualização, você pode adotar uma estratégia de "blue-green deployment" com Docker. A ideia é ter dois containers (blue e green), onde um deles está em execução (ativo) e o outro é atualizado e preparado para substituir o atual. Uma vez que a nova versão está pronta, você altera o roteamento para apontar para o container atualizado. Aqui estão os passos para implementar isso:

### Passo 1: Criação do Novo Container

1. **Script de Deploy**: Atualize seu script para criar um novo container (green) ao invés de derrubar o existente (blue).
2. **Port Binding Temporário**: O novo container pode ser configurado para rodar em uma porta temporária.

```bash
#!/bin/bash

# Nome do container original
ORIGINAL_CONTAINER_NAME="my_server_blue"
# Novo nome do container
NEW_CONTAINER_NAME="my_server_green"
# Nome da imagem
IMAGE_NAME="my_image"
# Porta temporária
TEMP_PORT=8081
# Porta original
ORIGINAL_PORT=8080

# Pull do código mais recente
git pull origin main

# Construir nova imagem
docker build -t $IMAGE_NAME .

# Rodar novo container na porta temporária
docker run -d --name $NEW_CONTAINER_NAME -p $TEMP_PORT:80 $IMAGE_NAME
```

### Passo 2: Verificação do Novo Container

Certifique-se de que o novo container está funcionando corretamente antes de redirecionar o tráfego.

```bash
# Verificar se o novo container está rodando
if curl -s http://localhost:$TEMP_PORT | grep "Expected Content"; then
    echo "Novo container está funcionando corretamente"
else
    echo "Novo container apresentou erro, encerrando script"
    docker rm -f $NEW_CONTAINER_NAME
    exit 1
fi
```

### Passo 3: Atualização do Roteamento

1. **Derrubar o Container Original**: Pare e remova o container original.
2. **Iniciar o Novo Container na Porta Original**: Altere o novo container para a porta original.

```bash
# Parar e remover o container original
docker stop $ORIGINAL_CONTAINER_NAME
docker rm $ORIGINAL_CONTAINER_NAME

# Iniciar o novo container na porta original
docker run -d --name $ORIGINAL_CONTAINER_NAME -p $ORIGINAL_PORT:80 $IMAGE_NAME

# Opcional: Remover container temporário
docker rm -f $NEW_CONTAINER_NAME
```

### Passo 4: Limpeza

Após garantir que o novo container está funcionando corretamente, você pode limpar qualquer recurso temporário.

```bash
# Limpeza de containers antigos (opcional)
docker system prune -f
```

### Script Completo

Aqui está o script completo:

```bash
#!/bin/bash

# Nome do container original
ORIGINAL_CONTAINER_NAME="my_server_blue"
# Novo nome do container
NEW_CONTAINER_NAME="my_server_green"
# Nome da imagem
IMAGE_NAME="my_image"
# Porta temporária
TEMP_PORT=8081
# Porta original
ORIGINAL_PORT=8080

# Pull do código mais recente
git pull origin main

# Construir nova imagem
docker build -t $IMAGE_NAME .

# Rodar novo container na porta temporária
docker run -d --name $NEW_CONTAINER_NAME -p $TEMP_PORT:80 $IMAGE_NAME

# Verificar se o novo container está rodando
if curl -s http://localhost:$TEMP_PORT | grep "Expected Content"; then
    echo "Novo container está funcionando corretamente"
    
    # Parar e remover o container original
    docker stop $ORIGINAL_CONTAINER_NAME
    docker rm $ORIGINAL_CONTAINER_NAME

    # Iniciar o novo container na porta original
    docker run -d --name $ORIGINAL_CONTAINER_NAME -p $ORIGINAL_PORT:80 $IMAGE_NAME

    # Remover container temporário
    docker rm -f $NEW_CONTAINER_NAME
else
    echo "Novo container apresentou erro, encerrando script"
    docker rm -f $NEW_CONTAINER_NAME
    exit 1
fi

# Limpeza de containers antigos (opcional)
docker system prune -f
```

Este script automatiza o processo de atualização sem downtime significativo, garantindo que sempre haja um container ativo enquanto o novo é preparado.