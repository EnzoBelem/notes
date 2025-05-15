
#Git

[[0. Git]]

O Git oferece comandos específicos para lidar com alterações no diretório de trabalho e limpar arquivos indesejados. Dois desses comandos importantes são:

- `git restore` → Para reverter mudanças em arquivos modificados.
- `git clean` → Para remover arquivos não rastreados do repositório.

---

# `git restore` - Restaurando Arquivos Modificados

O comando `git restore` é usado para **desfazer alterações em arquivos no diretório de trabalho**, retornando à última versão registrada.

## Restaurar um arquivo modificado para a última versão confirmada

```bash
git restore <arquivo>
```

Isso descarta todas as alterações feitas no arquivo e o retorna ao último commit.

## Restaurar todos os arquivos modificados

```bash
git restore .
```

Isso remove todas as alterações nos arquivos rastreados no diretório de trabalho.

## Restaurar um arquivo que já estava na zona de preparação (`staging area`)

Se você já adicionou um arquivo à zona de preparação com `git add`, mas quer removê-lo de lá sem perder as alterações:

```bash
git restore --staged <arquivo>
```

Isso tira o arquivo da zona de preparação e o mantém modificado no diretório de trabalho.

## Restaurar um arquivo do último commit

Se quiser desfazer todas as mudanças e voltar ao último commit:

```bash
git restore --source=HEAD --staged --worktree <arquivo>
```

Isso remove as alterações do diretório de trabalho e da zona de preparação.

---

# `git clean` - Removendo Arquivos Não Rastreáveis

O comando `git clean` é usado para **deletar arquivos que não estão rastreados pelo Git**, ou seja, arquivos novos que ainda não foram adicionados ao repositório.

## Verificar quais arquivos serão removidos

Antes de executar o comando, sempre é bom conferir o que será apagado:

```bash
git clean -n
```

Isso faz um **simulado**, mostrando quais arquivos seriam removidos, sem apagá-los.

## Remover arquivos não rastreados

```bash
git clean -f
```

O parâmetro `-f` (**force**) força a remoção dos arquivos listados.

## Remover diretórios não rastreados

Se houver diretórios não rastreados, o comando acima não os removerá. Para limpar diretórios também:

```bash
git clean -fd
```

## Remover arquivos ignorados pelo `.gitignore`

Se houver arquivos ignorados (`.gitignore`), eles não são removidos por padrão. Para excluir arquivos ignorados também:

```bash
git clean -fx
```

## Remover arquivos não rastreados e ignorados juntos

```bash
git clean -fdx
```

---

## Atenção ao usar `git clean`!

- **Não há como recuperar arquivos removidos por `git clean`**, pois eles nunca foram rastreados pelo Git.
- Sempre use `git clean -n` antes de executar a versão final do comando.

---

# Resumo

|Comando|Função|
|---|---|
|`git restore <arquivo>`|Restaura um arquivo modificado para o último commit.|
|`git restore --staged <arquivo>`|Remove um arquivo da zona de preparação sem apagar as alterações.|
|`git restore .`|Restaura **todos** os arquivos modificados para a versão do último commit.|
|`git clean -n`|Simula a remoção de arquivos não rastreados.|
|`git clean -f`|Remove arquivos não rastreados.|
|`git clean -fd`|Remove arquivos e diretórios não rastreados.|
|`git clean -fx`|Remove arquivos não rastreados, incluindo os ignorados pelo `.gitignore`.|
