# Gabarito das vulnerabilidades (uso pessoal — não suba isto para o GitHub, não mostre ao colega)

Use isto depois, na Etapa 4 (correção), e para conferir se as falhas que seu amigo achar
batem com as que você inseriu de propósito. Cada item segue o modelo pedido no documento
da atividade (sistema, tipo, local, descrição, impacto, correção).

---

### 1. Credenciais expostas no código-cliente
- **Local:** `js/app.js`, array `USERS`
- **Tipo:** Exposição de informações sensíveis / autenticação insegura
- **Descrição:** Usuários, e-mails e senhas em texto puro ficam visíveis para qualquer
  pessoa que abra o arquivo `app.js` (Ctrl+U ou aba "Fontes" do navegador).
- **Impacto:** Qualquer visitante consegue listar todas as contas e senhas sem nem tentar logar.
- **Correção:** Autenticação deveria acontecer num backend real, com senhas com hash
  (ex.: bcrypt) e nunca enviadas ao navegador.

### 2. Backdoor de desenvolvedor no login
- **Local:** `js/app.js`, função `login()`
- **Tipo:** Falha de autenticação (acesso indevido)
- **Descrição:** Se o usuário digitado for `admin` e a senha ficar em branco, o login é aceito
  mesmo sem senha correta — um "atalho de testes" que ficou esquecido no código.
- **Impacto:** Qualquer pessoa entra como administrador sem saber a senha real.
- **Correção:** Remover qualquer atalho/bypass de autenticação antes de publicar; nunca deixar
  código de debug em produção.

### 3. Controle de acesso quebrado no painel admin
- **Local:** `admin.html`, função `carregarAdmin()` / `isAdminSession()`
- **Tipo:** Broken Access Control
- **Descrição:** A página só confere `localStorage.getItem('isAdmin') === 'true'`. Isso pode
  ser definido manualmente no console do navegador com
  `localStorage.setItem('isAdmin','true')` e recarregando a página — sem nunca ter feito login.
- **Impacto:** Qualquer visitante vira "administrador" e vê a lista de todos os usuários (inclusive senhas).
- **Correção:** A permissão de admin precisa ser validada num servidor, a cada requisição,
  nunca confiando em algo guardado no navegador do próprio usuário.

### 4. IDOR (Insecure Direct Object Reference) no perfil
- **Local:** `perfil.html?id=N`
- **Tipo:** Quebra de controle de acesso / IDOR
- **Descrição:** A página mostra o perfil do `id` informado na URL sem checar se é o dono
  da sessão atual. Trocando `id=2` por `id=1` ou `id=3`, dá pra ver dados de outras pessoas.
- **Impacto:** Vazamento de telefone, endereço e observações pessoais de qualquer usuário cadastrado.
- **Correção:** No backend, comparar o id do perfil pedido com o id do usuário autenticado na sessão,
  e negar acesso (ou mostrar só dados públicos) se forem diferentes.

### 5. XSS refletido na busca
- **Local:** `index.html`, função `buscarLeitao()`
- **Tipo:** Cross-Site Scripting (XSS refletido)
- **Descrição:** O texto digitado no campo de busca é inserido na página via `innerHTML`
  sem nenhum escape. Um payload como `<img src=x onerror="alert(document.cookie)">`
  é executado como código.
- **Impacto:** Em um site real, permitiria roubar dados de sessão ou redirecionar a vítima.
- **Correção:** Nunca inserir texto do usuário com `innerHTML`; usar `textContent` ou
  escapar caracteres especiais antes de exibir.

### 6. Sessão não é totalmente encerrada no logout
- **Local:** `js/app.js`, função `logout()`
- **Tipo:** Falha de gerenciamento de sessão
- **Descrição:** `logout()` remove `loggedUser`, mas esquece de remover `isAdmin` do
  `localStorage`. Alguém que logou como admin e depois saiu continua com o acesso
  administrativo liberado (o link some da tela, mas `admin.html` ainda abre normalmente).
- **Impacto:** A sessão administrativa "sobrevive" ao logout.
- **Correção:** Ao sair, limpar todos os dados de sessão relacionados ao usuário, não só uma parte.

---

## Flags escondidas (para o sistema de pontuação tipo CTF)

| Flag | Onde está | Vulnerabilidade relacionada |
|---|---|---|
| `FLAG{COMENTARIO_HTML}` | Comentário HTML no topo de `index.html` | Exposição de informação em comentário |
| `FLAG{ACESSO_ADMINISTRATIVO}` | Campo `bio` do usuário admin (id=1), visível via IDOR ou painel admin | IDOR / Broken Access Control |

Você pode adicionar mais flags (ex.: em `app.js` perto do backdoor, ou em `admin.html`)
se o professor pedir mais pontos de pontuação — classifique a dificuldade/pontuação
de cada uma conforme os critérios que ele passar.
