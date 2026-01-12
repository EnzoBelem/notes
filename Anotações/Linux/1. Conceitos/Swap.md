#Linux 

## Importância do swap
O arquivo de swap em um sistema Linux tem um papel crucial, especialmente quando o sistema está sobrecarregado. Aqui estão alguns pontos importantes:
1. **Gerenciamento de Memória**: O arquivo de swap é usado quando a quantidade de memória física (RAM) é totalmente utilizada. Quando isso acontece, o sistema operacional move alguns dos dados na RAM para o arquivo de swap. Isso libera a RAM para que ela possa ser usada para outras tarefas.
2. **Prevenção de Falhas do Sistema**: Sem um arquivo de swap, se a RAM ficar completamente cheia, novos processos podem falhar ou, em casos extremos, o sistema pode travar. Com um arquivo de swap, o sistema pode continuar a funcionar mesmo que a RAM esteja cheia.
3. **Desempenho**: Embora o acesso ao arquivo de swap seja mais lento do que o acesso à RAM, ter um arquivo de swap pode melhorar o desempenho geral do sistema. Isso ocorre porque o sistema operacional pode mover os dados que não estão sendo usados ativamente para o arquivo de swap, deixando mais RAM disponível para os processos que precisam dela.
4. **Hibernação**: Em alguns sistemas Linux, o arquivo de swap é usado para armazenar o estado atual do sistema quando ele entra em hibernação. Quando o sistema é reiniciado, ele lê o conteúdo do arquivo de swap de volta para a RAM, permitindo que o sistema retome de onde parou.
## Definindo o tamanho ideal para o swap
The amount of swap you need is highly variable based on _your_ specific applications and workload. There is no universal formula on swap size without monitoring usage over a period of time. A reasonable place to start would be:
- For less then 4GB of physical memory (RAM), it's highly recommended that the swap space should, as a base minimum, be equal to the amount of RAM. Also, it's recommended that the swap space is maximum twice the amount of RAM depending upon the amount of disk space available for the system because of diminishing returns.
- For more modern systems (>4GB), your swap space should be at a minimum be ROUNDUP(SQRT(RAM)) I.E. the square root of your RAM size rounded up to the next GB. **However, if you use hibernation**, you need a minimum of physical memory (RAM) size **plus** ROUNDUP(SQRT(RAM)). The maximum, is again twice the amount of RAM, again because of diminishing returns.
- The only downside to having more swap space than you will actually use, is the disk space you will be reserving for it cannot be used for application or system data.
## Criar arquivo de swap
### Verificar swaps ativos
Antes de criar um arquivo de swap é preciso verificar sem já existem swaps ativos:
```shell
swapon --show
```
### Desativar o swap existente
Caso já exista um outro arquivo de swap é preciso desativa-lo:
```shell
sudo swapoff /swapfile
```
### Criar um arquivo
Você pode criar um arquivo com o comando `dd`. Por exemplo, para criar um arquivo com tamanho de 4 GB:
``` bash
sudo dd if=/dev/zero of=/swapfile bs=1G count=4
```
### Definir as permissões corretas no arquivo
Por razões de segurança, o arquivo de swap deve ser acessível apenas pelo usuário root. Use o comando `chmod` para definir as permissões corretas:
``` bash
sudo chmod 600 /swapfile
```
### Formatar o arquivo para swap
Use o comando `mkswap` para formatar o arquivo para swap:
``` bash
sudo mkswap /swapfile
```
### Ativar o swap
Agora, você pode ativar o swap com o comando `swapon`:
``` bash
sudo swapon /swapfile
```
### Ativar o swap na inicialização
Para ativar o swap na inicialização, adicione a seguinte linha ao arquivo `/etc/fstab`:
``` txt
/swapfile swap swap defaults 0 0
```
## Criar uma partição de swap
### Criar partição no disco
Crie uma partição em seu disco com um sistema de arquivos como `ext4`.
### Formatar partição para swap
### Formatar o arquivo para swap
Use o comando `mkswap` para formatar a partição para swap:
``` bash
sudo mkswap <particion_path>
```
### Ativar o swap
Agora, você pode ativar o swap com o comando `swapon`:
``` bash
sudo swapon <particion_path>
```
### Ativar o swap na inicialização
Descubra o `UUID`  da sua partição de swap:
``` bash
sudo lsblk -no UUID <particion_path>
```
Para ativar o swap na inicialização, adicione a seguinte linha ao arquivo `/etc/fstab`:
``` txt
UUID=<uuid> none swap defaults 0 0
```