# Reimplementando a geração de mundo do Minecraft em Python

### **Módulo 1 — Fundação matemática: PRNG** ✅ _(quase concluído)_

- 1.1 Entender LCG (Linear Congruential Generator) na teoria — ✅ feito
- 1.2 Implementar `JavaRandom` completo:
    - `next(bits)` — ✅ feito
    - `next_int()` sem bound — ✅ feito
    - `next_int(bound)` com bound — ✅ praticamente feito (só falta corrigir aquele último bug e validar)
    - `next_long()`, `next_double()`, `next_float()` — ainda pendentes, vamos precisar deles mais à frente
- 1.3 Validar com casos de teste conhecidos — pendente
- 1.4 Implementar `String.hashCode()` para seeds em texto — pendente

### **Módulo 2 — Ruído (Noise)** ⏳ _(próximo grande passo)_

- 2.1 Entender o que é Perlin Noise e por que ele gera terreno "orgânico"
- 2.2 Implementar Perlin Noise 1D → 2D → 3D
- 2.3 Octaves (múltiplas camadas de ruído somadas)
- 2.4 Ver como o Minecraft usa isso pra decidir "densidade" de bloco

### **Módulo 3 — Biomas** (vamos focar em versão pré-1.18, mais simples)

- 3.1 O sistema de GenLayers (zoom progressivo, voronoi)
- 3.2 Implementar as camadas de biomas
- 3.3 Gerar um "mapa de biomas" visual pra um seed

### **Módulo 4 — Estruturas**

- 4.1 Sistema de regiões (region-based placement)
- 4.2 Spacing, separation, salt
- 4.3 Implementar localização de uma estrutura simples (ex: vilas)
- 4.4 Validação de bioma/condições no candidato

### **Módulo 5 — CLI**

- 5.1 Juntar tudo numa interface de linha de comando usável
- 5.2 Otimizações de performance
- 5.3 (Opcional) expandir pra versões mais modernas (Xoroshiro128++, density functions)

---

Estamos bem avançados no Módulo 1 — falta só fechar o `next_int(bound)`, adicionar os métodos que faltam (`next_long`, `next_double`, `next_float` — vamos precisar deles principalmente no Módulo 2, pro Perlin Noise), escrever testes de validação, e implementar o hash de seed em string.

Quer corrigir e validar o `next_int(bound)` agora, ou prefere que eu já te explique pra que servem os métodos que faltam (`next_long`, `next_double`, `next_float`) antes de implementá-los?