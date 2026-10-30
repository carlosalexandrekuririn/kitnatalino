# Controle de Kits Natalinos (Clealco): contexto para outra IA

Leia este texto antes de analisar o arquivo `index.html` (ou `index.txt`, que tem o mesmo conteúdo).

## O que é
Um app web, todo num único arquivo HTML, para controlar a entrega de kits natalinos em 4 ou mais pontos da empresa, divididos entre matriz e filial. Vários usuários usam ao mesmo tempo, e a coordenação acompanha tudo em tempo real.

## Funcionalidades
- Login próprio (login e senha) e níveis de acesso configuráveis, com permissões por categoria: painel, entregar, transferir, estoque, estornar, histórico, colaboradores, pontos, usuários, perfis e todosLocais.
- Cadastro de pontos de entrega (matriz/filial), de usuários e de níveis de acesso.
- Estoque em livro-razão. Nada é apagado: o saldo de cada ponto é calculado a partir dos movimentos (entrada, entrega, transf, ajuste, estorno).
- Entrega com trava de um kit por matrícula, válida em todos os pontos.
- Transferência entre pontos registrando quem transferiu e a quantidade.
- Painel em tempo real: estoque por ponto, entregas por usuário, transferências e pendentes.
- Importação de colaboradores por Excel ou CSV, usando a biblioteca SheetJS (xlsx 0.18.5, carregada pelo cdnjs).
- Histórico com filtros, estorno e exportação em CSV.
- Modo claro e escuro, ajuste de tamanho de fonte (P/M/G/GG) e layout para celular com barra inferior.
- Cores da marca Clealco: bordô #780E14, vermelho #EC1D24, amarelo #FCCA0C, laranja #F48514 e creme #FCF6D6.

## DEPENDÊNCIA IMPORTANTE: plataforma claude.ai
O app foi feito para rodar como "Artifact" no claude.ai e usa uma API dessa plataforma, que não existe fora dela:

```js
const db   = await claude.use('db');        // banco de documentos em tempo real (parecido com o Firestore)
const user = await claude.use('user');      // identidade do visitante na plataforma
const dl   = await claude.use('downloads'); // salvar arquivo (CSV, modelo .xlsx)
```

Métodos do `db` que o app usa (sintaxe parecida com a do Firestore):
- `db.collection(nome).onSnapshot(fn, errFn)` e `db.collection(nome).add(obj)`
- `db.doc('col/id').set(obj)`, `.update(obj)`, `.delete()` e `.get()`, que retorna `{exists, data()}`
- `db.doc('col/id').acquire({holder, ttlMs})`, uma trava curta usada para evitar entrega duplicada da mesma matrícula

Coleções:
- `locais`: {nome, tipo: 'matriz'|'filial', endereco, ativo}
- `usuarios`: o id é o login; {nome, login, salt, hash (PBKDF2-SHA256), perfilId, localId, ativo}
- `perfis`: {nome, permissoes: {chave: bool}}. O perfil `admin` sempre tem todas as permissões.
- `colaboradores`: id `m-<matricula>`; {matricula, nome, setor, localId}
- `movimentos`: {tipo, origem, destino?, qtd, matricula?, colab?, obs, por, porNome, porLogin, ts, estornado?, foraLista?, foraPonto?}
- `matriculas`: id `m-<matricula>`; marca quem já recebeu o kit (movId, localId, por, ts)

Para rodar fora do claude.ai, troque essas três chamadas por um backend real. O Firebase/Firestore é o mais próximo e exige menos mudanças; o Supabase também serve. Recomendado: passar a autenticação para o servidor (Firebase Auth / Supabase Auth). Hoje a senha é validada no navegador, o que é aceitável só para controle interno. O resto do código (telas, regras, cálculos) pode ser mantido.
