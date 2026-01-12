#Linux 

É possível acessar outra máquina remotamente através do protocolo `ssh`. 

## Configurar o ambiente
### Instalar pacote ssh
Para utilizarmos o protocolo como cliente e servidor precisamos instalar o pacote, no através do comando: 
```bash
sudo apt-get install ssh
```
### Gerar chave ssh
Você pode criar uma chave ssh com o seguinte comando:
``` bash
ssh-keygen
```
### Gerenciando chaves permitidas na máquina
As chaves aceitas em sua máquina estão salvas no arquivo:
```bash
 ~/.ssh/authorized_keys
```
## Conectar remotamente
- `ssh <nome do usuario>@<ip da máquina remota>` Realiza uma conexão remota via ssh.
- `ssh -X <nome do usuario>@<ip da máquina remota>` A conexão remota também irá executar a parte gráfica reproduzindo tudo na máquina local.
- `ssh -i <caminho da chave pública> <nome do usuario>@<ip da máquina remota>` Conexão informando o caminho da chave pública que deve ser fornecida para autenticação.
- `scp <nome do arquivo> <nome do usuario>@<ip da máquina remota>:<diretório remoto>` Copiar um arquivo da máquina local para a remota.

### Executar scripts remotamente
É possível executar um script existente em sua máquina local em uma máquina remota sem copiá-lo usando o SSH. Você pode fazer isso redirecionando o conteúdo do script através de SSH e executando-o remotamente.

Aqui está um exemplo de como fazer isso:

```bash
ssh usuário@host_remoto 'bash -s' < seu_script.sh
```

Isso executa o script `seu_script.sh` na máquina remota usando o interpretador bash. O conteúdo do script é passado através do SSH e executado remotamente.

