#Git 

O comando `git diff` é uma ferramenta poderosa no Git usada para **comparar estados do código** em diferentes momentos ou locais (working directory, staging area, commits etc). Abaixo estão as principais utilidades e variações do comando, explicadas de forma prática:

## 1. Comparar alterações não preparadas (working directory vs último commit)

```bash
git diff
```

Mostra as mudanças feitas **ainda não adicionadas com `git add`**.

## 2. Comparar alterações preparadas (staging area vs último commit)

```bash
git diff --cached
```

Ou:

```bash
git diff --staged
```

Mostra o que já foi adicionado com `git add`, mas ainda **não foi commitado**.

## 3. Comparar dois commits

```bash
git diff commit1 commit2
```

Exemplo:

```bash
git diff 3a4b1f2 HEAD
```

Mostra as diferenças entre dois commits específicos.

## 4. Comparar branches

```bash
git diff main feature-xyz
```

Mostra as diferenças entre o que está em `main` e o que está em `feature-xyz`.

## 5. Comparar arquivos entre branches ou commits

```bash
git diff branch1:file.txt branch2:file.txt
```

Útil para ver o que mudou em um arquivo específico entre duas branches ou commits.

## 6. Mostrar mudanças com mais contexto

```bash
git diff -U10
```

Mostra 10 linhas de contexto ao redor das mudanças (útil para revisão).

## 7. Resumo das mudanças (sem conteúdo)

```bash
git diff --stat
```

Exemplo de saída:

```
 src/app.py  | 10 +++++-----
 README.md   |  2 +-
```

Mostra arquivos alterados, número de linhas adicionadas/removidas.

## 8. Mostrar mudanças com realce de palavras

```bash
git diff --color-words
```

Destaque inline das palavras modificadas, útil para pequenas alterações.

## 9. Comparar com `origin` (remoto)

```bash
git diff origin/main
```

Mostra as diferenças entre o que você tem localmente e o que está no repositório remoto.

## 10. Comparar diretórios

```bash
git diff --name-only
```

Mostra **apenas os nomes dos arquivos** que foram modificados.

## 11. Ver diferenças numéricas compactas

```bash
git diff --shortstat
```

Resumo curto de linhas adicionadas/removidas, sem detalhar por arquivo.

## 12. Ignorar espaços em branco

```bash
git diff -w
```

Ignora mudanças de formatação (espaços, tabs) ao comparar.
