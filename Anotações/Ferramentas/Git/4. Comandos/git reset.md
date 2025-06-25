#Git 

O comando `git reset` é uma ferramenta poderosa e potencialmente destrutiva no Git. Ele altera o ponteiro `HEAD`, podendo também modificar a área de staging (index) e o diretório de trabalho. É usado principalmente para **desfazer commits**, **desfazer `git add`** e **reescrever histórico local**.

## Modos de uso do `git reset`

Existem **três modos principais** de `reset`: `--soft`, `--mixed` (padrão) e `--hard`. Cada um afeta o ambiente de trabalho de maneira diferente.

### 1. `git reset --soft <commit>`

- Move o ponteiro `HEAD` para o commit indicado.
    
- **Preserva** a área de staging e o diretório de trabalho.
    
- Ideal para reescrever o histórico sem perder alterações.
    

```bash
git reset --soft HEAD~1
```

Significa: volte um commit, mas mantenha tudo no stage (`git add`) como estava.

### 2. `git reset --mixed <commit>` (padrão)

- Move o `HEAD` para o commit indicado.
    
- **Limpa a área de staging** (index), mas preserva as mudanças no diretório de trabalho.
    
- É o comportamento padrão se não especificar nenhum modo.
    

```bash
git reset HEAD~1
```

Significa: volte um commit, e remova os arquivos do stage, deixando as alterações no diretório de trabalho.

### 3. `git reset --hard <commit>`

- Move o `HEAD` para o commit indicado.
    
- **Limpa a área de staging** e o **diretório de trabalho**.
    
- Todas as mudanças **são perdidas permanentemente** (se não estiverem salvas em outro lugar).
    

```bash
git reset --hard HEAD~1
```

Significa: volte um commit, apague do stage e também descarte as alterações locais.

## Outras formas comuns de uso

### Remover arquivos do stage (sem alterar o conteúdo):

```bash
git reset HEAD caminho/arquivo.py
```

Útil para desfazer um `git add` feito por engano.

## Considerações importantes

- `git reset` **só deve ser usado localmente**. Nunca em commits que já foram enviados (push), pois reescreve o histórico.
- Para desfazer commits compartilhados, use `git revert`.    
- `git reflog` pode ajudar a recuperar commits perdidos após um reset errado.
