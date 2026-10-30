# Como colocar o sistema no ar (GitHub Pages + Firebase)

Tempo: cerca de 30 minutos. Plano gratuito (Spark) do Firebase é suficiente para a campanha.

## 1. Criar o projeto no Firebase
1. Acesse https://console.firebase.google.com e clique em **Adicionar projeto**. Nome sugerido: `kits-clealco`.
2. Desative o Google Analytics (não é necessário).

## 2. Ativar o login por e-mail e senha
1. No menu, **Authentication › Começar**.
2. Aba **Método de login › E-mail/senha › Ativar** e salve.

## 3. Criar o banco de dados
1. No menu, **Firestore Database › Criar banco de dados**.
2. Escolha o modo **produção** e a região **southamerica-east1 (São Paulo)**.
3. Aba **Regras**: apague o conteúdo, cole o conteúdo do arquivo `firestore.rules` deste repositório e clique em **Publicar**.

## 4. Pegar a configuração do app
1. Em **Configurações do projeto** (ícone de engrenagem) › **Seus apps** › ícone **</>** (Web).
2. Dê um apelido (ex.: `kits-web`), não marque hospedagem do Firebase e registre.
3. Copie o objeto `firebaseConfig`.
4. No arquivo `index.html`, substitua o bloco `FIREBASE_CONFIG` pelos valores copiados (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId). Faça commit.

## 5. Publicar no GitHub Pages
1. No repositório, **Settings › Pages**.
2. Em **Build and deployment**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e salve.
3. Em alguns minutos o endereço fica disponível: https://carlosalexandrekuririn.github.io/kitnatalino/

## 6. Primeiro acesso (importante)
Abra o link e **crie o administrador imediatamente**. Quem abrir o link antes da criação do administrador pode criar esse acesso. Depois disso, o sistema só aceita novos usuários pelo próprio administrador.

Depois: cadastre os pontos, confira os níveis, cadastre os usuários e importe a lista de colaboradores.

## Senhas
- Cada usuário entra com **login e senha**. O login vira internamente o e-mail `login@sistema-kits.net`, que é um domínio fictício (não recebe e-mails).
- **Esta versão não troca senha de ninguém pelo app.** Se alguém esquecer a senha, o administrador desativa esse usuário e cria outro login. Para troca de senha com botão no sistema, é preciso uma função de servidor (Cloud Functions, plano Blaze), que pode ser feita numa próxima etapa.

## Limites do plano gratuito
- Leituras: 50 mil por dia. Cada vez que alguém abre o painel, o app lê as listas inteiras. Para dezenas de pessoas usando ao longo do dia, fica bem abaixo do limite; se a lista de colaboradores e o histórico crescerem muito, avise para ajustarmos.
- Gravações: 20 mil por dia, mais que suficiente para entregas.
- Armazenamento: 1 GB.

## Segurança
- As regras do Firestore (`firestore.rules`) valem no servidor: quem não está logado, ou não tem a permissão do nível de acesso, não consegue gravar, mesmo mexendo no navegador.
- Quem gera o acesso é o próprio Firebase Auth; a senha nunca fica no código.
- O código do app (incluindo a configuração pública do Firebase) fica visível no GitHub. Isso é normal no Firebase: a segurança está nas regras, não em esconder o código.
