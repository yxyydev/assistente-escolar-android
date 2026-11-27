# Assistente Escolar — versão instalável para Android

App de notas (AC, AE, AB, RP, Resumo anual e Recuperação final) que funciona **offline**, instala no Android
e **sincroniza entre aparelhos usando o seu Google Drive**.

Arquivos desta pasta: `index.html` (o app), `manifest.webmanifest`, `sw.js` (offline), `icon-*.png`.

## Como os dados ficam no Drive
- O app cria **um arquivo visível no seu Meu Drive**: `assistente-escolar-dados.json`.
- Ele só enxerga arquivos que **ele mesmo criou** (permissão `drive.file`). Não lê o resto do seu Drive.
- Cada aparelho guarda também uma cópia local. A sincronização é entre o aparelho e esse arquivo.

---

## Passo 1 — Colocar o app na internet (grátis)
O Android só instala o app se ele estiver em um endereço `https`. Escolha uma opção:

**Opção A: Netlify Drop (mais fácil)**
1. Acesse `app.netlify.com/drop` e entre com sua conta.
2. Arraste **a pasta inteira** (com todos os arquivos) para a página.
3. Anote o endereço gerado, por exemplo `https://nome-aleatorio.netlify.app`.

**Opção B: GitHub Pages**
1. Crie um repositório público e envie todos os arquivos desta pasta.
2. Em *Settings → Pages*, escolha a branch `main` e a pasta `/ (root)`.
3. O endereço será `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

Anote o endereço: você vai usá-lo no Passo 2 (apenas a parte inicial, `https://...`, sem caminho).

## Passo 2 — Criar o "Client ID" do Google (uma vez só)
Isso autoriza o app a usar o seu Drive. Os nomes dos menus podem variar um pouco.
1. Acesse `console.cloud.google.com` e crie um projeto (ex.: "Assistente Escolar").
2. Em *APIs e serviços → Biblioteca*, procure **Google Drive API** e clique em **Ativar**.
3. Em *Tela de permissão OAuth* (ou *Google Auth Platform*): tipo **Externo**, informe nome do app e seu e-mail.
   Em *Escopos*, adicione `.../auth/drive.file`. Em *Usuários de teste*, adicione **o seu e-mail do Google**.
4. Em *Credenciais → Criar credenciais → ID do cliente OAuth*: tipo **Aplicativo da Web**.
   Em **Origens JavaScript autorizadas**, adicione o endereço do Passo 1 (ex.: `https://nome-aleatorio.netlify.app`
   ou `https://SEU-USUARIO.github.io`), sem barra no final e sem caminho.
5. Copie o **ID do cliente** (termina com `.apps.googleusercontent.com`).

Dica: se o Google pedir para autorizar de novo a cada poucos dias, volte à tela de permissão e clique em **Publicar app**.

## Passo 3 — Instalar no Android
1. No Chrome do celular, abra o endereço do Passo 1.
2. Toque em **⋮ → Instalar app** (ou *Adicionar à tela inicial*).
3. O ícone "Assistente" aparece na tela inicial e abre em tela cheia.

## Passo 4 — Conectar o Google Drive
1. No app, toque em **Backup**.
2. Na seção *Google Drive*, cole o **ID do cliente** e toque em **Conectar e sincronizar**.
3. Autorize com a sua conta Google. Pronto: o arquivo é criado no seu Drive.
4. Nos outros aparelhos, instale o app, cole **o mesmo ID do cliente** e conecte com **a mesma conta**.

## Como a sincronização funciona
- Com *Sincronizar automaticamente* ligado, o app envia as alterações ~15 segundos depois de você editar
  e confere se há novidades quando você volta ao app.
- A autorização do Google vale cerca de **1 hora**. Depois disso, toque no ícone **☁** (canto superior direita)
  para reconectar. É só um toque.
- Se dois aparelhos mudaram dados diferentes, o app mostra **"Dados diferentes no Drive"** e você escolhe
  qual manter. Os dados substituídos ficam guardados no aparelho (`assistente-pre-sync`).
- **Salvar cópia datada** cria um arquivo extra no Drive (`assistente-escolar-copia-AAAA-MM-DD-HHMM.json`).
- O botão **Backup** continua oferecendo o texto para copiar e restaurar manualmente.

## Trazer os dados da versão do claude.ai
1. Na versão antiga: **Backup** → copie o texto da caixa.
2. Na versão instalada: **Backup** → cole o texto na caixa → **Restaurar**.
3. Depois conecte o Drive (Passo 4).

## Opcional — Arquivo APK (instalar por arquivo ou Play Store)
Com o app hospedado (Passo 1), use `pwabuilder.com`: cole o endereço, escolha *Android* e baixe o pacote.
Para instalar o APK direto é preciso permitir "fontes desconhecidas". A Play Store exige conta de desenvolvedor.

## O que não existe nesta versão
- Link de compartilhamento das notas e IA na importação (dependem do claude.ai).
  A importação de PDF funciona pela leitura própria do app, inclusive o "Quadro de notas".
- Backup automático por conector do Claude (substituído pela sincronização com o Drive).

## Atualizar o app
Troque os arquivos no site e, no `sw.js`, mude `assistente-v1` para `assistente-v2` (e assim por diante),
para os aparelhos baixarem a nova versão.
