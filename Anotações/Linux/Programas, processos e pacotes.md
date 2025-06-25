
[[0. Linux|Início]]

## Processos
- `ps` Lista todos os processos rodando no terminal atual.
- `ps -e` Lista todos os processos rodando no sistema.
- `ps -ef` Lista todos os processos rondando no sistema em formato detalhado.
- `ps -ef | grep <nome do processo>` Utilizar pipe `|` para redirecionar a saída da lista de processos para o processo grep que realiza uma filtragem de linhas baseada em padrões de texto, nesse caso o padrão fornecido foi o nome de um processo.
- `pstree` Lista uma árvore hierárquica de todos os processos rodando.
## Processos em segundo plano
- `jobs` Lista os processos em segundo plano (trabalhos) em execução no terminal.
- `jobs -p` Lista o id de todos os processos em segundo plano no terminal.
- `bg <número do trabalho>` Executa um trabalho parado no background.
- `fg <número do trabalho>` Executa um trabalho parado no foreground.
- `<nome do processo> &` Executa um processo diretamente no background.
## Sinais de processos
> No Linux os processos utilizam sinais para se comunicar e o sistema operacional pode utilizá-los para interferir nesses processos.
- `kill -STOP/CONT/TERM/KILL <id do processo>` Alguns exemplos de sinais que podem ser enviados.
- `kill <id do processo>` Solicita ao processo que o mesmo seja encerrado.
- `kill - 9 <id do processo>` Força a finalização do processo sem chance para realizar o processo de forma ordenada.
- `killall <nome do processo>` Mesmo funcionamento do kill, porém sendo aplicado a todos os processos que possuam determinado nome.
- `kill -9 $(jobs -p)` Força a finalização de todos as jobs executando no terminal.
## Localizar processos
- `locate <nome do processo>` Lista todos os arquivos que possuam o determinado nome.
- `sudo updatedb` Força uma atualização no banco de dados interno do Linux que mantém o registro da localização de todos os arquivos no sistema operacional.
- `which <nome do processo>` Mostra a localização do arquivo de um comando.
## Recursos
- `top` Lista detalha do consumo de recursos do sistema por cada processo.
- `top -u <nome do usuário>` Lista apenas os processos de um determinado usuário.
- `top -p <id do processo>` Acompanhar apenas um determinado processo.
## Serviços
> Para cada processo que inicia ao startup do sistema existe um script de inicialização contido no diretório `/etc/init.d/`.
- `service <nome do servico> stop` Enviar um sinal para qualquer serviço (qualquer processo que inicia no startup do sistema).
## Variáveis de ambiente
Através da variável `PATH` o terminal procura o programa a ser executado. É possível incluir novos diretórios junto a `PATH`, porém alterações feitas em um terminal só ficam restritas a ele. Para evitar isso podemos adicionar novas variáveis no arquivo de inicialização do terminal, o `.bashrc`. Cada novo terminal iniciado primeiro aplica as configurações contidas no `.bashrc`.
- `env` Lista todas as variáveis de ambiente do sistema.
## Gerenciador de pacotes
### apt
> O `apt` é o sistema de gerenciamento de pacotes padrão do Ubuntu. Utilizar o gerenciador de pacotes apt exige privilégios de administrador.

- `apt-get update` Verifica a existência de novas atualizações nos pacotes, porém não baixa e instala nenhuma atualização.
- `apt-get upgrade` Baixa e instala novas versões dos pacotes já instalados no sistema.
- `apt-get install <nome do pacote>` Instala um determinado pacote.
- `apt-get remove <nome do pacote>` Remove um determinado pacote.
- `apt-cache search <nome do pacote>` Trás uma lista de pacotes baseada na pesquisa feita.
### dpkg
> O `dpkg` é o gerenciador de pacotes local. Utilizado para fazer a instalação de pacotes `.deb`.

- `dpkg -i <nome do pacote>.deb` Instalar um determinado pacote local.
- `dpkg -r <nome do pacote>` Remover um programa instalado através do nome do pacote.
## Instalar programas
### App image
É possível instalar programas através de uma imagem. Para isso você primeiro precisa obviamente da imagem. Considere a imagem como um executável para instalar o arquivo, porém por padrão este executável não tem permissão para ser executado.

Usando o comando `chmod`pode modificar as permissões padrões de um arquivo podemos executá-lo e por fim instalar o programa.
### Pacotes
Via `apt` quando o programa já está disponibilizado na central de pacotes do OS Linux.
Via `dpkg` quando baixamos pelo navegador da internet um pacote `.deb` do programa.
### Código fonte
É possível instalar um programa através do código fonte. Nessa situação é preciso baixar seu código fonte, compilá-lo e instalá-lo.
A recomendação é baixar o código fonte compactado no formato `tar.gz` para que suas permissões de execução sejam mantidas. Para descompactá-lo basta: `tar -xf <nome do arquivo>.tar.gz`.
É possível que o programa exija algumas dependências para rodar, um padrão comum é a existência de um script `configure` que faz a verificação das dependências no sistema.
Assumindo que estamos instalando um programa escrito em C, usaremos o comando `make` para compilar o projeto. É possível que erros de dependências aconteçam nesse momento. É preciso lidar com eles de maneira individual.
Caso tudo dê certo podemos enfim instalar o programa através do `make install`.

Portanto, existem basicamente três passos para instalar um programa a partir de seu código fonte:
1. `./configure` para verificar as dependências e configurações da máquina.
2. `make` para gerar o programa, ou seja, compilar. Lembrando que, neste passo, pode haver outras dependências necessárias para a tarefa e por isso talvez seja preciso realizar instalações de outras bibliotecas.
3. `sudo make install` para que o programa seja instalado em nossa máquina. 

