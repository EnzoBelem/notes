A estrutura básica do dicionário `LOGGING` no Django (baseado na biblioteca `logging` do Python) é uma configuração declarativa que define **como, onde e o que será registrado** nos logs da aplicação.

Vamos decompor os principais elementos da estrutura:

---

# Estrutura básica do `LOGGING`

```python
LOGGING = {
    "version": 1,  # obrigatório, sempre 1
    "disable_existing_loggers": False,

    "formatters": { ... },   # Define o formato dos logs
    "handlers": { ... },     # Define onde os logs serão enviados (arquivo, console, etc)
    "loggers": { ... },      # Define quais partes da aplicação vão gerar logs e como

    # opcional:
    "root": { ... },         # Logger base/global (não usado na maioria dos casos)
    "filters": { ... },      # Filtros adicionais (ex: apenas se DEBUG=False)
}
```

---

### 1. `"formatters"` – Como os logs serão formatados

Define o texto que será exibido no log. Pode incluir data, nível, nome do logger, mensagem, etc.

```python
"formatters": {
    "verbose": {
        "format": "[{asctime}] {levelname} {name} - {message}",
        "style": "{",  # pode ser '%', '{' ou '$'
    },
    "simple": {
        "format": "{levelname}: {message}",
        "style": "{",
    },
}
```

---

### 2. `"handlers"` – Para onde os logs vão

Controla os destinos: console, arquivo, e-mail, etc.

```python
"handlers": {
    "console": {
        "level": "DEBUG",
        "class": "logging.StreamHandler",
        "formatter": "simple",
    },
    "arquivo_geral": {
        "level": "INFO",
        "class": "logging.FileHandler",
        "filename": "geral.log",
        "formatter": "verbose",
    },
}
```

Você também pode usar outros tipos como:

- `logging.handlers.TimedRotatingFileHandler` (para rotação)
    
- `logging.handlers.SMTPHandler` (para e-mail)
    

---

### 3. `"loggers"` – Quem está enviando logs

Define os **nomes dos módulos ou apps** que geram logs e quais handlers usar.

```python
"loggers": {
    "django": {
        "handlers": ["console", "arquivo_geral"],
        "level": "INFO",
        "propagate": True,
    },
    "rest_framework": {
        "handlers": ["console"],
        "level": "WARNING",
        "propagate": False,
    },
    "sua_api": {
        "handlers": ["console", "arquivo_geral"],
        "level": "DEBUG",
        "propagate": False,
    },
}
```

### Campos importantes:

- **`level`**: Mínimo nível de log aceito (DEBUG, INFO, WARNING, ERROR, CRITICAL)
    
- **`handlers`**: Lista de handlers associados a esse logger
    
- **`propagate`**:
    
    - `True`: o log sobe para o logger pai (como o `django`)
        
    - `False`: evita logs duplicados
        

---

### 4. `"filters"` (opcional)

Permite definir regras para exibir logs apenas em certas condições (ex: só se `DEBUG=False`).

---

### Exemplo completo mínimo:

```python
LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "default": {
            "format": "[{asctime}] {levelname} {message}",
            "style": "{",
        },
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "formatter": "default",
        },
    },
    "loggers": {
        "django": {
            "handlers": ["console"],
            "level": "INFO",
        },
    },
}
```

---

# Os componentes do `LOGGING`

## Handlers

**Handlers** são componentes que dizem **para onde as mensagens de log devem ir**, como:

- Console (terminal)
    
- Arquivos de log
    
- E-mail para administradores
    
- Banco de dados
    
- HTTP (webhook)
    
- Outros sistemas como syslog, journald, etc.
    

Cada handler pode:

- Definir seu **nível mínimo de log**
    
- Usar um **formato diferente** (`formatter`)
    
- Ser usado por múltiplos `loggers`
    

---

### Estrutura geral de um handler

```python
"nome_handler": {
    "class": "logging.<HandlerClass>",
    "level": "<NÍVEL MÍNIMO>",
    "formatter": "<nome_do_formatter>",
    ... # outros parâmetros específicos da classe
}
```

---

### Tipos de handlers mais comuns

#### 1. **`StreamHandler`**

- Envia logs para `sys.stdout` ou `sys.stderr` (geralmente o console).
    

```python
"class": "logging.StreamHandler"
```

#### 2. **`FileHandler`**

- Grava logs diretamente em um arquivo.
    

```python
"arquivo_geral": {
    "class": "logging.FileHandler",
    "level": "INFO",
    "filename": "/caminho/para/geral.log",
    "formatter": "verbose",
}
```

#### 3. **`TimedRotatingFileHandler`**

- Grava em arquivo e **gira o arquivo automaticamente por tempo** (ex: a cada dia, meia-noite).
    

```python
"arquivo_rotativo": {
    "class": "logging.handlers.TimedRotatingFileHandler",
    "level": "DEBUG",
    "filename": "/caminho/rotativo.log",
    "formatter": "verbose",
    "when": "midnight",
    "backupCount": 7,  # mantém 7 dias
}
```

#### 4. **`SMTPHandler`**

- Envia log por e-mail.
    

```python
"mail_admins": {
    "class": "django.utils.log.AdminEmailHandler",
    "level": "ERROR",
    "include_html": True,
}
```

> Depende de `ADMINS`, `SERVER_EMAIL`, `EMAIL_BACKEND` no settings.

#### 5. **`NullHandler`**

- Descarta todos os logs. Útil para desligar um logger.
    

```python
"class": "logging.NullHandler"
```

#### 6. **`WatchedFileHandler`**

- Similar ao `FileHandler`, mas acompanha alterações externas (bom para Unix + logrotate).
    

---

### Nível de log por handler

Cada handler pode ter seu próprio **nível mínimo**:

|Level|Descrição|
|---|---|
|`DEBUG`|Tudo, inclusive detalhes de desenvolvimento|
|`INFO`|Informações normais de operação|
|`WARNING`|Algo inesperado, mas que não causou falha|
|`ERROR`|Um erro que afetou o comportamento|
|`CRITICAL`|Erro muito grave ou sistema inutilizável|

Exemplo: você pode configurar `StreamHandler` para `DEBUG` no desenvolvimento, mas usar `FileHandler` com `ERROR` em produção.

---

### Usando múltiplos handlers

Um mesmo logger pode usar mais de um handler:

```python
"loggers": {
    "django": {
        "handlers": ["console", "arquivo_geral", "mail_admins"],
        "level": "INFO",
    }
}
```

---

### Recurso extra: Filtragem por handler

Você pode adicionar filtros específicos (por exemplo, só logar se `DEBUG=False`), embora isso seja mais avançado.

```python
"filters": {
    "require_debug_false": {
        "()": "django.utils.log.RequireDebugFalse",
    }
}
```

---

### Conclusão prática

Para um projeto DRF bem instrumentado, normalmente você terá:

|Handler|Finalidade|
|---|---|
|`console`|Logs para o terminal (dev)|
|`arquivo_geral`|Logs gerais da aplicação|
|`arquivo_erros`|Apenas erros e exceções|
|`mail_admins`|Erros críticos por e-mail|
|`rotating_handler`|Arquivo rotativo para logs históricos|

---

## Loggers

Um **logger** é a **entidade responsável por emitir mensagens de log**. Ele define:

1. **Nome do contexto** (ex: `"django"`, `"rest_framework"`, `"sua_api.views"`)
    
2. **Qual nível mínimo de severidade aceitar**
    
3. **Quais handlers devem ser acionados**
    
4. **Se deve ou não propagar o log para loggers "pais"** (`propagate`)
    

---

### Estrutura básica de um logger no `LOGGING`:

```python
"loggers": {
    "nome_do_logger": {
        "handlers": ["console", "arquivo_erros"],
        "level": "INFO",
        "propagate": True,
    },
}
```

---

###  Componentes de um logger

|Chave|Descrição|
|---|---|
|`handlers`|Lista de handlers responsáveis por processar os logs emitidos por este logger|
|`level`|Nível mínimo de severidade das mensagens que serão processadas|
|`propagate`|Se `True`, envia a mensagem para o logger pai. Se `False`, bloqueia o "vazamento"|

---

###  Níveis de log (ordem de severidade crescente)

|Nível|Uso|
|---|---|
|`DEBUG`|Diagnóstico detalhado (uso interno, dev)|
|`INFO`|Eventos normais do sistema|
|`WARNING`|Algo inesperado, mas não crítico|
|`ERROR`|Falhas que impactam funcionalidades|
|`CRITICAL`|Erros graves ou sistema comprometido|

> O logger só emite mensagens **no nível definido ou acima dele**.

---

### Importância do **nome** do logger

O nome do logger define sua hierarquia e escopo. Por padrão:

```python
logger = logging.getLogger("sua_api.views")
```

- `"sua_api"` pode ter um logger específico
    
- `"sua_api.views"` herda de `"sua_api"` se `propagate=True`
    
- `"django"` é o logger usado internamente por componentes do framework
    
- `"django.request"` lida com exceções e erros HTTP
    

---

### Exemplos práticos de loggers comuns

```python
"loggers": {
    "django": {
        "handlers": ["console"],
        "level": "INFO",
        "propagate": True,
    },
    "django.request": {
        "handlers": ["arquivo_erros"],
        "level": "ERROR",
        "propagate": False,  # evita log duplicado
    },
    "rest_framework": {
        "handlers": ["console"],
        "level": "WARNING",
        "propagate": False,
    },
    "sua_api": {
        "handlers": ["console", "arquivo_geral"],
        "level": "DEBUG",
        "propagate": False,
    },
}
```

---

###  `propagate: True` vs `False`

#### `True` (padrão)

- A mensagem **"sobe" para os loggers pais**, que também podem tratá-la.
    
- Pode causar logs duplicados se o logger pai tem os mesmos handlers.
    

#### `False`

- O log **é tratado apenas pelos handlers do logger atual**.
    
- Recomendado quando o logger já trata bem o destino e você quer evitar duplicidade.
    

---

### Como usar na prática

#### Exemplo em um arquivo `.py`:

```python
import logging
logger = logging.getLogger("sua_api.views")

def minha_view():
    logger.debug("Início da função")
    logger.info("Requisição iniciada")
    try:
        ...
    except Exception as e:
        logger.exception("Erro ao processar a requisição")
```

#### Isso será processado apenas se:

- O logger `"sua_api.views"` estiver definido ou herdar de `"sua_api"`
    
- O nível configurado permitir aquele tipo de log
    
- Houver handlers associados
    

---

### Boas práticas com loggers

|Boas práticas|Por quê|
|---|---|
|Defina nomes por módulo (`"meuprojeto.views"`)|Para granularidade e filtragem|
|Não use `print()`|Use `logger.info`, `logger.debug`, etc|
|Evite logar dados sensíveis (senha, token)|Segurança|
|Combine logger + handler + formatter por contexto|Ex: logs de requisição, de tarefa, de erro crítico|

---

## Root

O `root` é o **logger mais alto da hierarquia** no sistema de logging do Python.

> **Todos os loggers sem configuração explícita herdam dele**, desde que `propagate=True`.

---

### Estrutura básica

```python
"root": {
    "level": "WARNING",
    "handlers": ["console"],
}
```

#### Componentes:

|Chave|Função|
|---|---|
|`level`|Nível mínimo de log que o logger raiz aceitará|
|`handlers`|Lista de destinos padrão para os logs que não têm logger configurado diretamente|

---

### Quando o `root` é usado?

- Quando você chama `logging.warning(...)` **diretamente**, sem `getLogger(...)`
    
- Quando um módulo de terceiros **usa logging sem declarar logger nomeado**
    
- Quando você **não declara um logger específico para algum componente**, ele herda do `root` se `propagate=True`
    

---

### Exemplo prático

#### Sem root:

```python
import logging
logging.warning("Algo aconteceu")
```

- Essa mensagem **não aparece** se não houver logger definido para ela, e o `root` não estiver configurado.
    

#### Com root:

```python
"root": {
    "level": "WARNING",
    "handlers": ["console", "arquivo_geral"],
}
```

- Agora, qualquer log sem logger nomeado (ou com `propagate=True`) será direcionado para `console` e `arquivo_geral`.
    

---

### Quando evitar mexer no root

- Se você **tem controle total sobre todos os loggers usados**, pode deixar o `root` em branco ou não configurá-lo.
    
- Se você **usa muitos pacotes de terceiros**, ou quer garantir um fallback de log, o `root` pode servir como rede de segurança.
    

---

###  Recomendação para projetos Django/DRF

#### Simples e segura:

```python
"root": {
    "level": "WARNING",
    "handlers": ["console"],
}
```

#### Com rotação e arquivo:

```python
"root": {
    "level": "INFO",
    "handlers": ["arquivo_geral", "console"],
}
```

---

###  Como o root interage com outros loggers?

Se você tem:

```python
"loggers": {
    "sua_api": {
        "handlers": ["arquivo_geral"],
        "level": "DEBUG",
        "propagate": True,
    },
},
```

E você emite:

```python
logger = logging.getLogger("sua_api")
logger.error("Erro crítico")
```

O log vai:

- Para `arquivo_geral` (definido no logger)
    
- E **também** para o handler do `root` (por causa do `propagate=True`)
    

---

### Boas práticas com `root`

|Boas práticas|Por quê|
|---|---|
|Configure `root` com `WARNING` ou `INFO`|Para capturar logs inesperados ou genéricos|
|Use `console` ou arquivos rotacionados como handler|Para depuração ou auditoria|
|Use `propagate=False` nos loggers específicos|Para evitar duplicidade de logs com `root`|
|**Nunca dependa só do `root`**|Configure loggers por domínio (Django, DRF, sua aplicação)|

---

## Filters

Um `filter` em logging é uma **função ou classe que decide se uma mensagem de log deve ou não ser processada por um handler**.

- Ele atua como uma **camada extra de lógica condicional** entre o logger e o handler.
    
- Pode ser usado para:
    
    - Limitar logs por ambiente (`DEBUG=True`)
        
    - Separar logs de usuários autenticados
        
    - Redirecionar logs específicos para handlers distintos
        
    - Evitar logar determinados módulos ou endpoints
        

---

### Estrutura geral

```python
"filters": {
    "meu_filtro": {
        "()": "caminho.para.MeuFiltroClasseOuFuncao",
        "param1": "valor",
    }
}
```

- O valor de `"()"` é o caminho para uma **classe ou função** que herda de `logging.Filter` ou `django.utils.log.CallbackFilter`.
    
- Pode conter parâmetros adicionais, passados para o `__init__`.
    

---

### Como aplicar filtros

Você adiciona filtros **dentro de um handler**, assim:

```python
"handlers": {
    "arquivo_debug": {
        "class": "logging.FileHandler",
        "filename": "debug.log",
        "level": "DEBUG",
        "formatter": "verbose",
        "filters": ["so_em_debug"],
    },
}
```

---

### Filtros úteis embutidos no Django

#### 1. `RequireDebugTrue`

Só permite logs se `DEBUG=True`.

```python
"filters": {
    "so_em_debug": {
        "()": "django.utils.log.RequireDebugTrue",
    },
}
```

#### 2. `RequireDebugFalse`

Só permite logs se `DEBUG=False`.

```python
"filters": {
    "so_em_producao": {
        "()": "django.utils.log.RequireDebugFalse",
    },
}
```

---

### Exemplo completo

```python
"filters": {
    "so_em_debug": {
        "()": "django.utils.log.RequireDebugTrue",
    },
},
"handlers": {
    "console_debug": {
        "class": "logging.StreamHandler",
        "formatter": "simple",
        "filters": ["so_em_debug"],
        "level": "DEBUG",
    },
},
"loggers": {
    "django": {
        "handlers": ["console_debug"],
        "level": "DEBUG",
    },
}
```

> Nesse exemplo, o log só vai para o console **se o `DEBUG=True`**.

---

### Criando seu próprio filtro

#### Exemplo com `logging.Filter`:

```python
import logging

class FiltroPorUsuario(logging.Filter):
    def filter(self, record):
        return hasattr(record, "request") and getattr(record.request, "user", None)
```

> Depois, registre no `LOGGING`:

```python
"filters": {
    "usuario_autenticado": {
        "()": "caminho.para.FiltroPorUsuario",
    }
}
```

---

### Casos comuns de uso

|Caso|Solução|
|---|---|
|Logar só em produção|`RequireDebugFalse`|
|Evitar logs duplicados do DRF ou Django Admin|Filtro por `request.path`|
|Redirecionar logs de usuários autenticados|Filtro customizado|
|Filtrar logs por conteúdo da mensagem|Filtro que analisa `record.msg`|
|Ignorar logs de health-checks|Filtro por `request.path.startswith("/health")`|

---

### `CallbackFilter`

Você também pode usar `CallbackFilter` com lambda/função simples:

```python
from django.utils.log import CallbackFilter

def somente_post(record):
    req = getattr(record, "request", None)
    return req and req.method == "POST"
```

```python
"filters": {
    "apenas_post": {
        "()": "django.utils.log.CallbackFilter",
        "callback": "caminho.somente_post",
    }
}
```

---

### Conclusão

|Conceito|Papel|
|---|---|
|`filters`|Controlam _se_ a mensagem deve ser processada|
|Atuam nos `handlers`|São filtros condicionais por ambiente, conteúdo ou contexto|
|Podem ser simples ou complexos|Pode usar classes customizadas, lambdas, ou filtros prontos|