# Feira do Mês — Controle de Compras

App web colaborativo para organizar compras mensais de supermercado entre várias pessoas: cadastro de mercados (Região Metropolitana do Recife), produtos (nome, categoria, unidade, preço), usuários (gerenciados pelo administrador), lista de compra compartilhada com total em tempo real, histórico e leitor de QR Code de notas fiscais (NFC-e).

## Tecnologia

- Front-end: HTML + JavaScript puro, um único arquivo (`index.html`)
- Banco de dados: [Supabase](https://supabase.com) (Postgres), schema `feira`
- Leitura de QR Code: biblioteca [jsQR](https://github.com/cozmo/jsQR)

## Como publicar

Este é um site estático — pode subir em **GitHub Pages**, Netlify, Vercel etc.

### Publicar no GitHub Pages
1. Crie um repositório no GitHub (público ou privado).
2. Suba este projeto:
   ```bash
   git init
   git add .
   git commit -m "Primeira versão do Feira do Mês"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
   git push -u origin main
   ```
3. No repositório, vá em **Settings → Pages**, escolha a branch `main` e a pasta `/ (root)`, salve.
4. Em alguns minutos o app estará em `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/`.

## Banco de dados (Supabase)

O app já está configurado para usar o projeto Supabase `liviocharlles-tech's Project`
(schema isolado `feira`, separado das tabelas da biblioteca/secretaria).

- URL: `https://wzfstallegpihgihiefg.supabase.co`
- Chave pública (anon/publishable): já embutida em `index.html`
- Tabelas: `feira.usuarios`, `feira.mercados`, `feira.produtos`, `feira.compras`, `feira.itens_compra`
  (expostas via views em `public.feira_*` para a API REST)

☑️ **Segurança**: acesso exige conta real (e-mail/senha) via Supabase Auth. As regras (RLS) só liberam leitura/escrita para quem tem uma linha em `feira.usuarios` ligada à própria conta autenticada. O primeiro cadastro vira administrador automaticamente; os demais entram como usuário comum até o admin promover ou remover o acesso pela aba Usuários. Recomenda-se também ativar "Leaked Password Protection" no painel do Supabase (Authentication > Policies).
pode ler e escrever). Não há login real do Supabase — o "login" do app é só escolha de nome
+ PIN opcional, uma trava leve. Não é adequado para dados sensíveis ou uso em grande escala
sem reforçar a segurança (RLS por usuário autenticado, chave de serviço no back-end, etc.).

## Estrutura

```
index.html   → aplicativo completo (todas as telas e lógica)
README.md    → este arquivo
```

## Usar como app no celular (PWA)

O app agora é instalável na tela inicial, como um aplicativo de verdade — sem precisar de loja de app. Isso só funciona depois de publicado num endereço HTTPS de verdade (ex.: GitHub Pages), não no arquivo aberto direto do computador.

**Android (Chrome):** abra o link do app; depois de alguns segundos aparece uma faixa "Instalar" no topo — toque nela. Se não aparecer, use o menu (⋮) → "Instalar aplicativo" ou "Adicionar à tela inicial".

**iPhone (Safari):** abra o link do app, toque no ícone de **Compartilhar** (o quadrado com a seta para cima) e depois em **"Adicionar à Tela de Início"**.

Depois de instalado, o app abre em tela cheia, com ícone próprio, sem a barra de endereço do navegador — os dados continuam sincronizados pelo Supabase normalmente.

### Arquivos do PWA
```
manifest.json          → nome, ícone e cores do app instalado
sw.js                   → service worker (guarda o "esqueleto" do app para abrir mais rápido; nunca guarda dados do Supabase)
icon-192.png, icon-512.png, icon-512-maskable.png → ícones do app
```
