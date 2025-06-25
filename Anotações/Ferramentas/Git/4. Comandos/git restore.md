#Git 

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

## Restaurar um arquivo que já estava na zona de preparação

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
