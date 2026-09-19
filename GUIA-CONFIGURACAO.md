# Guia de configuração — Prontuário do clube

Tudo aqui é feito pelo navegador, sem terminal — igual você já faz no Baú de Ferramentas.

## 1. Criar o projeto Firebase (novo e separado)

1. Acesse console.firebase.google.com e clique em "Adicionar projeto".
2. Dê um nome (sugestão: `prontuario-clube-cec`). Este projeto é **separado** do `checkin---pre-jogo` (Baú) e do projeto do prontuário particular.
3. Pode desativar o Google Analytics — não é necessário.

## 2. Ativar Authentication

1. No menu lateral, vá em **Build > Authentication**.
2. Clique em "Get started".
3. Na aba "Sign-in method", ative o provedor **E-mail/senha**.
4. Vá na aba "Users" e clique em "Add user" para criar uma conta para você e uma para seu colega — defina e-mail e senha de cada um manualmente aqui. Isso garante login individual, não senha compartilhada.

## 3. Ativar Firestore

1. No menu lateral, vá em **Build > Firestore Database**.
2. Clique em "Create database".
3. Escolha "Start in production mode" (as regras de segurança do arquivo `firestore.rules` vão controlar o acesso).
4. Escolha a localização mais próxima (ex.: `southamerica-east1`).

## 4. Aplicar as regras de segurança do Firestore

1. Em **Firestore Database > Regras**, apague o conteúdo padrão e cole todo o conteúdo do arquivo `firestore.rules`. Clique em "Publicar".

## Sobre os anexos

O Firebase mudou a política e passou a exigir o plano pago (Blaze, com cartão cadastrado) para usar o Storage — mesmo que o uso ficasse gratuito na prática. Para não depender de cartão, os anexos deste sistema ficam guardados **dentro do próprio registro no Firestore** (que continua 100% gratuito), em vez de num serviço de armazenamento de arquivos separado.

Isso significa um limite prático: **até 700KB por arquivo** (dá para um PDF de poucas páginas ou uma foto comprimida, mas não serve para vídeo ou imagem em alta resolução). Se no futuro isso for uma limitação real do dia a dia, dá para reavaliar — inclusive migrar para o Storage pago, se o clube decidir que vale o custo.

## 6. Pegar as credenciais do projeto

1. Clique no ícone de engrenagem (⚙) ao lado de "Visão geral do projeto" > **Configurações do projeto**.
2. Em "Seus aplicativos", clique no ícone `</>` (Web) para registrar um app.
3. Dê um apelido (ex.: `prontuario-web`) e clique em "Registrar app". **Não** marque a opção de Firebase Hosting.
4. Copie os valores mostrados (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`).
5. Abra o arquivo `firebase-config.js` (num editor de texto simples, nunca no TextEdit em modo rich text — mesma regra do Baú, para não corromper o arquivo) e cole cada valor no lugar de `"COLE_AQUI"`.

## 7. Criar o repositório no GitHub

1. No GitHub, crie um repositório novo e **privado** (ex.: `prontuario-clube`) — privado porque, mesmo com as regras de segurança, é melhor não deixar o código-fonte público.
2. Vá em "Add file" > "Upload files" e arraste todos os arquivos desta pasta (`index.html`, `atletas.html`, `atleta.html`, `admin-tipos.html`, `style.css`, `firebase-config.js` já preenchido).
   - Não suba `firestore.rules` — ele já foi colado direto no Firebase Console no passo 4, não precisa estar no site.
3. Confirme o commit direto na branch principal.

## 8. Ativar o GitHub Pages

1. No repositório, vá em **Settings > Pages**.
2. Em "Source", escolha a branch principal e a pasta raiz (`/`).
3. Salve. Em alguns minutos, o link aparece no topo da página (pode levar um tempo para propagar, como já acontece com o Baú).

## 9. Testar

1. Acesse o link gerado, faça login com uma das contas criadas no passo 2.
2. Cadastre um atleta de teste.
3. Confira se as quatro abas (Anamnese, Atendimentos, Demandas, Intervenções) aparecem automaticamente na ficha — elas são criadas sozinhas na primeira vez que alguém entra em "Tipos de registro".
4. Peça para seu colega logar com a conta dele e testar a visibilidade: um atendimento que você não marcar como "visível ao colega" não deve aparecer para ele.

## Pendência: transferir a administração para o clube

Hoje o projeto Firebase (`psicologia-cec`) e o repositório GitHub estão registrados na sua conta pessoal. Isso funciona, mas significa que a administração do sistema (mudar regras, adicionar profissional novo, remover acesso de alguém) fica dependendo de você.

Assim que o clube criar um e-mail institucional (ex.: `ti@cuiabaec.com.br` ou o e-mail de quem coordena o departamento), volte aqui e faça:

1. No Firebase Console, vá em **Configurações do projeto > Usuários e permissões**.
2. Clique em "Adicionar membro", cole o e-mail institucional e escolha o papel **Proprietário**.
3. No GitHub, vá em **Settings > Collaborators** do repositório e adicione esse mesmo e-mail (ou uma conta GitHub associada a ele) como colaborador com acesso total.

Isso não tira o seu acesso — só garante que o clube não fica travado se você sair. Não precisa fazer isso agora, mas não esqueça depois que o e-mail existir.

## Redefinição de senha sem depender do console

A tela de login já tem um link "Esqueci minha senha" — quem esquecer digita o e-mail e recebe um link de redefinição direto da Google, sem precisar que você entre no Firebase Console para resetar manualmente.

## Senha do sistema

Diferente do Baú (senha única `eryca2025` para o painel), aqui **cada profissional tem login individual** — não existe senha compartilhada, por causa da exigência de rastreabilidade de quem escreveu cada registro.

## Atualização grande (nova estrutura)

O sistema foi reestruturado: a ficha do atleta agora tem só **Anamnese** (dividida em Parte 1 e Parte 2) e as abas que você criar em "Tipos de registro" (a primeira delas, pré-cadastrada, é "Acompanhamento"). Também foi criada uma seção nova, **"Atendimentos"**, separada da ficha do atleta, para registrar e consultar os atendimentos dos dois profissionais juntos.

Isso exige dois passos antes de usar:

1. **Republicar as regras do Firestore**: cole o conteúdo atualizado de `firestore.rules` em Firestore Database > Regras > Publicar (o arquivo tem trechos novos para as coleções `anamneses`, `atendimentos_meta` e `atendimentos_conteudo`).
2. **Subir os arquivos novos e atualizados no GitHub**: além dos arquivos que já existiam, agora tem `atendimentos.html` e `importar-anamneses.html`, que também precisam estar na mesma pasta do repositório.

## Sobre a importação das anamneses antigas

Vá em "Importar" no menu, exporte a planilha de respostas do Google Forms como CSV (Arquivo > Fazer download > Valores separados por vírgula) e envie o arquivo ali. O sistema tenta casar cada resposta com um atleta pelo nome — cria o atleta se ele não existir, e pula quem já tem anamnese preenchida no sistema, para não sobrescrever nada. O conteúdo original de cada resposta do formulário fica preservado por completo dentro da Parte 2 (em "Observações psicológicas"), mesmo quando não encaixa perfeitamente nos campos novos — nada se perde na importação.

## Adicionando uma aba nova, no futuro

Vá em "Tipos de registro" (link no topo, ao lado de "Atletas"), preencha nome, campos e visibilidade padrão, e clique em "Criar aba". Aparece automaticamente na ficha de todos os atletas — sem precisar subir nenhum arquivo novo no GitHub.
