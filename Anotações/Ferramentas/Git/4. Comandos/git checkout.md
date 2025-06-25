#Git 

O comando `git checkout` é um dos mais versáteis do Git, tem **duas funções principais**, dependendo do contexto:

## 1. Trocar de branch ou commit (modo de navegação)

```bash
git checkout nome-da-branch
```

Este uso muda o diretório de trabalho (`working directory`) para refletir o estado da branch ou do commit especificado. O ponteiro `HEAD` é atualizado para apontar para essa nova referência.

### Exemplos

```bash
git checkout main              # Troca para a branch main
git checkout feature/login     # Troca para a branch feature/login
git checkout 4e3d2a1           # Troca para um commit específico (modo detached HEAD)
```

**Nota:** ao fazer checkout de um commit específico (sem ser uma branch), o Git entra em modo _detached HEAD_. Se você fizer commits nesse estado, eles não estarão ligados a nenhuma branch e podem ser perdidos se não forem salvos em uma nova branch.

## 2. Restaurar arquivos específicos (modo de restauração)

```bash
git checkout origem -- caminho/para/arquivo
```

Esse uso permite restaurar arquivos específicos para o estado que estavam em determinado commit, branch ou tag. Os arquivos são sobrescritos no diretório de trabalho.

### Exemplos:

```bash
git checkout HEAD -- app.py             # Restaura app.py para o último commit
git checkout main -- requirements.txt   # Recupera a versão de main desse arquivo
git checkout 4e3d2a1 -- src/utils.py    # Recupera versão antiga de utils.py
```

**Atenção:** esse comando substitui o conteúdo do(s) arquivo(s) diretamente no disco, sem passar pelo staging (`git add`). Use com cuidado, pois ele pode sobrescrever mudanças não salvas.

## Considerações

Desde o Git 2.23, o comando `git checkout` passou a ser dividido em dois comandos mais específicos para clareza:
- `git switch` → para trocar de branch
- `git restore` → para restaurar arquivos