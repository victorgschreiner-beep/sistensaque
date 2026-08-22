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
