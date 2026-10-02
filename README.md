# Fazenda Três Porquinhos

Site fictício de venda de leitões, feito para a atividade de segurança de aplicações web.
HTML, CSS e JavaScript puros — sem build, sem backend. Funciona em qualquer hospedagem estática, incluindo o GitHub Pages.

## Como publicar no GitHub Pages

1. Crie um repositório novo no GitHub (pode ser público).
2. Envie todo o conteúdo desta pasta para o repositório (pela interface do GitHub ou via `git`):
   ```
   git init
   git add .
   git commit -m "Primeira versão do site"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
   git push -u origin main
   ```
3. No repositório, vá em **Settings → Pages**.
4. Em **Branch**, selecione `main` e a pasta `/ (root)`, depois clique em **Save**.
5. Depois de alguns minutos o site estará no ar em algo como:
   `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/`
6. Compartilhe esse link com seu amigo para a fase de hacker ético.

## Contas de teste (já cadastradas)

| Usuário        | Senha      | Tipo     |
|----------------|------------|----------|
| admin          | admin123   | admin    |
| joao.criador   | joao2024   | cliente  |
| maria.compras  | maria2024  | cliente  |

## Importante sobre o "banco de dados"

O GitHub Pages só serve arquivos estáticos — não roda PHP, Node, nem bancos de dados de verdade.
Por isso, os usuários ficam numa lista dentro de `js/app.js`, e novos cadastros são guardados em
`localStorage` do navegador. Isso é, em si, parte do que torna o site inseguro: todo mundo que abre
o site consegue ler o "banco" inteiro só vendo o código-fonte.

## Estrutura

```
index.html      → página inicial / loja
login.html       → login
cadastro.html    → cadastro de novo usuário
perfil.html       → perfil (recebe ?id=N pela URL)
admin.html       → área administrativa
css/style.css    → estilos
js/app.js        → dados e lógica (login, sessão, etc.)
```

## Aviso

Este site foi criado de propósito com falhas de segurança, para uma atividade de ensino sobre
hacking ético. Não reaproveite esta base de código para um site real sem corrigir as falhas.
