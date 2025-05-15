
[[0. Linux|Início]]

O comando `chmod` no Linux é usado para alterar as permissões de arquivos e diretórios. As permissões podem ser definidas usando notação simbólica (letras) ou octal (números). Aqui está uma explicação detalhada de ambas as notações:

### Notação Simbólica

Na notação simbólica, você usa letras para representar as permissões e operadores para defini-las:

- **Letras que representam as permissões:**
  - `r` : leitura
  - `w` : escrita
  - `x` : execução

- **Letras que representam quem recebe as permissões:**
  - `u` : usuário (owner)
  - `g` : grupo
  - `o` : outros
  - `a` : todos (user, group, e others)

- **Operadores:**
  - `+` : adiciona permissão
  - `-` : remove permissão
  - `=` : define permissão exatamente

#### Exemplos

- Adicionar permissão de execução para o usuário:
  ```bash
  chmod u+x arquivo
  ```

- Remover permissão de escrita do grupo:
  ```bash
  chmod g-w arquivo
  ```

- Definir permissão de leitura para todos:
  ```bash
  chmod a=r arquivo
  ```

### Notação Octal

Na notação octal, as permissões são representadas por números de 0 a 7, onde cada dígito representa um conjunto de permissões. As permissões são calculadas somando os valores:

- **4** : leitura (`r`)
- **2** : escrita (`w`)
- **1** : execução (`x`)

Cada dígito da notação octal representa as permissões do usuário, do grupo e dos outros, nessa ordem.

#### Exemplos

- **7** : leitura, escrita e execução (`rwx` = 4 + 2 + 1 = 7)
- **6** : leitura e escrita (`rw-` = 4 + 2 = 6)
- **5** : leitura e execução (`r-x` = 4 + 1 = 5)
- **4** : leitura (`r--` = 4)
- **3** : escrita e execução (`-wx` = 2 + 1 = 3)
- **2** : escrita (`-w-` = 2)
- **1** : execução (`--x` = 1)
- **0** : nenhuma permissão (`---` = 0)

#### Exemplos

- Definir permissões `rwxr-xr-x` (leitura, escrita e execução para o usuário; leitura e execução para grupo e outros):
  ```bash
  chmod 755 arquivo
  ```

- Definir permissões `rw-r--r--` (leitura e escrita para o usuário; leitura para grupo e outros):
  ```bash
  chmod 644 arquivo
  ```

- Definir permissões `rwx------` (leitura, escrita e execução apenas para o usuário):
  ```bash
  chmod 700 arquivo
  ```

### Tabela de Permissões

Aqui está uma tabela para referência rápida:

| Permissão | Simbólica | Octal |
|-----------|-----------|-------|
| `---`     | nenhum    | 0     |
| `--x`     | execução  | 1     |
| `-w-`     | escrita   | 2     |
| `-wx`     | escrita e execução | 3 |
| `r--`     | leitura   | 4     |
| `r-x`     | leitura e execução | 5 |
| `rw-`     | leitura e escrita | 6 |
| `rwx`     | leitura, escrita e execução | 7 |

### Exemplos de Uso do `chmod`

- Tornar um script executável apenas pelo usuário:
  ```bash
  chmod 700 script.sh
  ```

- Permitir que todos leiam um arquivo, mas apenas o usuário pode modificar:
  ```bash
  chmod 644 arquivo.txt
  ```

- Permitir que o usuário e o grupo modifiquem um arquivo, mas outros apenas leiam:
  ```bash
  chmod 664 arquivo.txt
  ```

Essas são as maneiras básicas de usar o `chmod` para definir permissões de arquivos e diretórios no Linux.