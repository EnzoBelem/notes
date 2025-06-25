
[[0. Linux|Início]]

## Parâmetros coringas na execução de comandos
- `-r --recursive` - executar o comando de forma recursiva.
- `-q --quiet` - executar o comando sem saída final.
- `-v --verbose` - executar o comando trazendo ainda mais informações na saída final.
## Comandos gerais
- `man command` Comando de manual do Linux, trás a documentação do comando.
- `pwd` Caminho do diretório atual.
- `date` Retorna a data atual do sistema.
- `sh <nome script>`  Executa um determinado arquivo no terminal.
## Usuário do sistema
- `whoami` Informa o nome do usuário no Linux.
- `passwd` Altera a senha do usuário atual.
- `sudo passwd` Altera a senha do usuário root.
- `su <nome do usuário>` Loga como o determinado usuário.
- `sudo su` Passando apenas o comando `su` iremos logar como usuário root.
- `adduser <nome usuário>` Adiciona um novo usuário ao sistema.
## Navegar por diretórios
- `cd` Vai para o diretório home do usuário.
- `cd /` Vai para o diretório raiz do sistema.
- `cd "caminho_diretorio/"` Trocar o diretório atual.
- `cd ..` Voltar para o diretório anterior.
## Listar conteúdos de um diretórios
- `ls` Listar o conteúdo do diretório atual.
- `ls -l` Listar os conteúdos com mais informações.
- `ls -a` Listar os conteúdos, incluindo invisíveis.
- `ls dir_name/` Listar os conteúdos internos de um diretório especificado.
## Imprimir dados no terminal
- `echo "mensagem"` Imprimir uma mensagem no terminal.
- `echo "mensagem" > file_name.txt` Redirecionar a saída do comando echo para um arquivo.
- `echo "mensagem" >> file_name.txt` Redirecionar a saída do comando echo e concatenar ao conteúdo de um arquivo.
## Ler conteúdos no terminal
- `cat file.txt` Ler o conteúdo de um arquivo.
- `cat file?.txt` Ler o conteúdo dos arquivo com o padrão `file<caractere>.txt`.
- `cat file*.txt` Ler o conteúdo dos arquivos com `file<qualquer número de caracteres>.txt`.
- `cat *.txt` Ler o conteúdo dos arquivos com a extensão `.txt`.
## Manipular diretórios e arquivos
### Diretórios
- `mkdir "dir_name"` Criar um novo diretório.
- `rmdir dir_name` Remover diretório vazio.
- `rm -r dir_name` Remover diretório recursivamente (remover tudo).
- `cp -r origin_dir destiny_dir` Copiar um diretório recursivamente.
### Arquivos
- `toach file_name.txt` Tocar um arquivo alvo realizando uma alteração na data de modificação, porém pode ser usado para criar um novo arquivo.
- `cp origin_file.txt destiny_file.txt` Copiar um arquivo.
- `mv origin_file.txt destiny_file.txt` Mover um arquivo.
- `rm file_name.txt` Remover um arquivo.
- `head file_name.txt` Lê apenas um número fixo de dez linhas de um arquivo.
- `head -n 3 file_name.txt` Lê um número especifico de linhas de um arquivo.
- `tail` Mesmo funcionamento do comando `head` porém lê as linhas finais de um arquivo.
- `less file_name.txt` Abre o arquivo permitindo o uso da seta no terminal, possui uma visualização mais adequada no conteúdo no terminal.
#### Permissões de arquivos

> No Linux os arquivos tem três tipos de permissão:  `r` Leitura, `w` Escrita e `x` Execução.
> Permissões variam a depender do tipo de usuário: `u` Dono, `g` Grupo e `o` Outros

- `chmod <u/g/o> +/- <permissão especifica> <nome_arquivo>` Modifica as permissões de um determinado arquivo.
## Compactar e descompactar
### zip
- `zip dir_name.zip dir_name` Compactar o diretório no arquivo `.zip` especificado.
- `zip -r dir_name.zip dir_name` Compactar o diretório de forma completa usando recursividade no arquivo `.zip` especificado.
- `unzip zip_file.zip` Descompactar um arquivo `.zip`.
- `unzip -l zip_file.zip` Listar o conteúdo do arquivo `.zip`.
### tar
O `tar` não compacta, apenas empacota os arquivos, então é possível utilizá-lo com outros formatos de compactação. Um exemplo é `bzip2` usando `-j`.
- `tar -cz dir_name > tar_file.tar.gz` Criar arquivo `.tar.gz`.
- `tar -xz < tar_file.tar.gz` Descompactar um arquivo `.tar.gz`.
- `tar -czf tar_file.tar.gz dir_name` Criar um arquivo compactado com uma sintaxe que não envolva direcionamento.
#### Parâmetros
- `-c` Criar um arquivo do tipo `tar`.
- `-x` Extrair um arquivo do tipo `tar`.
- `-f` Informar o nome do arquivo `tar` alvo.
- `-z` Zipar o arquivo que será compactado usando `tar`.
## Editores de texto no terminal
1. **nano**: Este é um editor de texto simples e fácil de usar. Para editar um arquivo chamado `meu_arquivo.txt`, você usaria:
```bash
nano meu_arquivo.txt
```
2. **vi/vim**: O `vi` é um editor de texto mais poderoso, mas também mais complexo. O `vim` é uma versão melhorada do `vi`. Para usar o `vi` ou `vim`, você faria:
```bash
vim meu_arquivo.txt
```
3. **emacs**: Este é outro editor de texto poderoso disponível no Ubuntu. Para usar o `emacs`, você faria:
```bash
emacs meu_arquivo.txt
```
