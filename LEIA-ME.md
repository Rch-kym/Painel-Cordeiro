# Painel Cordeiro

Um único arquivo (`index.html`). Não precisa de servidor próprio nem de instalação.

## Como publicar (GitHub Pages, grátis)
1. No GitHub, crie um repositório novo (pode ser privado se o seu plano permitir Pages privado; senão público, sem problema: quem não tem login não vê dado nenhum).
2. Envie o `index.html` para a raiz do repositório.
3. Settings > Pages > Source: "Deploy from a branch" > main > / (root) > Save.
4. Em 1 a 2 minutos o endereço aparece ali (algo como `https://SEU-USUARIO.github.io/NOME/`). Esse é o link do painel.

## Quem vê o quê
- **Administrador do banco de dados** (papel "dono" no banco): tudo, inclusive Equipe (ativar pessoas, papel, meta) e Locais por rota (edita).
- **monitor**: tudo da operação de todos os colaboradores, mais Cobranças; vê Locais, mas não edita.
- **bipador**: só os próprios bipes.

## Segurança
- O que protege os dados são as regras do banco (RLS), não o endereço do painel. A chave que está no arquivo é a "publishable", feita para isso.
- No Supabase, em Authentication > Sign In / Providers, deixe **"Allow new users to sign up" desligado**. Assim só entra quem o administrador criou.
- Nunca coloque no arquivo a chave `secret`/`service_role` nem a senha do banco.

## Novas pessoas
Supabase > Authentication > Users > Add user (e-mail + senha, marque "Auto Confirm"). Depois, no painel > Equipe, ative e escolha o papel.

## Versão 1.3 — novo layout (mockup)
Menu: Painel do dia, Acareações, Motoristas e rotas, Bipagem e histórico (Produtividade, Erros e divergências, Ficha do pacote), Não recebidos, Backlog, Fechamento. Em Administração: Cobranças, Locais por rota, Equipe.
Acareações, Não recebidos e Backlog ainda mostram um aviso: esses dados ficam só na extensão e ainda não são enviados ao banco.

## Versão 1.4 — Não recebidos e Backlog
Rode antes o SQL **05-nao-recebidos-backlog.sql** no Supabase. As abas Não recebidos (lista com achados em verde) e Backlog (foto da última busca completa, por motorista) leem os dados enviados pela extensão e pelo app.

## Versão 1.5
- Não recebidos atualiza sozinho a cada 15 s e o Backlog a cada 20 s (só redesenha se chegou algo novo). Não precisa mais clicar em Atualizar.

## Versão 1.6 — Backlog interativo
- Os cinco cartões viram filtros: toque em "Mais de 10 dias" (risco), "Erros de rota", "Sem motorista" ou "3+ dias" para ver só aqueles pacotes. Toque de novo para desligar.
- Filtro por cidade, busca, faixas de dias e "Limpar filtros".
- Dois modos: Por motorista (abre vários ao mesmo tempo, "Abrir todos") e Lista de pacotes (mais antigo primeiro; toque no nome do motorista para ir ao grupo dele).
- Ordenar por nome, pacotes, 3+ dias ou mais antigo (cabeçalhos da tabela).
- "Copiar cobrança (3+ dias)" gera a mensagem pronta para o motorista; "Copiar códigos".
- O CSV leva só o que está filtrado.

## Versão 1.7
- Tela de entrada: subtítulo "Sistema de gestão e acompanhamento." e a logo da Cordeiro logo abaixo dele.

## Versão 1.8
- Ícone da aba do navegador: logo da Cordeiro. Se continuar aparecendo o ícone antigo, aperte Ctrl+F5 (o navegador guarda o ícone em cache).

## Versão 1.9 — Acareações
- A aba Acareações agora mostra o histórico enviado pela extensão (3.16.0): cartões que filtram (enviadas, com problema, prontas, com observação), período, busca, visão por quem deu a baixa e CSV. Atualiza sozinha a cada 20 s.
- Requer o script 06 no Supabase.

## Versão 1.9.1
- A barra lateral mostra "Painel 1.9.1" no rodapé, para conferir qual versão está publicada.

## Versão 1.10 — Tela de entrada animada
- O canto esquerdo da entrada ganhou um mapa esquemático animado: Teixeira de Freitas como base (com ondas), rotas pontilhadas para as cidades da região, pacotes viajando pelas rotas e um caminhão passando pela estrada. Respeita a opção "reduzir movimento" do sistema.

## Versão 1.11
Nova tela de entrada: nome gigante "CORDEIRO" ao fundo, mapa animado de rotas, barra de login de vidro na parte de baixo e caminhão na estrada. Publique a pasta interna `painel-cordeiro` e dê Ctrl+F5; o rodapé do menu deve mostrar "Painel 1.11".

## Versão 1.12 e 1.13
O visual por dentro agora é corporativo e sóbrio: fundo cinza-azulado frio, menu azul-marinho liso com o item ativo marcado por uma barra âmbar, títulos menores, cartões brancos com borda fina, tabelas com cabeçalho em maiúsculas pequenas e a lista "Precisa de atenção" com barra vermelha na lateral. O tema claro/escuro automático e o celular continuam funcionando. Publique a pasta interna `painel-cordeiro` e dê Ctrl+F5; o rodapé do menu deve mostrar "Painel 1.13".

## Versão 1.13.1
Equipe: ao salvar, o painel agora confere se o banco realmente aplicou a mudança. Se não aplicou, mostra o motivo (em vez de parecer salvo), e quando dá certo a lista é redesenhada na hora (some a etiqueta "Aguardando ativação").

## Versão 1.14
Não recebidos: abas **Não encontrados** (vermelho) e **Encontrados** (verde), agrupamento e filtro por **cidade** (cada cidade mostra quantos faltam e quantos foram encontrados) e **tempo real**: quando alguém acha um pacote no app, a tela atualiza na hora e mostra o aviso "Pacote encontrado". Para o tempo real funcionar, rode antes o script `07-tempo-real.sql` no Supabase. Sem ele, o painel continua atualizando a cada 30 segundos. Publique a pasta interna `painel-cordeiro` e dê Ctrl+F5; o rodapé do menu deve mostrar "Painel 1.14".

## Versão 1.15
- **Erros e divergências** ganhou o estado **Corrigido** (chave Pendente ↔ Corrigido por ocorrência; abre em "Pendentes"; cartões de totais; o CSV traz Corrigido, Corrigido por, Corrigido em e Forma). Quando o colaborador passa o pacote para a rota/motorista certo no app, o banco marca a divergência como corrigida sozinho (selo **AUTO**) e "ver prova" mostra a ocorrência, a reatribuição manual e o resultado. A reatribuição continua manual. Você pode desmarcar; a decisão manual trava a linha.
- **Produtividade** passa a contar só **atribuições** (chegadas ficam de fora), com **sacas fechadas** e **saca agora** (cadeado) por colaborador.
- Nova aba **Sacas**: totais, sacas em andamento com cadeado, tabela com filtros e CSV, e ao clicar numa saca a lista de pacotes dela; ao clicar num pacote, o detalhe (produto, bairro, endereço, CEP, histórico).
- O **produto** do pacote aparece no detalhe quando alguém já tocou em "Mostrar produto" no app; senão o painel indica abrir no aplicativo.
- Tempo real nas sacas e nas correções.
Antes: rode `08-sacas-corrigido-produto.sql` no Supabase. Publique a pasta interna `painel-cordeiro` e dê Ctrl+F5; o rodapé do menu deve mostrar "Painel 1.15".

## Versão 1.15.1
**Produtividade** voltou a ser um item próprio do menu lateral (antes só abria por dentro de "Bipagem e histórico"). "Bipagem e histórico" agora abre em Erros e divergências. Rodapé: "Painel 1.15.1".

## Versão 1.17 — Meta do dia
Nova tela **Meta do dia** (menu, logo acima de Chegada e conciliação).
- Regra: universo = pacotes que **chegaram no dia**; meta = **95%** do universo (padrão). Atribuídos = pacotes da chegada do dia já atribuídos; "A expedir" = universo − atribuídos. Dias anteriores não entram.
- Mostra % atribuído, meta em pacotes, quanto falta, ritmo por hora até o horário de corte (hoje) e atribuídos por motorista.
- Dono e monitor podem **ajustar o dia à mão**: percentual, ± em chegaram, ± em atribuídos, horário de corte e motivo. Fica registrado quem ajustou.
- Campos "Chegada no DS" e "A expedir no DS": você digita o que o DS mostra; o painel avisa se divergir do nosso cálculo.
- Sacas › "Quem está fazendo o quê agora": cartões corrigidos (e-mail longo não empurra mais o número para fora; nome da saca em até 2 linhas; altura igual; horários numa linha). E-mail no lugar do nome aparece só até o @ (nome completo ao passar o mouse).
- Revisão geral de UX/UI: gráfico "Bipagens por hora" (número do pico não fica mais sobre a barra); tabela de Sacas cabe com a gaveta ao lado (nada de palavra quebrada no meio nem coluna cortada) e, no celular, tocar numa saca rola até os detalhes; Meta do dia com 4 cartões e colunas alinhadas; nome na lateral sem o @.
- Nome na tela: se o cadastro em Equipe tem e-mail no campo Nome, o painel mostra o **Nome no iMile** (ex.: Anna, Lavine). Para mudar de vez, edite o campo Nome em Equipe.
- **Rode o `10-meta-dia.sql` no Supabase** (depois do 09). Sem ele a tela funciona com a regra padrão, mas não salva ajustes.

## Versão 1.16
- Novo menu **Chegada e conciliação**: tempos da chegada (primeiro bipe, último bipe e tempo total, com ritmo por minuto), totais (chegaram / atribuídos / faltam) e as cidades. Cidade com 100% atribuído mostra **✓ Concluída**; as demais mostram **Faltando N** e, ao abrir, cada pacote com AWB, endereço completo, bairro, CEP, hora da chegada e quem bipou. Pacotes sem cidade ficam em "Sem cidade". Hoje / Ontem / escolher dia, CSV e tempo real.
- **Sacas** mostram Início, Último bipe e Tempo total (cartões, tabela, CSV e total do dia). A contagem de cada saca vale pela última atribuição do pacote (quando passa para outro motorista, sai de uma saca e entra na outra).
- **Locais por rota** em tempo real: adicionar, editar e excluir pelo painel (qualquer pessoa ativa), com confirmação "Tem certeza que deseja fazer essa alteração?" e o nome de quem alterou; alterações dos celulares aparecem na hora.
Antes: rode `09-chegada-conciliacao-locais.sql` no Supabase (depois do 08). Publique a pasta interna `painel-cordeiro` e dê Ctrl+F5; o rodapé do menu deve mostrar "Painel 1.16".

## Versão 1.18 - Revisão geral de UX/UI

- Navegação agrupada, busca de seções e atalho Ctrl/Cmd+K.
- Menu lateral acessível no celular, com fechamento por botão/Escape.
- Resumo da meta da chegada do dia no Painel do dia, com ritmo até o corte.
- Metas individuais e contagens por etapa claramente separadas; retirada da comparação inadequada de “meta batida”.
- Nova hierarquia visual, cartões, espaçamento e tabelas em temas claro/escuro.
- Proteção de edições não salvas, foco de confirmação e tratamento de telas menores.
- Busca em Locais e Equipe; limpeza e exportação do filtro em Não recebidos.
- Retorno da ficha do pacote para a consulta anterior e prevenção de buscas antigas sobrepondo novas.
- Prevenção de registro repetido por duplo clique em Cobranças e validação de ajustes.

Publique o index.html desta pasta no lugar do anterior. Dê Ctrl+F5 e confirme “Painel 1.18” no rodapé. Esta revisão não exige SQL novo; permanecem os requisitos da 1.17. O relatório detalhado está em ../documentacao/ANALISE-UX-UI.md.

Validado com dados simulados e verificações de código. A autenticação, a escrita e o tempo real precisam de validação no banco real do usuário. Nenhum dado de teste está incluído no index.html publicável.

## Versão 1.18.2 — fechar sacas pelo painel
- Em **Sacas**, quem é administrador ou monitor vê o quadro "Alguém esqueceu de fechar uma saca?": **Fechar sacas de dias anteriores**, **Fechar sacas paradas**, **Fechar todas as abertas** e, na gaveta de cada saca, **Fechar esta saca**. Sempre pede confirmação.
- A saca fechada à mão recebe a hora do **último bipe** (não a hora atual), então o tempo dela não fica inflado. Se estava em andamento, fecha na hora do clique.
- Não precisa de SQL novo: as regras do banco (08) já permitem ao administrador/monitor fechar.
- **Chegada (sem motorista)**: não é saca de verdade. Era criada por engano pelo app quando um bipe de chegada repetido aparecia. O painel agora tira essas sacas dos números e oferece "Limpar sacas de chegada" para fechá-las. O app 1.14.1 para de criá-las.

## Versão 1.18.3 — Locais por rota
- A lista de rotas do campo "Rota" só mostrava rotas parecidas com o texto já escrito (ex.: com "Rota 2" aparecia só "Rota 2"). Agora há um seletor ▾ ao lado do campo com **todas** as rotas; escolher uma preenche o campo. Digitar uma rota nova continua funcionando.

## Versão 1.18.4 — alerta de saca fechada com pacote faltando
- Quando **todas as sacas de uma cidade** do dia estão fechadas e a conciliação ainda mostra pacotes que **chegaram e não foram atribuídos**, aparece uma faixa vermelha no topo (em qualquer tela) e um aviso na tela, com botão **Ver pacotes que faltam** (abre Chegada e conciliação). **Dispensar** esconde o alerta até o número de pacotes faltando mudar.
- Confere sozinho a cada minuto e na hora em que uma saca é fechada ou um pacote é bipado.
- **Teixeira de Freitas fica de fora** (tem várias rotas). A lista está em `CIDADES_SEM_ALERTA_SACA` no início do bloco "Alerta".

## Versão 1.19.0 — Risco de extravio
**Rode antes o SQL `12-extravio.sql` no Supabase.** Nova tela **Risco de extravio** (Operação; administrador e monitor), widget no Painel do dia e alerta global.
- Fonte: a última busca completa do **Backlog** (campo "dias"). Se o Backlog tiver mais de 24 h, o painel avisa.
- 🟡 **Nível 1 · Atenção (4+ dias parados):** fila de **Reatribuição obrigatória** (individual ou em lote, com seleção). Ações: copiar códigos, "Reatribuição feita", registrar tratativa.
- 🔴 **Nível 2 · Crítico (5+ dias em rota, sem baixa, sem tratativa):** faixa vermelha fixa no topo (pulsa), janela de aviso ao abrir, som (a cada 15 min enquanto existir) e notificação do navegador (botão "Ativar notificações"). Ações: copiar cobrança por motorista, registrar tratativa (nota obrigatória).
- **Sai do alerta** quando: some do Backlog (baixa), recebe nova atribuição depois da busca, ou ganha tratativa justificada (1, 2, 3 ou 5 dias; depois disso, se continuar parado, volta) — cobrança enviada nos últimos 2 dias também conta.
- **Rotina de segunda, quarta e sexta:** o status aparece no Painel do dia ("Pendente hoje", "Atrasada desde segunda", "Concluída — próxima quarta (em 2 dias)"). **Concluir auditoria de hoje** registra quem fez e quantos ficaram sem tratativa.
- A reatribuição em si continua sendo feita no app/extensão; o painel não escreve no iMile.

## Versão 1.20.0 — Reatribuição automática (controle)
**Rode antes o SQL `13-reatribuicao-auto.sql`.** Em Risco de extravio, o cartão **Reatribuição automática** mostra a última rodada da extensão (simulados/reatribuídos, falhas, avisos) e, para o administrador, deixa escolher modo (Desligada, Teste, Real), horário, limite por rodada, **Pausar** e **Rodar agora**. Pacotes reatribuídos pela extensão saem do alerta por 2 dias. Requer a extensão 3.17.0.

## Versão 1.20.1
Reatribuição automática: a lista agora mostra **todos** os pacotes da rodada, com abas Todos, ✓ Reatribuídos, Falhas e Avisos (com a contagem de cada uma), busca, escolha da rodada (as últimas 15), "Mostrar mais" e **Baixar CSV**. Os reatribuídos aparecem primeiro.

## Versão 1.20.2
Alerta de saca fechada com pacote faltando: agora inclui **Teixeira de Freitas** (dispara quando todas as sacas dela do dia estão fechadas) e mostra na faixa os **códigos** dos pacotes que faltam (5 primeiros, com bairro; os outros em Chegada e conciliação).

## Versão 1.20.3
**Rode antes o SQL `14-quem-atribuiu.sql`.** Em Chegada e conciliação, cada cidade abre (inclusive as concluídas) e mostra **Quem atribuiu** (nome e hora), **Motorista**, quem deu a chegada e a situação. Alternador "Só os que faltam / Todos". O CSV ganhou as colunas Situação, Quem atribuiu, Motorista e Atribuído em. Pacote ainda não atribuído mostra "ainda ninguém". Sem o SQL 14 o painel funciona, mas sem o nome de quem atribuiu.

## Versão 1.20.4 — andamento da reatribuição
- Faixa no topo do bloco "Reatribuição automática": **Pedido enviado → Reatribuindo… X de N (com barra e último pacote) → Última rodada concluída**.
- Se a extensão não responder em 2 min 30 s, a faixa vira alerta vermelho com o que conferir.
- Enquanto há algo em andamento a tela se atualiza sozinha a cada 4 s; o botão fica "Em andamento…".
- Precisa da extensão 3.17.3 ou mais nova (ela passou a gravar cada pacote na hora e marcar início e fim da rodada).

## Versão 1.20.5
- Pacotes **sem motorista (no piso)** saem do Risco de extravio (Nível 1, contadores, banner e alertas). A tela só avisa quantos ficaram de fora.

## Versão 1.20.6 — Chegada por saca
- Chegada e conciliação abre em **Por saca**: cada saca é um cartão (distrito e município separados), com chegaram / atribuídos / faltam, aberta ou fechada e lista de pacotes.
- Os pacotes que faltam são ligados à saca pelo bairro (a saca que já levou pacotes daquele bairro). Sem bairro conhecido, ficam em "Sem saca definida · Cidade".
- O botão **Por cidade** mantém a visão antiga.
- Rode `15-saca-na-conciliacao.sql` antes (sem ele, cada motorista vira uma saca só).

## Versão 1.20.7
- Chegada e conciliação: coluna **Produto** nos pacotes (por saca e por cidade) e no CSV. O nome vem de quem tocou em "Mostrar produto" no app; sem consulta aparece "ainda não consultado".

## Versão 1.20.8 — voltar da ficha sem se perder
- Ao abrir um pacote (link do AWB ou "ver ficha") e voltar, a lista volta **na mesma posição**, com o pacote aberto no meio da tela.
- O último pacote aberto fica destacado (barra âmbar e "último aberto"); os já abertos ficam riscados e mais apagados. A marca vale até fechar a aba do navegador.

## Versão 1.20.9 — prévia do pacote (opção A)
- Pairar ~0,7 s sobre um AWB mostra a **prévia**: chegada (hora e quem), motorista, produto e as repetições com hora e colaborador.
- Clicar no AWB abre a **gaveta lateral** com a ficha: produto, endereço, bairro/CEP, cidade, motorista, saca, rota, cobrança e linha do tempo. Botões: Abrir ficha completa e Copiar AWB. Esc ou clicar fora fecha, e o foco volta ao link.
- Vale em todas as listas com AWB (Erros e divergências, Risco de extravio, Backlog, Chegada...). No celular/toque não há prévia: o toque abre a gaveta.
- A marcação de "último aberto" e riscado também vale para quem abriu pela gaveta.
- Sem SQL novo.

## Versão 1.20.10
- Na gaveta do pacote, o botão **Ver na saca** (no lugar de "Abrir ficha completa") leva à aba Sacas, abre a saca onde o pacote ficou (no dia dela) e destaca o pacote na lista, já rolado para ele. Sem saca registrada, o botão fica desligado.

## Versão 1.24.0
- **Reatribuição dos dois níveis**: o nível 2 (crítico, 5+ dias) agora também é reatribuído, junto com o nível 1 (4+ dias). Antes o crítico ficava de fora da reatribuição automática. Na tela de Extravio o nível 2 ganhou o botão **Reatribuição feita**, e a cobrança continua disponível.
- Precisa da extensão 3.21.0 (a reatribuição automática roda nela).

## Versão 1.23.2
- Chegada: o mesmo lugar escrito de dois jeitos ("itabata" e "Itabatã") agora vira um cartão só.

## Versão 1.23.1
- Chegada "Por saca": uma saca com pacotes de dois locais agora aparece em dois cartões (um por local), em vez de juntar tudo ('Teixeira + Nova Viçosa').

## Versão 1.23.0
- **Resumo do dia** (em Meta do dia): texto pronto com chegada, atribuídos, SLA, atrasados, pendentes por cidade, sacas ainda abertas e atribuídos por motorista, com botão **Copiar resumo** para colar no WhatsApp. Só no painel.
- Sem SQL novo.

## Versão 1.22.0
- **Chegada e conciliação por local**: cada rota (Rota 3, Rota 5 de Teixeira), distrito (Itabatã, Mucuri) e cidade aparece em cartão separado. Bairro fora das rotas aparece como "rota não definida".
- Precisa do script `17-lugar-na-conciliacao.sql` e do app 1.15.0. Pacotes que chegaram antes continuam por cidade.

## Versão 1.21.0
- **Meta do dia agora é o SLA**: baixas ÷ vencimentos do dia, meta 95% (mostra %, quantas baixas faltam, pendentes, atrasados e a evolução no dia). A meta de atribuição continua logo abaixo.
- Precisa do script `16-sla-dia.sql` e da extensão 3.18.0 (lê o iMile a cada ~10 min com uma aba do DS aberta).

## Versão 1.20.11
- Repetidos bipados na etapa de **chegada** deixam de aparecer: a lista de Repetidos (Erros e divergências), a prévia e a gaveta do pacote mostram só os de **atribuição**. (A ficha completa continua com o histórico inteiro.) Repetidos antigos sem a marca da etapa continuam aparecendo.
