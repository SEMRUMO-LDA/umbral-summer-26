# Umbral · Verão 2026

Campanha de verão da [Umbral](https://byumbral.com). Feita para telemóvel — é
assim que vai ser usada, e só assim.

São duas apps, uma por tipo de meio, porque quem chega a cada uma chega em
circunstâncias diferentes:

| App | Meio | Quem chega |
|---|---|---|
| [`index.html`](index.html) | Saco das bolas de berlim, avião com faixa | Não conhece a marca: há um jogo a apresentá-la |
| [`carrinha/index.html`](carrinha/index.html) | Carrinha com o quarto ambulante | Já está à frente da marca: não há jogo, há máquina de prémios |

Ambas acabam no mesmo sítio — uma foto do verão, o sorteio de uma semana de
alojamento e o remate com o aviso de proteção solar.

## Percursos

**Saco e avião** — quatro passos: sopa de letras com seis palavras que dão a
conhecer a marca → email (destrava os 20% e o sorteio) → foto → remate.

**Carrinha** — três passos: a máquina (o email é o que a faz rolar) → prémio,
com o convite para o sorteio → remate. O nome é pedido nas duas apps e é
opcional nas duas: quem o deixar é tratado por ele nos emails. O prémio sai no telemóvel porque os
brindes não têm stock limitado: quem está na carrinha é que os entrega.

A lista de brindes está no topo do JS da carrinha, em `BRINDES`. Só o cupão dos
20% traz código; os outros entregam-se ali.

### Os pesos saem do stock

| Prémio | Stock | Hipótese |
|---|---:|---:|
| Caneta | 6000 | 32,0% |
| 20% na estadia | — | 28,0% |
| Chapéu de palha | 500 | 12,9% |
| Almofada de praia | 500 | 12,9% |
| Saco de pano | 400 | 10,3% |
| Para-sol para o carro | 100 | 2,6% |
| Guarda-sol de praia | 48 | 1,2% |

A caneta e o cupão são inesgotáveis para o tamanho desta campanha, por isso os
pesos deles são escolhidos: sobram 40% da máquina para os cinco artigos
escassos. Esses cinco levam **o próprio stock como peso**, e é isso que faz a
conta fechar — a hipótese de cada um passa a ser a sua parte do stock, portanto
**todos acabam à mesma jogada, a 3870**, em vez de um morrer no primeiro dia e
deixar a máquina a prometer o que já não há.

Proporção pura entre os sete dava 79% de caneta e 0,64% de guarda-sol: sete
prémios na fita para dar caneta quase sempre.

**Mexer numa quantidade é mexer no peso.** Para o stock durar mais jogadas,
baixa-se a fatia dos cinco: menos peso neles, mais no cupão e na caneta.

Não há contador nem interruptor. Um contador no telemóvel sabe quantas vezes a
app prometeu, não quantos artigos restam na carrinha — alguém ganha e vai-se
embora sem levantar, alguém recebe um à mão sem passar pela app, e ao fim de dois
dias o contador e a caixa não batem certo. Quem sabe o que resta é quem lá está.

**As duas apps abrem com o mesmo grito.** "Olhaaá boooorla da Umbral!" é a frase
da campanha e é linguagem de rua, de quem apregoa — encaixa numa carrinha parada
na estrada tão bem como num saco. O que muda entre as duas é a instrução por
baixo. O trocadilho do mecanismo passou para o botão, que é onde uma instrução
pertence: **Deixa rolar**.

Isto também alinhou o que faltava: sendo o mesmo texto, o título tem a mesma
altura nas duas, e o cartão branco passa a arrancar do mesmo sítio.

**O topo das duas apps alinha.** O jogo devolve ao cabeçalho a folga que a
grelha lhe deixa, através da variável `--ar`; a carrinha faz o mesmo com a folga
que sobra depois de o botão caber. Sem isso, a mesma marca abria de duas
maneiras conforme o meio por onde a pessoa chegasse — os pontos e o título
ficavam 34px mais acima na carrinha.

Os números — dividir a folga por 4, com tecto de 20px — saíram de medir o `--ar`
do jogo em seis telemóveis reais, onde vale 0, 3, 14, 15, 18 e 24. Ele cresce
devagar e a folga da carrinha depressa (172px num Pro Max), por isso não bastava
o divisor. Assim a diferença entre as duas apps não passa dos 8px no título em
nenhum tamanho.

**A fita passeia antes de alguém carregar** — uma cópia da lista a cada 11
segundos. É assim que os prémios se mostram sem pedir a ninguém que leia nada, e
resolve outra coisa: parada, a máquina ficava no `-20%`, o que se pode ler como
"isto já é meu". Ao carregar, a app congela a fita onde ela vai e acelera dali
até ao prémio — não parte do zero, senão dava um salto para trás.

O passeio pára enquanto se escreve o email, e não existe para quem pediu menos
movimento: conteúdo que se move sozinho e nunca pára contraria o WCAG 2.2.2.

**Um rolo, não três.** Numa slot machine a três rolos perde-se quando os
símbolos não combinam, e aqui não há como perder — a app promete que sai sempre
borla. Um rolo diz a verdade da mecânica: uma fita de prémios que para num
deles. A fita tem oito cópias da lista empilhadas; a máquina para na sétima e as
que sobram são a folga de que o recuo final precisa.

**A arte está em `carrinha/img/`** — sete quadrados de 451px com fundo branco, e
é por isso que a janela da máquina é branca: assim não se vê a moldura de cada
JPEG.

**Trocar uma foto obriga a medir outra vez.** Cada brinde traz um `ocupa`: a
altura do artigo dentro da sua fotografia, em fração da altura total. Serve para
todos aparecerem do mesmo tamanho na janela — a almofada ocupa 37% do seu
quadrado e o saco 87%, e desenhados os dois à altura da janela a almofada saía
2,3 vezes menor.

**A normalização é por massa, não por altura.** Cada brinde traz duas medidas —
`ocupa` (altura) e `larga` (largura) — e a app iguala a **média geométrica** das
duas, que é um proxy honesto para o peso visual.

Normalizar só pela altura, que foi a primeira tentativa, dava a todos a mesma
altura e larguras muito diferentes: a almofada, baixa e larga, esticava até
ficar três vezes mais larga que o saco, alto e estreito. Aos olhos a massa ia de
0,67 a 1,19 — quase o dobro entre o menor e o maior prémio. Pela média
geométrica ficam todos em 0,76 e cada um mantém a sua forma.

O tecto é o saco, o mais alto de todos: a 0,76 ocupa 93% da altura da janela. O
construtor recusa-se a gerar o ficheiro se alguma foto passar disso ou se o
corte da margem branca chegar aos 26%.

Para medir: contar os pixéis que não são brancos e dividir a altura da caixa que
os contém pela altura da imagem.

## Correr localmente

Não há build nem dependências. Cada app é um ficheiro único e autónomo, com o
logótipo, o mosaico e os selos embutidos em SVG — um pedido HTTP e está lá tudo,
o que conta para quem abre isto na rua com uma barra de rede.

```bash
npx --yes serve -l 8080 .
```

Acrescentar `?dev` ao endereço abre uma barra para saltar entre ecrãs sem
percorrer o fluxo. `?o=teste` grava as inscrições noutro canal, para os testes
não sujarem os números da campanha.

### O que as duas apps têm em comum

A camada de marca está duplicada de propósito, para cada app continuar a ser um
ficheiro só. Mexer numa destas coisas é mexer nas duas:

- os quatro SVG: mosaico, logótipo, selo do Pantone Escaldão, selo da Ao Rubro
- as variáveis de cor e tipografia, e as regras de `.app`, `.topo`, `.cabeca`,
  `.folha`, `.passos`, `.btn`, formulários, `.rodape` e `.remate-final`
- a troca de ecrãs (`mostrar`, `animarAltura`), os confetis, o swipe
- a camada do Supabase (`pedir`, `inserir`, `inserirTeimoso`, os três casos de
  409) e todo o ecrã da foto

`carrinha/index.html` foi gerado a partir do `index.html` para os SVG não serem
copiados à mão. É um ficheiro normal a partir daí — edita-se diretamente.

## Base de dados

Supabase, projeto `vidhfmbsmmadgsudtpco`.

- `inscricoes` — email, código, brinde, tempo de jogo e canal de origem
- `participacoes` — instagram e caminho da foto, uma por inscrição
- `fotos-campanha` — bucket de leitura pública, 5 MB, só JPEG/PNG/WebP
- `campanha_umbral_2026` — vista com o join feito, para exportar em CSV
- `limpar-fotos-orfas` — Edge Function que apaga fotos sem registo, com uma
  margem de 6 horas. **Falta agendá-la** (Integrations → Cron, diária).

A chave publicável está no código-fonte de propósito: é para isso que serve. O
papel `anon` tem exatamente três permissões — INSERT nas duas tabelas e upload
no bucket. Não lê, não altera, não apaga. As inscrições veem-se no dashboard do
Supabase, nunca na página.

### As duas regras de unicidade

Vivem em índices da base de dados, não no cliente, e são diferentes de propósito:

- `inscricoes_email_por_canal` sobre `(lower(email), origem)` — uma inscrição
  por canal. Quem veio pelo saco pode jogar a máquina na carrinha: são momentos
  diferentes e o código dos 20% é igual para todos, portanto repetir é inócuo.
- `participacoes_email_unico` sobre `lower(email)` — **uma** participação no
  sorteio por pessoa, venha de onde vier. É esta a borla a sério.

O email das `participacoes` é copiado da inscrição por um trigger
`SECURITY DEFINER`, para o cliente não o poder forjar e contornar a regra.

## Por fazer

Falta ligar os gatilhos do envio — o webhook e a tarefa periódica. Ver a secção
dos emails.

Falta também o endereço do QR do saco, para entrar no mapa `CANAIS` e as
inscrições desse meio se distinguirem das do avião.

Nada por fazer do lado da arte: as sete fotos estão em `carrinha/img/` e medidas.

## Emails

Três, escolhidos pelo **prémio** e não pelo meio — na carrinha o cupão dos 20%
também sai da máquina, e quem o tira precisa do email do código, não do brinde:

| Email | Vai para | Dispara em |
|---|---|---|
| **A · o teu código** | Quem tem `codigo`: saco, avião, e o cupão da carrinha | `inscricoes` |
| **B · o teu brinde** | Quem tirou um artigo na carrinha | `inscricoes` |
| **C · estás no sorteio** | Só quem enviou foto | `participacoes` |

O C não precisa de filtro: a linha em `participacoes` só existe se a foto foi
mesmo enviada, portanto quem escolheu "quero ir dar um mergulho" nunca lá está.

### Como funciona

`enviar-emails` é uma Edge Function em **drenagem**: procura o que ainda não foi
enviado, envia pelo Resend, e marca `email_enviado_em`. Não recebe o registo de
que se trata. Por isso o mesmo código serve dois gatilhos — um Database Webhook
no INSERT, que dá o envio quase imediato, e uma tarefa periódica que apanha o
que o webhook falhou — e correrem os dois ao mesmo tempo não duplica nada: quem
manda é a marca, não quem chamou.

Falhas 4xx são a morada que não presta: marcam-se como tratadas com o erro à
vista em `email_erro`, porque repetir dava sempre o mesmo. Falhas 5xx e de rede
ficam por enviar e passam à próxima ronda.

`?seco=1` conta o que enviaria sem enviar. Só aceita a chave de serviço: sem ela
responde 401, incluindo à chave publicável que está no código das apps.

### O remetente é um subdomínio próprio

`campanha.byumbral.com`, e não o `byumbral.com`. Duas razões, e ambas contam:

O domínio principal já está em uso por **outra equipa Resend**, noutra parte da
operação. Verificá-lo aqui teria transferido a posse e revogado o acesso deles —
o próprio Resend avisa disso. O subdomínio não lhes toca.

E isola a reputação. Uma campanha de rua para milhares de pessoas gera sempre
algumas marcações de spam; num subdomínio isso não arrasta as confirmações de
reserva que saem do domínio principal.

Ganha-se ainda margem no DMARC: a raiz tem `p=reject` mas `sp=none`, portanto um
email do subdomínio nunca é recusado em silêncio.

**Verificado e testado.** Um envio de prova deu `DKIM=PASS` e `DMARC=PASS`. O SPF
aparece como softfail quando o destinatário reencaminha o email — é o
reencaminhamento a quebrar o SPF, não a montagem, e é por isso que o DKIM é o que
segura tudo aqui.

Os emails caem no separador **Promoções** do Gmail. Não é spam, é a categoria
certa para o que eles são — mas é a razão pela qual o ecrã, e não o email, é o
que vale para levantar o brinde na carrinha.

### O que falta para enviar

1. Database Webhook em INSERT nas duas tabelas, e uma tarefa periódica de minuto
   a minuto como rede de segurança, ambos a chamar `enviar-emails`. Sem eles a
   função só corre à mão.
2. Plano do Resend: o gratuito dá **100 emails por dia**. Uma semana de rua com
   três meios em simultâneo passa disso à vontade.

`?prova=<email>` manda os três para uma morada só, com dados de mentira e sem
tocar na base de dados.

## Acessibilidade

Todo o texto cumpre o contraste AA, com uma exceção conhecida e deliberada: os
textos pequenos a branco sobre coral ficam em 3,58:1. Texto claro pequeno sobre
o coral da marca não passa desse valor — só títulos grandes é que atingem o
mínimo, e é por isso que todo o texto de leitura vive no cartão branco.

A sopa de letras é jogável sem arrastar: cada palavra da lista é um botão que a
marca na grelha. Sem isso o jogo excluía quem usa leitor de ecrã.

Ambas as apps respeitam `prefers-reduced-motion` — sem confetis, sem transições
e a máquina para no prémio sem rolar.
