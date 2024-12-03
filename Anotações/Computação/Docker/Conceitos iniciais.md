## Conceitos iniciais de Docker

**Imagem Docker**: Um conjunto de camadas independentes que ao serem agrupadas que geram determinada regra de execução do contêiner.

**Docker reference** - documentação online do Docker.
### Comandos iniciais
- `docker run <nome da imagem>` cria um novo contêiner com base na imagem fornecida e executa-o.
- `docker run <nome da imagem> -d` cria um novo contêiner e executa-o sem travar o terminal.
- `docker run -it <nome da imagem> <comando>` cria um novo contêiner e executa-o de forma interativa.
- `docker run -P <nome da imagem> <comando>` cria um novo contêiner e executa-o realizando um mapeamento automático entre as portas do contêiner e da máquina.
- `docker run -p <porta host>:<porta contêiner> <nome da imagem> <comando>` cria um novo contêiner executa-o realizando um mapeamento manual entre as portas do contêiner e da máquina.

- `docker start <id/nome contêiner>` executa um contêiner inativo.
- `docker stop <id/nome contêiner>` para a execução de um contêiner ativo.

- `docker pause <id/nome contêiner>` pausa a execução de um contêiner ativo.
- `docker unpause <id/nome contêiner>` reinicia um contêiner pausado.

#### Executando comandos em um contêiner
- `docker exec -it <id/nome contêiner> <comando>` executa um comando em um determinado contêiner de forma interativa.
- `docker exec -it <container_id_or_name> /bin/bash` executar o terminal de um contêiner baseado em linux.

- `docker rm <id/nome contêiner>` remove um determinado contêiner.
- `docker rmi <id/nome imagem:versão>` remove uma determinada imagem.

- `docker ps/container ls` lista de todos os contêineres em execução. 
- `docker ps/container ls -a` lista de todos os contêineres, incluindo inativos.

- `docker images/image ls` - lista as imagens baixadas localmente.
- `docker history <id imagem>` informações detalhadas sobre todas as camadas de uma imagem.

- `docker inspect <id/nome de um objeto Docker>` trás uma série de informações detalhadas do objeto.

- `docker build -t <nome do contêiner:versão> <caminho Dockerfile>` cria uma nova imagem através de um Docker file.