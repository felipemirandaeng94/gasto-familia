# Controle de gastos da família

App para instalar no celular, sem e-mail e sem senha. Cada pessoa escolhe o próprio nome na tela inicial, vê só o próprio saldo e lança os gastos. O Felipe (administrador) define os limites por categoria e vê a família inteira. O banco recusa qualquer gasto que passe do limite da categoria.

## Arquivos

- `index.html`: o app inteiro.
- `supabase.sql`: tabelas, trava de limite e dados iniciais (Felipe, Isabela, Agatha, C6 Black e Banco do Brasil).
- `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`: o que faz o app instalar na tela inicial.

## Passo a passo

### 1. Banco no Supabase

1. Crie um projeto em supabase.com.
2. Abra **SQL Editor**, cole o conteúdo de `supabase.sql` e clique em **Run**. Deve terminar com "Success".
3. Se você já tinha rodado a versão antiga (com e-mail), leia o comentário no topo do SQL: tem um bloco de limpeza para descomentar uma única vez.

Não precisa criar usuários em Authentication. As três pessoas já nascem no banco.

### 2. Ligar o app ao banco

Em **Project Settings > API**, copie a **Project URL** e a chave **anon public**. No início do script do `index.html`, troque:

```js
var SUPABASE_URL = 'COLE_AQUI_A_URL_DO_PROJETO';
var SUPABASE_ANON_KEY = 'COLE_AQUI_A_CHAVE_ANON';
```

Nunca cole a chave `service_role` no HTML, só a `anon`.

### 3. Publicar no GitHub Pages

1. Crie um repositório e suba os 6 arquivos na raiz (sem pastas).
2. Em **Settings > Pages**, escolha a branch `main`, pasta `/ (root)` e salve.
3. O endereço sai como `https://SEU-USUARIO.github.io/NOME-DO-REPO/`.

### 4. Instalar no celular

- iPhone: abra o endereço no Safari, toque em Compartilhar e depois em **Adicionar à Tela de Início**.
- Android: abra no Chrome, menu de três pontos e **Instalar app**.

Na primeira vez, cada pessoa escolhe o próprio nome. O aparelho lembra, e nas próximas vezes o app já abre direto.

## Dia a dia

- Felipe, em **Ajustes**, escolhe a pessoa e define o limite de cada categoria. O limite é fixo: cada gasto desconta até acabar. Para liberar mais dinheiro, aumente o limite.
- Isabela e Agatha tocam em **Lançar gasto**, informam valor, categoria e cartão. Se passar do que sobra, o app avisa e o banco recusa.
- Em **Ajustes** o Felipe também troca nomes, cria categorias, cadastra cartões e adiciona outras pessoas.
- Só o perfil do Felipe apaga gastos lançados.
- O botão com o nome no topo (**Trocar**) volta para a tela de escolha.

## Sobre o PIN do Felipe

O perfil do Felipe pede um PIN de 4 dígitos na hora de escolher o nome. Isabela e Agatha entram direto nos próprios nomes, sem PIN. O PIN fica no código só como hash (SHA-256), nunca em texto. Para trocar o PIN, é preciso gerar um hash novo e trocar a linha `ADMIN_PIN_HASH` no `index.html`: peça ao Claude.

É uma trava leve para a família, não segurança de banco: a chave anon fica visível no código da página pública, então não publique o endereço fora da família.

## Se algo der errado

- Tela "Falta configurar": a URL e a chave ainda não foram coladas no `index.html`.
- "Nenhuma pessoa cadastrada": o `supabase.sql` não rodou até o fim. Rode de novo.
- Mudou o `index.html` e o celular mostra a versão antiga: feche o app e abra de novo. Se persistir, aumente o número em `CACHE` no `sw.js`.
