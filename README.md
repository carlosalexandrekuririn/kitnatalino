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
| `index.html` | O app completo (HTML, CSS e JS num arquivo) |
| `docs/ARQUITETURA.md` | Coleções do banco, dependências e como migrar |
| `docs/PROMPT.md` | Prompt padrão para criar novos sistemas de entrega |

## Como roda hoje
O app está publicado como Artifact no claude.ai e usa o banco em tempo real da plataforma (`claude.use('db')`, `'user'`, `'downloads'`). **Aberto direto no navegador, ele não carrega os dados.** Para rodar em servidor próprio, troque essa camada por Firebase (Firestore + Auth) ou Supabase. O passo a passo está em `docs/ARQUITETURA.md`.

## Segurança
Hoje as senhas (PBKDF2 + salt) e as permissões são validadas no navegador. Isso basta para controle interno. Numa versão em servidor próprio, mova a autenticação e as regras para o backend.
