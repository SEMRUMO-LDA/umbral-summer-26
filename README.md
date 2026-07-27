# Umbral · Verão 2026

Microsite da campanha de verão da [Umbral](https://byumbral.com), para o QR code
impresso no saco das bolas de berlim. Feito para telemóvel — é assim que vai ser
usado, e só assim.

O percurso tem quatro passos, cada um com um propósito:

| Passo | O que faz |
|---|---|
| Jogo | Sopa de letras com seis palavras que dão a conhecer a marca |
| Email | Desbloqueia 20% de desconto e a hipótese da semana de férias |
| Foto | Garante a participação no sorteio |
| Remate | Aviso de proteção solar e ligação para o site |

## Correr localmente

Não há build nem dependências: `index.html` é um ficheiro único e
autónomo, com o logótipo e o mosaico da marca embutidos em SVG.

Abrir o ficheiro diretamente no browser funciona. Para testar no telemóvel,
servir a pasta e abrir o IP da máquina a partir de um telefone na mesma rede:

```bash
npx --yes serve -l 8080 .
```

## Base de dados

Supabase, projeto `vidhfmbsmmadgsudtpco`.

- `inscricoes` — nome, email, código e tempo de jogo
- `participacoes` — instagram e caminho da foto, uma por inscrição
- `fotos-campanha` — bucket privado, 5 MB, só JPEG/PNG/WebP
- `campanha_umbral_2026` — vista com o join feito, para exportar em CSV

A chave publicável está no código-fonte de propósito: é para isso que serve. O
papel `anon` tem exatamente três permissões — INSERT nas duas tabelas e upload
no bucket. Não lê, não altera, não apaga. As inscrições veem-se no dashboard do
Supabase, nunca na página.

Um jogo por email, garantido por índice único sobre `lower(email)`.

## Por fazer

**Nada envia o email com o código de desconto.** A app promete-o duas vezes e é
o único sítio onde o utilizador o pode receber, desde que o código deixou de
aparecer no ecrã. Falta uma Edge Function ligada a um serviço de envio.

## Acessibilidade

Todo o texto cumpre o contraste AA, com uma exceção conhecida: o crédito do
rodapé, a branco sobre coral, fica em 3,58:1. Texto claro pequeno sobre o coral
da marca não passa desse valor — só títulos grandes é que atingem o mínimo.
