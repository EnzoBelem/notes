#Linux 

Aqui estão as **maneiras mais simples e seguras de limpar o sistema Linux**, liberando espaço em disco e removendo arquivos desnecessários, especialmente para distros baseadas em **Debian/Ubuntu** ou **Arch/Garuda** (como você usa):

---

## 1. **Limpar pacotes e dependências não utilizados**

### **Debian/Ubuntu:**

```bash
sudo apt autoremove --purge
sudo apt autoclean
sudo apt clean
```

- `autoremove`: remove pacotes órfãos.
    
- `autoclean`: remove arquivos `.deb` antigos do cache.
    
- `clean`: remove todo o cache de pacotes.
    

### **Arch/Garuda:**

```bash
sudo pacman -Rns $(pacman -Qdtq)   # remove dependências órfãs
sudo pacman -Sc                    # limpa cache parcial
sudo pacman -Scc                   # limpa todo cache (cuidado!)
```

> **Recomendado:** executar `pacman -Qtdq` antes de remover para revisar o que será apagado.

---

## 2. **Limpar cache do sistema**

### Cache do usuário:

```bash
rm -rf ~/.cache/*
```

> Pode liberar bastante espaço, especialmente se você usa navegadores ou editores pesados (VS Code, etc).

---

## 3. **Verificar pastas grandes (como `/var` e `/tmp`)**

- **Limpar arquivos temporários:**
    
    ```bash
    sudo rm -rf /tmp/*
    sudo rm -rf /var/tmp/*
    ```
    
- **Limpar logs antigos:**
    
    ```bash
    sudo journalctl --vacuum-time=7d
    ```
    
    > Mantém apenas os logs dos últimos 7 dias.
    

---

## 4. **Limpar lixeira**

```bash
rm -rf ~/.local/share/Trash/*
```

---

## 5. **Limpar cache do Flatpak (se usar)**

```bash
flatpak uninstall --unused
```

---

## 6. **Limpar cache do Snap (se usar)**

```bash
sudo du -h /var/lib/snapd/snaps/
sudo rm /var/lib/snapd/snaps/*.old
```

---

## 7. **Usar ferramentas de limpeza automáticas**

### `bleachbit` (modo gráfico ou CLI)

Instalação:

```bash
sudo apt install bleachbit      # Debian/Ubuntu
sudo pacman -S bleachbit        # Arch/Garuda
```

Modo gráfico:

```bash
bleachbit
```

Modo terminal:

```bash
sudo bleachbit --clean system.cache system.tmp
```

---

## Dica final

Antes de rodar comandos destrutivos, sempre verifique o que será excluído. Exemplos:

```bash
# Verificar pacotes órfãos no Arch:
pacman -Qtdq

# Verificar diretórios grandes:
sudo du -h --max-depth=1 / | sort -hr | head
```
