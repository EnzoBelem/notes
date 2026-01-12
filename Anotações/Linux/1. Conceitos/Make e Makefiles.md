#Linux 

O `Makefile` é um arquivo usado pelo utilitário **`make`**, que automatiza a execução de **tarefas repetitivas via terminal**. É muito comum em projetos de software — especialmente com C, Python, Docker e CI/CD — por ser simples e poderoso.

---

## O que é o `make`

O `make` é uma ferramenta que:

- Executa comandos definidos em um arquivo chamado `Makefile`.
    
- Permite nomear e reutilizar sequências de comandos.
    
- Garante **padronização** nos comandos do time de desenvolvimento.
    

---

## Estrutura de um `Makefile`

Um `Makefile` é composto por **regras**:

```makefile
nome-da-tarefa:
	comando1
	comando2
```

Exemplo prático:

```makefile
migrate:
	docker exec -it clean_api python manage.py migrate
```

Agora, em vez de digitar o comando inteiro, você pode apenas rodar:

```bash
make migrate
```

---

## Vantagens de usar Makefile

| Vantagem                      | Descrição                                                    |
| ----------------------------- | ------------------------------------------------------------ |
| Evita repetir comandos longos | Use `make up-dev` em vez de digitar docker-compose completo. |
| Facilita onboarding           | Novos devs só precisam saber `make up-dev` e `make migrate`. |
| Automatiza tarefas            | Testes, builds, migrações, lint, etc.                        |
| Autodocumentação              | Pode incluir `make help` para listar tudo.                   |
| Multiplataforma (Linux/macOS) | Não depende de Python, Node etc., só do `make`.              |

---

## Requisitos

- No **Linux/macOS** o `make` já vem instalado.
    
- No **Windows**, é necessário:
    
    - Usar o WSL (Windows Subsystem for Linux), ou
        
    - Instalar o `make` com o [Chocolatey](https://chocolatey.org/) ou via [GnuWin](http://gnuwin32.sourceforge.net/packages/make.htm).
        

---

## Onde o Makefile se encaixa no seu projeto

No seu caso (Django + Docker), o `Makefile` funciona como uma **interface de linha de comando personalizada** para o projeto. Ele centraliza tarefas como:

- Subir/derrubar containers
    
- Executar migrações
    
- Criar superusuário
    
- Acessar shell
    
- Rodar testes
    
- Construir imagens Docker
    