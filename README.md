# Life Atlas — protótipo de organizador pessoal

Website **estático e responsivo** em português, preparado para GitHub Pages. Inclui um **blog editorial independente** em inglês (`blog.html`).

> A versão entregue é um protótipo funcional local. A ligação a Google Calendar / Google Drive exige configuração OAuth por ti e autorização Google. Não inclui IA generativa nem backend privado. Não foi publicado na tua conta GitHub.

## O que funciona sem configurar Google

- **Hoje**: tarefas, concluído/por fazer, instruções práticas e propostas de próximos passos.
- **Calendário**: dia, semana, mês e ano, com navegação por datas.
- **Objetivos** por área e histórico de tarefas concluídas em cada área.
- **Planos & guias** com passos para Anatomia, Bioquímica, Fisiologia, BMC, Medicina Clínica, treinos de principiantes, ginástica supervisionada, francês, representação, portefólio, vídeos e jantares temáticos.
- **Ideias & listas**: presentes, refeições, viagens, ideias de conteúdo, etc. Podem virar tarefas datadas.
- **Reagendamento** individual para amanhã e recuperação de até duas tarefas flexíveis atrasadas por dia, sem esconder compromissos importantes.
- **Personalização**: energia e minutos disponíveis, usados pelas sugestões de atividades; calendário externo quando conectado também reduz o número de sugestões nos dias mais cheios.
- **Guardar dados** no próprio navegador (`localStorage`) e importar/exportar cópias JSON.

## Como abrir / testar

Abrir `index.html` num browser normal. Para permitir autenticação Google, o site tem de estar alojado num domínio HTTP(S) autorizado (tipicamente GitHub Pages com HTTPS) ou localhost configurado. **A ligação Google não funciona em `file://`.**

## Publicar grátis no GitHub Pages

1. No GitHub, criar um repositório, por exemplo `life-atlas`.
2. Carregar `index.html`, `styles.css`, `app.js`, `icon.svg`, `blog.html`, `README.md` na **raiz** do repositório.
3. Em **Settings → Pages**, escolher **Deploy from a branch**, `main` e pasta `/ (root)`; guardar.
4. O endereço terá a forma `https://UTILIZADOR.github.io/life-atlas/` (alterar UTILIZADOR para o teu nome de utilizador real).
5. Abre a página no iPad, iPhone ou computador. Podes adicionar o endereço ao ecrã inicial do Safari para acesso rápido.

**Segurança:** GitHub Pages serve o site em URL público por defeito. Mesmo se o repositório de origem for privado nalguns planos, isso não garante que a página seja privada. Não colocar dados pessoais, tokens, passwords, credenciais OAuth com `client_secret` ou ficheiros JSON com dados privados no repositório. O código HTML e JS deste protótipo não contém agenda ou contactos pessoais importados. Usa o blog separadamente e só publica o que decidires divulgar.

## Sincronizar com Google Calendar e Google Drive (opcional)

Esta versão liga-se diretamente às APIs oficiais do Google através da **Google Identity Services JavaScript library**. São necessárias credenciais OAuth para um website — um `client_id` público, **nunca** um `client_secret`. A app solicita:

- `https://www.googleapis.com/auth/calendar.readonly` — ler calendários e eventos.
- `https://www.googleapis.com/auth/drive.appdata` — guardar um ficheiro `life-atlas.json` na pasta de dados privada da aplicação no Google Drive.

### Preparação

1. Abrir [Google Cloud Console](https://console.cloud.google.com/), criar/selecionar um projeto teu.
2. Em **APIs & Services / Library** (ou interface equivalente), ativar **Google Calendar API** e **Google Drive API**.
3. Configurar **Google Auth Platform / OAuth consent screen**. Para uma aplicação pessoal, manter em modo *Testing* e adicionar a tua conta como utilizadora de teste, se aplicável. Estes âmbitos podem exigir aprovação/verificação para disponibilização pública a outras pessoas.
4. Criar um **OAuth client ID** do tipo **Web application**.
5. Em **Authorized JavaScript origins**, adicionar exatamente o domínio HTTPS: `https://UTILIZADOR.github.io` (sem o caminho `/life-atlas/`). Se usares domínio próprio, também adicionar a origem desse domínio. Para testar em localhost, adicionar a origem local correta.
6. Copiar **apenas o Client ID**, terminado em `.apps.googleusercontent.com`.
7. Abrir o site publicado → **Definições** → colar Client ID → **Guardar Client ID** → **Ligar conta Google** → conceder permissões. Selecionar os calendários que queres ver.
8. Em cada outro dispositivo, introduzir o mesmo Client ID e autorizar a mesma conta Google. O planeador recupera e guarda os dados na pasta da aplicação da tua conta Google.

### Atualização e limites

- Ao estar **aberto e autorizado**, o calendário é atualizado quando ligas a conta, carregas em *Atualizar calendário*, quando regressas à página e aproximadamente de 5 em 5 minutos.
- Se alterares um compromisso no Google Calendar, ele surgirá após a próxima atualização. **Não é instantâneo nem recebe webhooks em segundo plano** e não funciona com a página fechada.
- **A integração do calendário é só de leitura:** adicionar uma tarefa no Life Atlas **não** cria um evento no Google Calendar. Uma integração bidirecional exigiria permissões extra, desenho de sincronização e testes cuidadosos para evitar duplicações.
- Dados do planeador são enviados para a `appDataFolder` apenas depois de dares autorização Google. A primeira ligação pode recuperar uma versão já guardada. Recomenda-se exportar JSON antes da primeira sincronização ou em caso de conflito. O mecanismo fornecido é básico e **não é um sistema multiutilizador nem tem resolução avançada de conflitos entre vários dispositivos abertos em simultâneo**.
- Os dados `localStorage` **não são cifrados**. Se partilhares o dispositivo/browser, alguém com acesso local poderá conseguir aceder-lhes. Para dados especialmente íntimos ou de saúde, usar uma solução com autenticação e cifragem adequadas, idealmente com backend.
- O Google Identity Services OAuth token é mantido apenas em memória durante a sessão; após expirar, tens de voltar a autorizar. **Nunca guardar tokens de acesso no GitHub.**

## Como melhorar para uma V2

1. Login verdadeiro obrigatório antes de apresentar dados (backend com autenticação e regras de acesso) e backups com proteção de conflitos.
2. Integração Calendar bidirecional (opcional) e sincronização quase em tempo real via backend.
3. Planeador inteligente com restrições horárias, períodos de exames, treino progressivo e memória de desempenho.
4. Diário de treino/estudo e revisões espaçadas a partir dos erros e dos temas escolhidos.
5. Publicação de posts do blog com CMS e revisão editorial, mantendo blog e área privada isolados.
6. IA personalizada via serviço seguro no backend; **nunca inserir chave de API de IA no JavaScript publicado no GitHub Pages**.

## Ficheiros

- `index.html` — dashboard
- `styles.css` — design responsivo
- `app.js` — lógica e armazenamento local, conexão opcional Google
- `blog.html` — conceito independente de blog editorial
- `icon.svg` — favicon
- `README.md` — instalação e limites
