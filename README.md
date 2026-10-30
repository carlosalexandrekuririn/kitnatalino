# Controle de Kits Natalinos (Clealco)

App web para controlar a entrega de kits natalinos em vários pontos da empresa (matriz e filiais), com vários usuários ao mesmo tempo e painel em tempo real.

## Funcionalidades
- **Estoque em livro-razão** por ponto de entrega: entrada, entrega, transferência, ajuste e estorno. Nada é apagado.
- **Um kit por matrícula**, válido em todos os pontos, com trava contra registro duplicado.
- **Transferências entre pontos** registrando quem transferiu, a quantidade e o motivo.
- **Painel em tempo real**: estoque por ponto, entregas por usuário, transferências e pendentes.
- **Importação de colaboradores** por Excel (.xlsx, .xls, .csv), com prévia e modelo para baixar.
- **Histórico** com filtros, estorno e exportação em CSV.
- **Acesso**: login próprio, níveis de acesso por permissão e usuário vinculado a um ponto.
- **Aparência**: modo claro e escuro, ajuste de fonte (P/M/G/GG) e layout para celular com barra inferior.
- **Visual** com a paleta da marca Clealco.

## Estrutura
| Arquivo | Conteúdo |
| --- | --- |
| `index.html` | O app completo (HTML, CSS e JS num arquivo), versão Firebase |
| `firestore.rules` | Regras de segurança do banco (cole no console do Firebase) |
| `docs/FIREBASE.md` | Passo a passo para colocar no ar |
| `docs/versao-claude-ai.html` | Versão para rodar no claude.ai |
| `docs/ARQUITETURA.md` | Coleções do banco, dependências e como migrar |
| `docs/PROMPT.md` | Prompt padrão para criar novos sistemas de entrega |

## Como roda
O app roda no **GitHub Pages** e usa o **Firebase** (Firestore para os dados e Authentication para o login). Não precisa de conta Claude.

Antes de abrir o link, siga **[docs/FIREBASE.md](docs/FIREBASE.md)**: criar o projeto, preencher `FIREBASE_CONFIG` no `index.html`, publicar as regras de `firestore.rules` e criar o administrador.

A versão que roda dentro do claude.ai (com o banco da própria plataforma) está em `docs/versao-claude-ai.html`.

## Segurança
Os logins são do Firebase Authentication, e as permissões são validadas pelas regras do Firestore (`firestore.rules`), no servidor. A interface esconde o que o usuário não pode usar, mas quem decide é a regra.
