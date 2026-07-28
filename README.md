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
com o convite para o sorteio → remate. O prémio sai no telemóvel porque os
brindes não têm stock limitado: quem está na carrinha é que os entrega.

A lista de brindes e os pesos de cada um estão no topo do JS da carrinha, em
`BRINDES`. Só o cupão dos 20% traz código; os outros entregam-se ali.

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
2,3 vezes menor. A app amplia cada foto até o artigo chegar a 74% da janela e
deixa as margens brancas serem cortadas, o que só funciona enquanto o número
corresponder ao ficheiro.

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

**Nada envia o email com o código de desconto.** As duas apps prometem-no e é o
único sítio onde o utilizador o pode receber, desde que o código deixou de
aparecer no ecrã. Falta uma Edge Function ligada a um serviço de envio, e falta
decidir as regras do sorteio que o email tem de explicar — quantos dias, para
quantas pessoas, em que datas.

Falta também o endereço do QR do saco, para entrar no mapa `CANAIS` e as
inscrições desse meio se distinguirem das do avião.

Nada por fazer do lado da arte: as sete fotos estão em `carrinha/img/` e medidas.

## Acessibilidade

Todo o texto cumpre o contraste AA, com uma exceção conhecida e deliberada: os
textos pequenos a branco sobre coral ficam em 3,58:1. Texto claro pequeno sobre
o coral da marca não passa desse valor — só títulos grandes é que atingem o
mínimo, e é por isso que todo o texto de leitura vive no cartão branco.

A sopa de letras é jogável sem arrastar: cada palavra da lista é um botão que a
marca na grelha. Sem isso o jogo excluía quem usa leitor de ecrã.

Ambas as apps respeitam `prefers-reduced-motion` — sem confetis, sem transições
e a máquina para no prémio sem rolar.
