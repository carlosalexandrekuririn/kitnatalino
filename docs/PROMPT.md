# Prompt padrão – Sistemas de Controle de Entregas

Troque os campos entre colchetes e cole numa conversa nova com o Claude.

| Campo | O que colocar | Exemplo |
| --- | --- | --- |
| [ITEM] | O que será entregue | Kit natalino |
| [CAMPANHA] | Nome e período | Natal 2026, de 01/12 a 20/12 |
| [PONTOS] | Pontos de entrega iniciais | Matriz – Portaria 2; Filial Centro |
| [QTD_POR_PESSOA] | Quantos itens cada colaborador recebe | 1 |
| [PÚBLICO] | Quem tem direito e de onde vem a lista | Todos os colaboradores ativos, planilha do RH |
| [ONDE_RODAR] | claude.ai ou servidor próprio (Firebase/Supabase) | claude.ai |

---

```markdown
Você é um desenvolvedor sênior. Crie um sistema web de controle de entregas de [ITEM] para a Clealco, campanha [CAMPANHA]. Responda e escreva toda a interface em português do Brasil.

## Contexto e problema
As entregas acontecem em 4 ou mais pontos da empresa, divididos entre matriz e filial, e várias pessoas entregam ao mesmo tempo. No passado o controle era feito no Excel: não se sabia quem tinha entregado nem em qual local, e houve extravios. O sistema precisa eliminar isso: cada item tem que ser rastreável de ponta a ponta, com login, local e horário.

Pontos iniciais: [PONTOS]
Cada colaborador recebe: [QTD_POR_PESSOA]
Público: [PÚBLICO]
Onde vai rodar: [ONDE_RODAR]

## Regras de negócio (obrigatórias)
1. Estoque em livro-razão: nada é sobrescrito nem apagado. O saldo de cada ponto é calculado a partir dos movimentos (entrada, entrega, transferência, ajuste, estorno). Erro se corrige com novo lançamento.
2. Cada movimento grava: tipo, ponto de origem (e destino), quantidade, login, nome do usuário, data/hora e observação.
3. Um item por matrícula (ou a quantidade definida), válido em TODOS os pontos. Ao tentar repetir, mostrar onde, quando e quem entregou. Usar trava para evitar duplicidade quando dois pontos registram a mesma matrícula ao mesmo tempo.
4. Transferência entre pontos em um único lançamento (sai da origem e entra no destino), com quem transferiu, quantidade e motivo/responsável pelo transporte.
5. Bloquear entrega, transferência ou ajuste que deixe saldo negativo.
6. Estorno de entrega com confirmação em dois toques: devolve ao saldo, libera a matrícula e mantém o registro original marcado como estornado.
7. Entrega fora da lista importada ou em ponto diferente do previsto é permitida só com confirmação e fica sinalizada no histórico.

## Usuários e acesso
- Login e senha próprios do sistema (senha com hash PBKDF2 + salt). No primeiro acesso, o dono cria o administrador.
- Níveis de acesso configuráveis por permissão: ver painel, registrar entregas, transferir, lançar entrada/ajuste, estornar, ver histórico, gerenciar colaboradores, cadastrar pontos, cadastrar usuários, cadastrar níveis, operar em qualquer ponto.
- Níveis prontos: Administrador (tudo, sempre), Coordenação (painel, estoque, transferências, estornos, histórico, colaboradores, todos os pontos), Entregador (só entregar, só no próprio ponto).
- Usuário vinculado a um ponto de atuação. Desativar usuário sem apagar o histórico dele.

## Telas
- Painel em tempo real (atualiza sozinho, sem recarregar): totais (recebido, entregue, em estoque, transferido, colaboradores atendidos/pendentes); estoque por ponto (recebido, transferências +/−, entregue, pendentes, saldo, saldo zerado em destaque); entregas por usuário/login; lista de transferências com quem transferiu e quantidade.
- Entregar: ponto já selecionado, saldo visível, matrícula com teclado numérico; ao digitar a matrícula, preencher nome e setor da lista; limpar e focar para o próximo após confirmar.
- Transferir, Estoque (entrada com nota fiscal/romaneio, ajuste positivo e negativo com motivo obrigatório).
- Colaboradores: importar Excel (.xlsx, .xls, .csv) com colunas Matrícula, Nome (obrigatórias), Setor e Ponto de entrega (opcionais); reconhecer colunas automaticamente, permitir corrigir o mapeamento, mostrar prévia e contagem de novos/atualizados/ignorados, ponto padrão para quem não tiver; reimportar atualiza sem duplicar; botão para baixar modelo .xlsx; lista com busca e filtro de pendentes/entregues por ponto.
- Histórico: filtros por tipo, ponto, usuário, entregas sinalizadas e busca; exportar CSV (separador ;, UTF-8 com BOM para abrir certo no Excel).
- Cadastros: pontos (nome, matriz/filial, endereço, ativar/desativar), usuários, níveis de acesso.

## Visual e usabilidade
- Identidade Clealco: bordô #780E14, vermelho #EC1D24, amarelo #FCCA0C, laranja #F48514, creme #FCF6D6. Fontes: [usar as do manual de marca, se houver; senão Barlow / Barlow Semi Condensed].
- Modo claro e escuro (Automático, Claro, Escuro) e ajuste de tamanho de letra (P, M, G, GG) que escala a tela inteira; preferências salvas por aparelho, num botão "Aa" no topo.
- Versão mobile de verdade: barra de navegação fixa embaixo com ícones, menu "Mais", tabelas viram cartões, botões e campos de pelo menos 44px, sem rolagem lateral.
- Mensagens claras do que deu errado e como resolver; sem alert/confirm do navegador.

## Entregas que espero
1. O sistema publicado e funcionando com vários usuários ao mesmo tempo.
2. Um manual passo a passo (administrador, entregador e coordenação; importação do Excel; problemas comuns).
3. O código-fonte em arquivo (.html e cópia .txt) e um LEIA-ME explicando arquitetura, coleções do banco e dependências, para eu poder levar a outra IA ou à TI.
4. Ao final, me diga com franqueza o que não foi testado e os limites da solução (acesso, contas, segurança, capacidade).

## Se rodar no claude.ai
- Use o banco compartilhado em tempo real do artifact. Me explique como compartilhar: cada pessoa precisa de conta Claude (pode ser gratuita); fora de uma organização Team, quem registra dados precisa do acesso "Pode editar"; limite de 50 convidados externos; o uso do sistema não consome tokens.

## Se rodar em servidor próprio
- Use Firebase (Firestore + Firebase Auth) ou Supabase, com autenticação e permissões validadas no servidor, e me entregue o passo a passo de instalação para a TI.
```
