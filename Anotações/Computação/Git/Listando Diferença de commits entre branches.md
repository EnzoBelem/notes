Para identificar os commits que são **exclusivos da sua branch**, ou seja, que **não estão na `master`**, mesmo após merges. Aqui estão algumas abordagens úteis com `git`:

---

### ✅ **1. Mostrar commits exclusivos da sua branch (em relação à master)**

```bash
git log --oneline master..sua-branch
```

Esse comando lista todos os commits que estão em `sua-branch` e **não estão em `master`**. Isso é útil quando a `master` já avançou e você quer saber o que **sua branch adicionou de fato**.

---

### ✅ **2. Mostrar commits únicos mesmo após merge com master**

Se você fez um merge da `master` para sua branch, isso pode poluir o histórico. Para filtrar commits realmente exclusivos (não compartilhados com `master`), use:

```bash
git log --oneline --no-merges master..sua-branch
```

Ou, se quiser ver mesmo commits de merge feitos na sua branch, mas que ainda não estão em `master`:

```bash
git log --oneline --first-parent sua-branch ^master
```

---

### ✅ **3. Alternativa visual com `gitk`**

Se você quiser ver visualmente:

```bash
gitk sua-branch ^master
```

Isso mostra o que está em `sua-branch` e não em `master`, útil para identificar divergências.

---

### 🔁 **Resumo**

- `git log master..sua-branch` → Commits da sua branch que não estão na master.
    
- `git log --no-merges master..sua-branch` → Mesma ideia, mas ignora commits de merge.
    
- `git log --first-parent sua-branch ^master` → Mostra o caminho linear da sua branch após divergir da master.
    

Se quiser identificar apenas os commits **autorados por você**, adicione `--author="Seu Nome"` no final dos comandos.