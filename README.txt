SORTEIO DA SORTE — Guia de configuração (com banco de dados real)
====================================================================

Este site agora usa o Firebase (Google) como banco de dados de
verdade: Firestore para guardar os cadastros e o vencedor, e
Firebase Authentication para o login do administrador.
É gratuito no plano padrão (Spark) para um volume como este.

Siga os passos abaixo antes de publicar. Leva uns 10-15 minutos.


PASSO 1 — Criar o projeto no Firebase
--------------------------------------
1. Acesse https://console.firebase.google.com
2. Clique em "Adicionar projeto" (ou "Add project")
3. Dê um nome, ex: "sorteio-da-sorte"
4. Pode desativar o Google Analytics (não é necessário)
5. Clique em "Criar projeto"


PASSO 2 — Ativar o Firestore Database
---------------------------------------
1. No menu lateral, clique em "Firestore Database"
2. Clique em "Criar banco de dados"
3. Escolha o modo "Produção" (production mode)
4. Escolha uma região (qualquer uma próxima do Brasil, ex:
   southamerica-east1 - São Paulo)
5. Clique em "Ativar"


PASSO 3 — Aplicar as regras de segurança
-------------------------------------------
1. Ainda em "Firestore Database", clique na aba "Regras" (Rules)
2. Apague o conteúdo padrão
3. Abra o arquivo "firestore.rules" (incluído neste zip) e cole
   o conteúdo dele no lugar
4. Clique em "Publicar"

Essas regras garantem que:
- Qualquer visitante pode ver quais números estão livres/ocupados
  e reservar um número disponível (mas nunca ver o nome de outros
  participantes, e nunca reservar um número já ocupado)
- Só o administrador logado pode ver a lista completa de nomes/
  telefones, editar, excluir e sortear


PASSO 4 — Ativar o login por e-mail/senha
--------------------------------------------
1. No menu lateral, clique em "Authentication"
2. Clique em "Começar" (Get started)
3. Na lista de provedores, clique em "E-mail/senha"
4. Ative a primeira opção ("E-mail/senha") e salve


PASSO 5 — Criar o usuário administrador
-------------------------------------------
1. Ainda em "Authentication", vá na aba "Users"
2. Clique em "Adicionar usuário"
3. Digite o e-mail e a senha que você (administrador) vai usar
   para entrar no painel de administração do site
4. Salve

Você pode criar mais de um usuário aqui se quiser mais de um
administrador.


PASSO 6 — Registrar o app Web e copiar as chaves
----------------------------------------------------
1. Clique no ícone de engrenagem > "Configurações do projeto"
2. Role até "Seus apps" e clique no ícone "</>" (Web)
3. Dê um apelido ao app (ex: "sorteio-web") e clique em "Registrar app"
4. O Firebase vai mostrar um bloco de código com um objeto chamado
   "firebaseConfig" parecido com isto:

   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "sorteio-da-sorte.firebaseapp.com",
     projectId: "sorteio-da-sorte",
     storageBucket: "sorteio-da-sorte.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };

5. Copie esses valores


PASSO 7 — Colar as chaves no site
-------------------------------------
1. Abra o arquivo "index.html" em qualquer editor de texto
2. Procure por "COLE_AQUI" (aparece 6 vezes, próximo ao final do
   arquivo, dentro de "firebaseConfig")
3. Substitua cada "COLE_AQUI" pelo valor correspondente que você
   copiou no passo anterior
4. Salve o arquivo


PASSO 8 — Publicar o site
-----------------------------
Agora é só subir a pasta com o index.html em qualquer hospedagem
de site estático. As opções mais simples e gratuitas:

  OPÇÃO A: Firebase Hosting (fica tudo no mesmo projeto)
    - Instale o Firebase CLI: npm install -g firebase-tools
    - No terminal, dentro desta pasta: firebase login
    - Depois: firebase init hosting
      (escolha o projeto que você criou; diretório público = ".";
       configurar como single-page app = Não)
    - Depois: firebase deploy
    - O site fica em https://SEU-PROJETO.web.app

  OPÇÃO B: Netlify
    - Acesse https://app.netlify.com/drop
    - Arraste esta pasta inteira (com o index.html já configurado)
    - Pronto, o Netlify gera um link público na hora

  OPÇÃO C: Vercel ou GitHub Pages
    - Funcionam do mesmo jeito: suba os arquivos e aponte para
      o index.html


COMO USAR DEPOIS DE PUBLICADO
--------------------------------
- Qualquer visitante acessa o link, clica em um número disponível
  na grade e preenche nome + telefone para reservar
- Você (administrador) acessa a aba "Administração", entra com o
  e-mail/senha criados no Passo 5, e de lá pode:
    - ver e editar todos os cadastros
    - excluir cadastros
    - adicionar cadastros manualmente
    - realizar o sorteio (escolhe aleatoriamente entre os números
      já reservados)
    - limpar o vencedor ou zerar tudo


SOBRE CUSTOS
--------------
O plano gratuito do Firebase (Spark) inclui bastante uso gratuito
de Firestore e Authentication por mês — para um sorteio de até 500
participantes, isso não deve gerar nenhum custo. Se quiser, dá
para acompanhar o consumo em "Uso e faturamento" no console.


DÚVIDAS COMUNS
-----------------
- "A tela mostra um aviso vermelho de configuração pendente"
  → Você ainda não colou as chaves do Passo 7 no index.html.

- "Erro de permissão ao tentar reservar/sortear"
  → Confira se as regras do Passo 3 foram publicadas corretamente.

- "Não consigo entrar como administrador"
  → Confira o e-mail/senha criados no Passo 5, e se o provedor
    "E-mail/senha" está ativado (Passo 4).
