#Git 

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

## Remover arquivos ignorados

Se houver arquivos ignorados (`.gitignore`), eles não são removidos por padrão. Para excluir arquivos ignorados também:

```bash
git clean -fx
```

## Remover arquivos não rastreados e ignorados juntos

```bash
git clean -fdx
```
