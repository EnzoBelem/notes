# Protocolo SFTP

#Linux 

O **SFTP (SSH File Transfer Protocol)** é um protocolo completamente novo, construído do zero para rodar dentro de um túnel seguro e é um sucessor moderno ao FTP.

### 1. FTP vs. SFTP

- **FTP (File Transfer Protocol):** É um protocolo inseguro. Ele abre duas conexões: uma para comandos e outra para os dados. Os arquivos e as senhas viajam em texto puro (qualquer um na rede pode ler).

- **SFTP:** Ele utiliza apenas uma única conexão (geralmente a porta 22). Tudo o que viaja ali dentro (credenciais, comandos de listagem e os arquivos em si) está criptografado pelo SSH.

### 2. Como funciona o  Handshake

Quando o cliente se conecta ao um servidor, acontece o seguinte:

1. **Negociação SSH:** O cliente e o servidor concordam em qual algoritmo de criptografia usar (ex: AES-256).

2. **Troca de Chaves:** Eles trocam "segredos" para garantir que ninguém no meio do caminho consiga interceptar a conversa.

3. **Autenticação:** O servidor pede a senha ou a Chave SSH.

4. **Criação do Canal:** Em vez de abrir um terminal (shell), o servidor abre um subsistema de transferência de arquivos.

### 3. Principais Características Técnicas

- **Pacotização:** O SFTP quebra os arquivos em pequenos pacotes. Cada pacote é confirmado pelo receptor. Se a conexão cair no meio de um arquivo de 10GB, o protocolo consegue retomar de onde parou (Resume).

- **Manipulação Remota:** O SFTP permite que o cliente manipule o sistema de arquivos remoto: renomear arquivos, deletar, alterar permissões e listar diretórios.

- **Segurança Binária:** No FTP tradicional, existia o modo "ASCII" e "Binário", o que muitas vezes corrompia arquivos. O SFTP é puramente binário, o que o torna muito mais confiável para mover dados sensíveis.

### 4. Por que ele é uma ótima alternativa para ETLs?

1. **Firewall Friendly:** Como ele usa apenas uma porta (22), é muito fácil liberar no firewall da sua VPN ou no seu servidor Linux.

2. **Identidade:** Ele suporta **Chaves Públicas/Privadas**. Em vez de salvar uma senha na ETL, você coloca a chave pública do cliente no seu servidor.

3. **Metadados:** Ele preserva informações importantes, como a data de modificação do arquivo, o que permite que a ETL saiba se um arquivo é novo ou se já foi processado.

### 5. Versões do Protocolo

O SFTP evoluiu ao longo do tempo. A versão mais comum é a **v3** (amplamente suportada pelo OpenSSH do Linux). Existem versões mais recentes (v4, v5, v6) que adicionam suporte a caracteres especiais (UTF-8) e melhores tratos de permissões, mas para transferência de dados brutos (CSV/JSON), a v3 é o padrão de estabilidade.
