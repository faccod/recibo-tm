# Recibo de Venda - Tabacaria da Montanha

App mobile (PWA) pra gerar recibos de venda profissionais com QR Code Pix direto do celular.

## Como usar

### No celular (uso normal)

1. Acesse a URL do app (ex: `https://recibo-tm.vercel.app`)
2. Toque em **"Adicionar à tela inicial"** (Safari iOS / Chrome Android) pra virar um app
3. Use normalmente — abre instantâneo, funciona offline

### Fluxo de um recibo

1. Digite o **nome do cliente**
2. Toque em **"Adicionar"** pra cada produto (descrição + qtd + valor unit)
3. (Opcional) Marque **"Salvar nos favoritos"** nos produtos que você usa sempre
4. Toque em **"Gerar Recibo"**
5. Na tela do recibo você tem 4 botões:
   - **WhatsApp** — manda texto formatado com chave Pix (ideal pra mandar a chave na conversa)
   - **Salvar** — baixa o recibo como imagem PNG (depois é só anexar no WhatsApp do cliente)
   - **Imprimir** — abre o diálogo de impressão do celular (salva como PDF também)
   - **Novo** — limpa o formulário pro próximo

### Dica de ouro

Pra produtos que você vende toda semana, adicione 1 vez como **favorito**. Da próxima vez, é só tocar no chip amarelo que o produto já entra com a qtd 1. Pra alterar a qtd, abre o produto (futuro: clique longo) ou remove e adiciona de novo.

## Estrutura

```
.
├── index.html      ← app inteiro (HTML + CSS + JS num arquivo só)
├── manifest.json   ← PWA (nome, ícone, cor)
├── icon.svg        ← ícone do app (letras TM)
├── sw.js           ← service worker (cache offline)
└── README.md       ← este arquivo
```

Não tem build, não tem dependência local. Tudo via CDN (Tailwind, Inter, html2canvas, qrcode).

## Como editar os dados da empresa

Abre o `index.html` e procura o bloco `const EMPRESA = { ... }` no `<script>`. Os campos são:

```js
const EMPRESA = {
  nome: 'TABACARIA DA MONTANHA',
  endereco: 'AV FREDERICO GRULKE 1367 - CENTRO',
  cidade: 'SANTA MARIA DE JETIBÁ/ES',
  cep: 'CEP 29645-000',
  cnpj: '97.519.383/0001-40',
  telefone: '(27) 9929-1314',
  pixChave: '97519383000140',  // chave Pix em si (CNPJ sem pontuação)
  pixTipo: 'CNPJ',             // tipo exibido no recibo
  merchantName: 'TABACARIA DA MONTANHA',  // vai no BR Code Pix (sem acento)
  merchantCity: 'SANTA MARIA DE JETIBA',  // vai no BR Code Pix (sem acento)
};
```

⚠️ Os campos `merchantName` e `merchantCity` **não podem ter acento** — o padrão Pix rejeita caracteres especiais no BR Code. Por isso as versões sem acento (JETIBÁ → JETIBA).

## Como hospedar (Vercel, grátis)

O jeito mais fácil:

1. Suba essa pasta pro GitHub (crie um repo `recibo-tm`, faça push)
2. Vá em [vercel.com](https://vercel.com) → **Add New Project** → importe o repo
3. Deploy. Em ~1min você recebe uma URL tipo `recibo-tm.vercel.app`
4. (Opcional) Em **Settings → Domains** adicione um domínio próprio

A Vercel serve o `index.html` direto, sem build. Atualizações no GitHub propagam em segundos.

## Backup

Os dados salvos no celular (favoritos, contador de recibos, rascunho) ficam no `localStorage`. Se você limpar os dados do navegador, perde. Pra não correr risco, **favoritos são só atalho** — não dependa deles como cadastro de produtos. Use o app como ferramenta, mantenha o cadastro de verdade onde você já mantém.

## Próximas evoluções (ideias)

- [ ] Catálogo de produtos completo (não só favoritos)
- [ ] Histórico de recibos no próprio app
- [ ] Editar qtd direto no chip (sem precisar abrir modal)
- [ ] Múltiplas empresas (cadastrar SMJ e ST)
- [ ] Assinatura digital do cliente
- [ ] Sincronização entre celular e PC

## Stack

- HTML + Tailwind (CDN)
- JS vanilla (sem framework)
- html2canvas (gerar PNG do recibo)
- qrcode (gerar QR Code do Pix)
- BR Code Pix (EMV) implementado à mão (~30 linhas)
