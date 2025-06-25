#Vim

No Vim, para substituir todas as ocorrências de uma string em um arquivo, você pode usar o seguinte comando no modo normal (pressione `Esc` para garantir que está no modo normal):

```
:%s/padrao/substituto/g
```

Explicação:

- `:` entra no modo de comando.
    
- `%` indica que a substituição será feita em **todas as linhas** do arquivo.
    
- `s` é o comando de substituição.
    
- `padrao` é a string que você quer substituir.
    
- `substituto` é a nova string que vai entrar no lugar.
    
- `g` (de _global_) faz com que **todas** as ocorrências na linha sejam substituídas, e não apenas a primeira.
    

### Exemplos

1. Substituir todas as ocorrências de `"foo"` por `"bar"`:
    
    ```
    :%s/foo/bar/g
    ```
    
2. Para fazer isso **sem diferenciar maiúsculas e minúsculas**:
    
    ```
    :%s/foo/bar/gi
    ```
    
3. Para confirmar cada substituição:
    
    ```
    :%s/foo/bar/gc
    ```
