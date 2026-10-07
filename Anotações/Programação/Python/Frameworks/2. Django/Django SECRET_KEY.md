# Django SECRET_KEY

## 🧠 **O que é a `SECRET_KEY` no Django?**

A `SECRET_KEY` é uma **chave secreta usada pelo Django para várias operações internas** que exigem segurança e criptografia.

Ela é essencial pra garantir que partes sensíveis do seu aplicativo sejam seguras.

---

## 🔐 **Qual é a função dela na prática?**

A `SECRET_KEY` é usada principalmente para:

### 🔄 1️⃣ **Assinatura de Tokens e Cookies**

- Django **usa essa chave pra assinar cookies de sessão e CSRF tokens**.
- Isso garante que **ninguém pode forjar ou manipular essas informações**, porque só o seu servidor conhece a chave.

### 🔑 2️⃣ **Hashing em segurança**

- Ela **é usada em alguns processos de hashing internos** do Django.
- Exemplo: Geração de tokens e links seguros.

### 🧑‍💻 3️⃣ **Geração de valores criptográficos**

- Django **gera valores seguros aleatórios** usando a `SECRET_KEY` em algumas partes internas.

---

## 🛑 **O que acontece se eu expor a `SECRET_KEY`?**

- **Qualquer pessoa que tiver a sua `SECRET_KEY` pode gerar tokens de sessão válidos e forjar cookies.**
- Isso pode **permitir que ela se autentique como qualquer usuário** ou **bypasse proteções de segurança**, em alguns casos.

Por isso, **em produção**, ela deve ser **única e secreta**!

---

## 🧑‍💻 **Como ela geralmente está no `settings.py`?**

No `settings.py` padrão do Django:

```python
SECRET_KEY = 'sua_chave_muito_secreta'
```

Esse valor **é gerado automaticamente** quando você cria o projeto:

```bash
django-admin startproject meu_projeto
```

Mas isso é só pra **desenvolvimento**.  
**Em produção, você deve usar uma variável de ambiente**.

---

## 🛠️ **Como lidar com isso corretamente (boa prática):**

### 🔵 1️⃣ **Localmente (sem Docker)**:

Usar `python-decouple` ou `dotenv` e colocar no `.env`:

```
SECRET_KEY=uma_chave_muito_secreta_aqui
```

No `settings.py`:

```python
from decouple import config

SECRET_KEY = config('SECRET_KEY', default='chave-desenvolvimento-insegura')
```

---

### 🟢 2️⃣ **No Docker (Compose)**:

No `docker-compose.yml`:

```yaml
api_django:
  environment:
    - SECRET_KEY=uma_chave_muito_secreta_aqui
```

Ou em `.env`:

```
SECRET_KEY=uma_chave_muito_secreta_aqui
```

E no Compose:

```yaml
env_file:
  - .env
```

---

## 🚀 **Resumo final:**

| Contexto            | O que fazer com a `SECRET_KEY`?                                |
| ------------------- | -------------------------------------------------------------- |
| **Desenvolvimento** | Pode deixar fixa ou em `.env`                                  |
| **Produção**        | **Sempre usar variável de ambiente (e não versionar a chave)** |

---

## 🧑‍💻 1️⃣ **Gerar com o próprio Django (mais comum):**

Esse é o jeito mais direto, usando o shell do Django:

```bash
python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
```

Isso vai gerar e imprimir uma chave como esta:

```
ebx#36(nfv&3+j8m&4*5^g@mg0+(bhm5%k@d+81wn6@bqgu7_1
```

Essa chave é segura e você já pode copiar e usar no seu `.env` ou `docker-compose.yml`.

---

## 🛠️ 2️⃣ **Gerar direto no Python (manual, se quiser entender melhor):**

Você pode abrir um shell Python e fazer assim:

```python
from django.core.management.utils import get_random_secret_key
get_random_secret_key()
```

Isso também gera uma nova chave segura.

---

## 📝 3️⃣ **Como colocar no `.env`:**

Cria (ou edita) o arquivo `.env`:

```
SECRET_KEY=ebx#36(nfv&3+j8m&4*5^g@mg0+(bhm5%k@d+81wn6@bqgu7_1
```

No `settings.py`:

```python
from decouple import config

SECRET_KEY = config('SECRET_KEY')
```

No `docker-compose.yml` (opcional):

```yaml
api_django:
  env_file:
    - ../../.env
```

---

## 🛑 **Importante:**

**Nunca commita a `SECRET_KEY` em repositório público**.  
Sempre usa `.env` e coloca no `.gitignore`:

```
.env
```

---

Isso ajuda?  
Quer tentar gerar uma e aplicar no seu `.env` agora? 🚀