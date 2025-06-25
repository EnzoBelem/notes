
Índices de banco de dados são fundamentais para o desempenho de consultas — especialmente em sistemas relacionais como PostgreSQL, MySQL ou Oracle.

---

## ✅ O que é um índice no banco de dados?

Um **índice** é uma **estrutura auxiliar de dados** (normalmente baseada em árvores B-tree, mas não exclusivamente) que o banco cria **para localizar informações mais rapidamente**, evitando varreduras completas da tabela (full table scan).

É conceitualmente **parecido com o índice no final de um livro**: em vez de ler todas as páginas, você consulta o índice e vai direto ao ponto.

---

## ✅ Como funciona internamente?

### Exemplo:

```sql
SELECT * FROM usuario WHERE email = 'a@exemplo.com';
```

Sem índice:

- O banco **lê linha por linha** para encontrar a que tem o email.
    
- Isso é **lento em tabelas grandes**.
    

Com índice no campo `email`:

- O banco consulta a estrutura de índice (ex: árvore B-tree).
    
- Encontra o ponteiro direto para a linha correta.
    
- Resultado: **consulta extremamente rápida**, mesmo com milhões de registros.
    

---

## ⚙️ Tipos mais comuns de índices

|Tipo|Explicação rápida|Uso típico|
|---|---|---|
|**B-tree**|Ordenado, eficiente para igualdade e comparações (`<`, `>`)|`WHERE campo = valor`, `ORDER BY`|
|**Hash**|Apenas igualdade (`=`), mais rápido em alguns casos específicos|Pouco usado, tem limitações|
|**GIN**|Índice invertido, usado para arrays, JSON, full-text search|`@>`, `?`, buscas em listas ou JSON|
|**GiST**|Geolocalização, buscas por proximidade|PostGIS, busca geoespacial|
|**BRIN**|Resumo de blocos — ótimo para tabelas muito grandes ordenadas|Data warehouse, logs|

---

## ✅ Quando criar índices?

Crie índice quando:

- Um campo é **usado com frequência em `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`**.
    
- Um campo é **único ou quase único**.
    
- Você tem **combinação de campos** usados juntos em filtros ou ordenações.
    
- O campo é usado em **pesquisas de texto**, listas (`ArrayField`), JSON (`JSONField`), etc.
    

---

## ⚠️ Quando **não** criar índice?

- Em **colunas booleanas** ou com **baixa cardinalidade** (ex: `status=True` em 99% das linhas) — o índice não melhora e pode até piorar a performance.
    
- Em colunas **não usadas para busca ou ordenação**.
    
- Quando o custo de **escrita** (INSERT/UPDATE) for mais importante — índices aumentam esse custo.
    

---

## 🧠 Regras práticas para pensar índices

1. **Foque nas consultas reais**: analise os filtros mais frequentes (`EXPLAIN ANALYZE` no PostgreSQL).
    
2. **Índice composto**:
    
    - `Index(fields=["empresa", "usuario"])` é útil se você faz consultas onde os dois são usados juntos.
        
    - Mas atenção à **ordem dos campos**: só ajuda se a busca começa por `empresa`.
        
3. **Evite excesso**:
    
    - Cada índice ocupa disco.
        
    - Afeta performance de escrita.
        
    - Torna o plano de execução mais complexo para o otimizador.
        

---

## ✅ Conclusão

|Conceito-chave|Resumo prático|
|---|---|
|Índice é uma estrutura auxiliar|Facilita localização rápida de registros|
|Custa mais na escrita, acelera leitura|Equilíbrio entre desempenho de leitura e escrita|
|Planeje com base nas queries reais|`WHERE`, `JOIN`, `ORDER BY`, `UNIQUE`|
|Use tipos específicos quando necessário|GIN, GiST, BRIN, B-tree conforme a natureza dos dados|

---
