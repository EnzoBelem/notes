
[[0. Linux|Início]]

Para executar um comando seguido de um `sudo su`, você pode utilizar a funcionalidade de sub shell do bash. Aqui estão algumas abordagens para conseguir isso:

### Abordagem 1: Usar um sub shell
Você pode usar um subshell para executar os comandos sequencialmente dentro do contexto do `sudo su`. Por exemplo:

```bash
comando1 && sudo su -c 'comando2'
```

Neste caso, `comando1` será executado, e se tiver sucesso, `sudo su` será executado com `comando2` dentro de um subshell.

### Abordagem 2: Usar um script
Você pode criar um script bash contendo os comandos que deseja executar e então usar `sudo su` para executar o script:

1. Crie um script, por exemplo `meu_script.sh`:
   ```bash
   #!/bin/bash
   comando1
   comando2
   ```

2. Torne o script executável:
   ```bash
   chmod +x meu_script.sh
   ```

3. Execute o script usando `sudo su`:
   ```bash
   sudo su -c './meu_script.sh'
   ```

### Abordagem 3: Usar múltiplos comandos com `sudo su`
Você pode passar múltiplos comandos diretamente para `sudo su` usando o operador `&&` ou `;`:

```bash
sudo su -c 'comando1 && comando2'
```
ou
```bash
sudo su -c 'comando1; comando2'
```

### Exemplos Práticos

1. **Executar um comando antes de `sudo su` e outro depois:**
   ```bash
   echo "Executando primeiro comando" && sudo su -c 'echo "Executando comando como root"'
   ```

2. **Criar um diretório como usuário normal e mudar permissões como root:**
   ```bash
   mkdir minha_pasta && sudo su -c 'chmod 700 minha_pasta'
   ```

Essas abordagens permitem que você execute comandos em sequência com a mudança de contexto para o usuário root utilizando `sudo su`.