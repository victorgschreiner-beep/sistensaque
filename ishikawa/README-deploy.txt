ISHIKAWA AI — build estático, pronto para viver DENTRO do repositório do
dashboard Raguife, sem sobrescrever nada que já existe lá.

Como instalar:
1. Copie esta pasta inteira "ishikawa/" (com o arquivo oculto .nojekyll, a
   pasta assets/ E o arquivo firebase-config.json) para a RAIZ do
   repositório do dashboard Raguife — ao lado do index.html que já existe
   lá. Não renomeie nem mova o index.html do Raguife: ele continua
   exatamente onde está.

   Estrutura final esperada no repositório:
     seu-repo/
       index.html              <- dashboard Raguife (já existe, intocado)
       ishikawa/                <- pasta nova que você está adicionando
         index.html
         firebase-config.json   <- já vem preenchido, ver abaixo
         .nojekyll
         assets/

2. Suba para o GitHub:
     git add ishikawa
     git commit -m "Adiciona Ishikawa AI"
     git push

3. Se o repositório já usa GitHub Pages, o Ishikawa AI fica acessível em:
     https://SEU-USUARIO.github.io/NOME-DO-REPO/ishikawa/
   O dashboard Raguife continua em:
     https://SEU-USUARIO.github.io/NOME-DO-REPO/

Por que isso funciona sem ajustes extras:
- Os caminhos dos arquivos (CSS/JS) dentro deste index.html são relativos
  (./assets/...), então funcionam tanto na raiz quanto dentro de /ishikawa/.
- A navegação interna entre as telas usa "hash routing" (#/sessao/...),
  que não depende de configuração nenhuma do servidor — evita qualquer
  conflito com as rotas ou scripts do dashboard Raguife.

Para editar o sistema no futuro, use o projeto-fonte completo (não estes
arquivos prontos) e rode "npm install && npm run build" para gerar uma nova
versão desta pasta.

===============================================================================
SOBRE O firebase-config.json — JÁ VEM PRONTO NESTE PACOTE
===============================================================================
Esta versão NÃO tem mais nenhuma sessão/ideia/participante fictício — o app
abre vazio, pronto para uso real.

Como você pediu, este pacote já vem configurado para usar O MESMO projeto
Firebase que o dashboard Raguife já usa (projeto "sistema-ensaque" —
extraído do firebaseConfig que já está dentro do seu index.html atual).
Ou seja: SE VOCÊ SEGUIR O PASSO 1 ACIMA (copiar a pasta inteira, incluindo
o firebase-config.json), a colaboração em tempo real pelo QR Code já
funciona direto, sem precisar passar pela tela de Configurações.

Verificações que eu já fiz antes de reaproveitar esse projeto:
- Conferi todos os caminhos que o dashboard Raguife já usa no banco (ops,
  apontamentos, produtos, ensaque_*, silos_registros, metas_diarias, etc.)
  — o Ishikawa AI grava tudo debaixo de "sessions/" e "sessions_index/",
  caminhos que o Raguife NUNCA usa. Não há risco de um sistema sobrescrever
  dado do outro.
- O dashboard Raguife não usa login/autenticação do Firebase (grava e lê
  direto), então as Regras do Realtime Database desse projeto já são
  abertas o suficiente para o Ishikawa AI funcionar do mesmo jeito — não
  precisa mexer em Regras para isso funcionar agora.

Se um dia você quiser usar um projeto Firebase DIFERENTE para o Ishikawa AI
(separado do Raguife), acesse dentro do app: menu lateral → "Conexão em
Tempo Real" (.../ishikawa/#/configuracoes-conexao) → preencha as novas
credenciais → "Testar conexão" → "① Baixar firebase-config.json" → troque
o arquivo desta pasta pelo novo → suba de novo para o GitHub.

Ponto de atenção (vale conferir com calma antes de operar com dados
sensíveis): como não há autenticação, qualquer pessoa com a URL do banco
consegue ler/escrever nele — isso já é assim para o Raguife hoje, então
reaproveitar o projeto não piora nem melhora esse ponto. Se algum dia isso
virar preocupação, a correção é no Firebase (Regras + Auth), não no código
do Ishikawa AI.

===============================================================================
DEPOIS DE SUBIR — CHECKLIST DE TESTE (faça sempre, é rápido)
===============================================================================
1. Espere 1–2 minutos após o push (GitHub Pages leva um tempinho pra
   publicar).
2. No CELULAR, se você já tinha acessado essa URL antes de uma versão
   anterior, force um recarregamento sem cache (no Safari/Chrome do
   celular: segure o botão de recarregar, ou limpe os dados do site) —
   navegadores mobile guardam cache agressivo, e testar com a versão
   antiga em cache reproduz bugs que já foram corrigidos.
3. No computador: crie uma sessão nova → tela de QR Code.
4. No celular (de preferência em rede de dados móveis, não no mesmo Wi-Fi,
   pra garantir que não é coincidência de rede local): escaneie o QR Code,
   identifique-se, envie uma ideia.
5. No computador: abra a Parede de Post-its da mesma sessão — a ideia deve
   aparecer em poucos segundos, sem precisar atualizar a página.

Se aparecer "Não foi possível conectar a esta sessão" no celular mesmo
depois disso, quase sempre é um destes dois motivos:
- O celular carregou uma versão em cache de ANTES do firebase-config.json
  existir no pacote (resolve com o passo 2 acima).
- O firebase-config.json não foi de fato publicado junto com o index.html
  desta pasta (confira acessando .../ishikawa/firebase-config.json
  diretamente no navegador — deve mostrar o JSON, não um erro 404).

===============================================================================
NOVO: INSIGHTS DE PARADAS (lê o dashboard Raguife, só leitura, 2 fontes)
===============================================================================
Nova tela: menu lateral → "Insights de Paradas" (ou pelo card na tela inicial).
Tem duas abas — troque entre elas sem sair da tela:

① EXTRUSÃO (aba padrão) — lê "extrusao_paradas" (cada parada registrada:
  linha, motivo, horário de início/fim, minutos parado, operador) cruzado
  em tempo real com o catálogo "extrusao_motivos_parada" (que mapeia cada
  motivo à sua categoria: Manutenção, Operacional, Fator Externo, Setup ou
  Utilidades — o mesmo catálogo que aparece na tela de Extrusão do
  dashboard, editável por lá). Quebra também por linha de produção.

② ENSAQUE (Balanças) — lê "ensaque_historico_paradas", como na versão
  anterior deste pacote — paradas de balança, quebradas por balança.

Ambas as abas mostram, dentro do Ishikawa AI:
- tempo total parado no mês, nº de paradas, paradas ainda em aberto;
- ranking das maiores causas de parada (por tempo parado, não só frequência);
- gráfico por categoria e por linha/balança.

FILTRO POR CATEGORIA: logo abaixo das abas aparecem "chips" clicáveis com
cada categoria encontrada naquele mês (ex: Manutenção, Operacional, Fator
Externo — mais Setup/Utilidades na Extrusão, quando existirem), cada um já
mostrando o tempo total daquela categoria. Ao clicar em uma categoria, os
cards, o ranking (que passa a mostrar as 5 maiores em vez de 8) e os
gráficos filtram só para ela — é o "me mostre as 5 maiores paradas de
Manutenção deste mês" direto na tela. Clique em "Todas" para voltar à visão
geral. Trocar de aba (Extrusão ↔ Ensaque) limpa o filtro automaticamente,
porque cada fonte tem seu próprio conjunto de categorias.

DRILL-DOWN POR LINHA: clique numa barra do gráfico "Tempo parado por
linha/balança" para abrir, logo abaixo, um painel só daquela linha —
quantidade de paradas, tempo total, ranking dos motivos daquela linha
específica, e a lista de cada ocorrência (data, horário, motivo). Na aba
Extrusão, cada ocorrência também mostra qual PRODUTO estava rodando na
linha naquele turno — informação cruzada em tempo real com a OP ativa
registrada pelo próprio dashboard (nó "extrusao_op_ativa" + "ops"), sem
nenhum dado hardcoded. Quando não há OP registrada para aquele turno (ex:
troca de turno, parada de setup antes de iniciar produção), aparece
"não identificado" em vez de adivinhar um produto errado. Clicar de novo na
mesma barra (ou no X do painel) fecha o drill-down. A aba Ensaque mostra o
mesmo painel (quantidade + motivos + ocorrências), mas sem a coluna de
produto — esse módulo ainda não registra qual OP estava ativa por parada.

Cada item do ranking tem um botão "Investigar" que já cria uma nova sessão
de brainstorm com o problema, setor e objetivo pré-preenchidos a partir
daquela parada — direto do dado real para a análise de causa-raiz.

Ponto de atenção sobre paradas em aberto (aba Extrusão): o próprio sistema
Raguife só calcula os minutos parados quando o operador registra o horário
de término. Uma parada ainda em aberto (sem esse horário) aparece com 0min
até ser fechada — o Ishikawa AI só exibe o que já está lá, não estima o
tempo corrido. Um aviso amarelo aparece na tela quando há paradas em aberto
no mês, para você não confundir "0min" com "sem impacto".

IMPORTANTE — leitura, nunca escrita: as três funções que alimentam esta
tela (subscribeExternalParadas, subscribeExternalExtrusaoParadas,
subscribeExternalExtrusaoMotivos, em src/services/RealtimeService.js) só
têm LEITURA para esses caminhos. Não existe, em nenhum lugar do Ishikawa
AI, código que escreva, atualize ou apague dado em "ensaque_historico_paradas",
"extrusao_paradas", "extrusao_motivos_parada" ou qualquer outro caminho do
dashboard Raguife.

Confirmação: o link https://victorgschreiner-beep.github.io/sistensaque/extrusao.html
que você indicou É o mesmo projeto Firebase "sistema-ensaque" já configurado
neste pacote — confirmei lendo o firebaseConfig de dentro do arquivo que
você enviou (mesma apiKey/databaseURL). Também percebi, ao investigar esse
repositório, que ele já contém uma pasta "ishikawa/" com o pacote que te
entreguei antes — ou seja, o deploy anterior já está publicado nesse mesmo
repositório GitHub.
