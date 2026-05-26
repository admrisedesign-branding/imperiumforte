# Imperium Forte — Landing Page

Landing page institucional e comercial da **Imperium Forte**, fabricante de cimento pronto sediada no Distrito Industrial 1 de Manaus.

Site desenvolvido pela [Rise Agência](https://rise.ag) — site único, sem dependências externas, otimizado para conversão de leads B2C (consumidores finais e pedreiros) e B2B (revendas e construtoras).

---

## Tecnologias

Site 100% estático construído com **HTML + CSS + JavaScript puro** — zero dependências de build. Pode ser servido por qualquer servidor estático (Vercel, Netlify, GitHub Pages, S3, Cloudflare Pages, nginx, etc.).

- HTML5 semântico
- CSS moderno (CSS Grid, Custom Properties, Backdrop-filter)
- JavaScript vanilla (ES2020+)
- Fontes: Inter + Instrument Serif (Google Fonts)
- Performance: imagens otimizadas em JPG/PNG, lazy rendering, sem frameworks pesados

---

## Estrutura do projeto

```
imperium-forte-site/
├── index.html              # Página principal (tudo em um arquivo)
├── vercel.json             # Configurações de cache + headers de segurança
├── .gitignore
├── README.md
└── images/
    ├── hero/               # Carrossel automático do hero (3 slides)
    │   ├── hero-1.jpg      # Soldados romanos construindo (1º slide)
    │   ├── hero-2.jpg      # Homem olhando a casa esboço→construída
    │   └── hero-3.jpg      # Família relaxando no jardim da casa pronta
    ├── products/           # Fotos dos 4 sacos de cimento
    │   ├── saco-amarelo.jpg    # Concreto Eco
    │   ├── saco-vermelho.jpg   # Concreto Pro (também usado p/ Imperium Mix com filtro azul)
    │   └── saco-verde.jpg      # Reboco Pro
    ├── logo/
    │   └── imperium-forte.png  # Logo oficial circular
    └── section/
        └── pedreiro.jpg    # Background da seção "Confiança Técnica"
```

---

## Como rodar localmente

Como o site é estático puro, basta abrir o `index.html` no navegador. Mas pra testar com paths absolutos funcionando corretamente, recomendo um servidor local:

### Opção 1 — Python (já vem instalado no Mac/Linux)

```bash
cd imperium-forte-site
python3 -m http.server 8000
```

Acesse: <http://localhost:8000>

### Opção 2 — Node.js (com http-server)

```bash
npx http-server -p 8000
```

### Opção 3 — VS Code

Instale a extensão **Live Server** e clique com botão direito no `index.html` → **Open with Live Server**.

---

## Deploy na Vercel

### Opção A — Via GitHub (recomendado)

1. Suba este projeto pro GitHub (público ou privado)
2. Acesse <https://vercel.com> e faça login
3. Clique em **Add New → Project**
4. Selecione o repositório `imperium-forte-site`
5. Configurações padrão (Vercel detecta site estático automaticamente)
6. Clique em **Deploy**

Pronto. Em ~30 segundos o site estará no ar em uma URL como `imperium-forte.vercel.app`.

### Opção B — Via Vercel CLI

```bash
npm i -g vercel
cd imperium-forte-site
vercel
```

### Opção C — Drag and drop

Acesse <https://vercel.com/new> e arraste a pasta inteira do projeto pro navegador.

### Domínio customizado

Depois do deploy, conecte um domínio próprio (ex: `imperiumforte.com.br`) em:
**Project Settings → Domains → Add Domain**

A Vercel guia o DNS automaticamente. Geralmente o setup leva minutos.

---

## ⚠️ Configurações necessárias antes do go-live

Antes de publicar o site em produção, **edite duas constantes no topo do `<script>` do `index.html`**:

```javascript
// Localize estas linhas no <script> do index.html (próximo da linha 3000)
const WEBHOOK_URL = 'https://webhook.site/SUBSTITUIR-POR-URL-DO-CRM';
const WHATSAPP_NUMBER = '559200000000'; // DDI + DDD + número
```

### WEBHOOK_URL

URL do webhook do CRM que vai receber o payload dos leads quando o formulário for enviado. Opções recomendadas:

- **Zapier** — crie um Zap "Webhook by Zapier (Trigger: Catch Hook)" e use a URL gerada
- **Make (ex-Integromat)** — Webhook → URL gerada
- **n8n** — Webhook node → URL gerada
- **CRM próprio** — endpoint da API que recebe POST JSON

O payload enviado tem este formato:

```json
{
  "nome": "João Silva",
  "whatsapp": "(92) 9 9999-9999",
  "whatsapp_clean": "92999999999",
  "email": "joao@email.com",
  "cidade": "Manaus, AM",
  "perfil": "b2b_construtora",
  "lead_tag": "B2B_QUENTE",
  "lead_score": 95,
  "lead_temperature": "quente",
  "produtos_interesse": ["concreto_pro", "imperium_mix"],
  "produtos_interesse_nomes": ["Concreto Pro", "Imperium Mix"],
  "calculadora": {
    "tipo": "estrutural",
    "area": 120,
    "espessura": 10,
    "sacos": 152,
    "total_estimado": 5913.20
  },
  "atribuir_para": "vendedor_b2b",
  "prioridade": "alta",
  "utm_source": "google",
  "utm_medium": "cpc",
  "utm_campaign": "...",
  "referrer": "...",
  "submitted_at": "2026-05-26T18:30:00.000Z",
  "time_on_page_seconds": 240
}
```

### WHATSAPP_NUMBER

Número do WhatsApp comercial da Imperium Forte no formato internacional **sem espaços, parênteses ou hifens**.

Exemplo: para `(92) 99999-9999` use `'5592999999999'`.

### Notificação por e-mail para a empresa

O envio do e-mail acontece **no CRM/Zapier após receber o webhook**, não pelo frontend. Configuração recomendada no Zapier:

1. **Trigger**: Webhook by Zapier (Catch Hook) — URL configurada em `WEBHOOK_URL`
2. **Action 1**: Email by Zapier → enviar para `vendas@imperiumforte.com.br` com os campos do lead
3. **Action 2** (opcional): Google Sheets → adicionar linha na planilha "Leads"
4. **Action 3** (opcional): Slack → notificar canal #vendas em leads quente (score ≥ 80)

---

## Recursos da landing

### Hero
- Carrossel automático de 3 slides (troca a cada 4 segundos, fade de 1.2s)
- 2 CTAs principais (B2C e B2B)
- Selos de certificação sutis (ABNT, ISO 14001)

### Stats bar
- 4 métricas-chave: obras atendidas, cidades, % feito no Amazonas, % aprovação

### Produtos
- 4 cards com foto real do saco
- Modal de ficha técnica completa ao clicar (composição, desempenho, indicações, modo de uso, armazenamento)
- Botão "Fazer orçamento" leva pro form com produto pré-selecionado

### Pilares (Por que Imperium)
- 3 diferenciais com numeração e citações editoriais

### Depoimentos
- 3 depoimentos reais (Carlos Souza, Fernanda Lima, Roberto Mendes)
- Fundo vermelho para destaque emocional

### Comparativo
- Velho cimento 40kg vs Imperium 20kg pronto

### Calculadora de consumo
- Funcional: calcula sacos e total em R$ baseado em área, espessura e tipo de aplicação
- Botão WhatsApp pré-preenchido com o cálculo

### Fábrica
- Background fotográfico do pedreiro aplicando reboco
- 4 cards de números da operação
- Link Google Maps "Conheça a fábrica"

### Formulário inteligente
- 4 etapas: Perfil → Produtos (checkbox múltiplo) → Detalhes condicionais → Contato
- Lead scoring 0-100 com tags automáticas (B2C_QUENTE/MORNO/FRIO, B2B_QUENTE/MORNO)
- Roteamento sugerido pro CRM (vendedor B2B ou B2C, prioridade alta/média/baixa)
- Captura UTMs, referrer, tempo na página
- Fallback localStorage caso o webhook falhe (não perde lead)
- Máscara automática no WhatsApp `(DD) D DDDD-DDDD`

### FAQ
- 6 perguntas mais comuns, respostas sempre visíveis

### Floating WhatsApp
- Botão fixo no canto inferior direito

### Modal de ficha técnica
- Header com foto + nome + descrição + peso + base
- Body com composição, desempenho, aplicações (indicado/não indicado), modo de uso, armazenamento
- Footer com 2 ações: Baixar PDF (placeholder) + Fazer orçamento

---

## Tracking opcional (Google Analytics e Meta Pixel)

No `<script>` do `index.html`, há comentários indicando onde adicionar tags de tracking:

```javascript
// Tracking opcional (Google Analytics, Meta Pixel) — descomente quando configurar
// if (typeof gtag !== 'undefined') gtag('event', 'lead_submitted', { lead_score: payload.lead_score, perfil: payload.perfil });
// if (typeof fbq !== 'undefined') fbq('track', 'Lead', { value: payload.lead_score, currency: 'BRL' });
```

Para ativar:

1. Adicione os scripts do GA4 e Meta Pixel no `<head>` antes do `</head>`
2. Descomente as linhas acima no `submitForm()`
3. Eventos importantes para trackear:
   - `lead_submitted` — quando o form é enviado
   - `produto_modal_aberto` — quando alguém clica em "Ver ficha técnica"
   - `calculadora_usada` — quando alguém clica "Orçamento no WhatsApp" da calc
   - `whatsapp_clicked` — quando o botão flutuante é clicado

---

## Próximos passos sugeridos

- [ ] Substituir a foto placeholder do **Imperium Mix** (atualmente usa o saco vermelho com filtro azul) por foto real do produto quando disponível
- [ ] Confirmar com a Imperium os números reais da seção stats (500+ obras, 15+ cidades, 99% aprovação) — devem ser comprovados se publicados
- [ ] Gerar os 4 PDFs estáticos das fichas técnicas (`/fichas/concreto_pro.pdf`, etc.) e ativar download real
- [ ] Configurar webhook do CRM e testar fluxo completo de lead
- [ ] Conectar domínio próprio (ex: `imperiumforte.com.br`)
- [ ] Ativar Google Analytics e Meta Pixel
- [ ] Configurar SEO: meta description, OG tags, sitemap.xml, robots.txt
- [ ] Testar em todos os principais dispositivos (iPhone, Android, iPad, Desktop)

---

## Performance

Tamanho total do site após otimização:

| Arquivo | Tamanho |
|---|---|
| index.html | ~120 KB |
| Imagens (10 arquivos) | ~1.3 MB |
| **Total transferido na primeira visita** | **~1.4 MB** |
| **Total nas visitas seguintes (cache)** | **~120 KB** |

O `vercel.json` configura cache imutável de 1 ano para todas as imagens — visitas recorrentes carregam apenas o HTML.

---

## Suporte

Site desenvolvido pela **Rise Agência**.

Dúvidas técnicas ou solicitações de mudança: entre em contato com a equipe de desenvolvimento.

---

## Licença

Projeto proprietário. Conteúdo, design e código são de propriedade da Rise Agência e Imperium Forte. Reprodução não autorizada.
