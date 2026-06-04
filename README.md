# 🏦 SacraBank — Dashboard de Internet Banking

> Dashboard financeiro com cards de resumo e formulário de transferência — Projeto Acadêmico SENAI

**🔗 [Acesse o projeto ao vivo](#)** *(https://sacradev.github.io/dashboard-fintech/)*

---

## Visão Geral

Esse projeto é um dashboard de internet banking fictício desenvolvido como trabalho acadêmico no SENAI. A ideia foi simular a tela principal de um sistema financeiro — o **SacraBank** — com foco em dois blocos principais: um painel de resumo financeiro com cards de saldo, limite e fatura, e um formulário funcional de transferência com suporte a PIX, TED e DOC.

A identidade visual foi construída em torno de uma paleta rubro-negra — vermelho escuro e preto carbono — pensada para transmitir solidez e autoridade sem cair no clichê do azul bancário. O layout é baseado em CSS Grid com nav lateral, típico de dashboards de sistemas internos.

---

## Estrutura do Projeto

```
sacrabank/
├── index.html          # Estrutura HTML semântica do dashboard
├── css/
│   ├── base.css        # Variáveis HSL, reset universal e estilos globais
│   ├── layout.css      # Grid do container, posicionamento e Flexbox dos cards
│   └── components.css  # Estilo dos cards, formulário, inputs e botão
```

A separação em três arquivos CSS foi uma decisão intencional desde o início. O `base.css` concentra tudo que é global — variáveis de cor em HSL, reset de margin/padding e comportamentos de fonte. O `layout.css` cuida exclusivamente de onde cada coisa fica na página, sem tocar em aparência. O `components.css` é onde cada elemento ganha sua cara — bordas, cores, hover, transições.

Essa separação torna a manutenção mais racional: quando algo está mal posicionado, o problema está no `layout.css`. Quando algo está feio, está no `components.css`. Você não precisa varrer o arquivo inteiro pra encontrar o que quer.

---

## Layout e Estrutura

### Grid principal

O dashboard usa um CSS Grid de duas colunas no `.container`, com o nav lateral fixo em `14rem` e o `main` ocupando o restante com `1fr`. O header atravessa as duas colunas com `grid-column: span 2`.

```html
<div class="container">
  <header>...</header>      <!-- span 2 colunas -->
  <nav>...</nav>            <!-- coluna 1 -->
  <main>...</main>          <!-- coluna 2 -->
</div>
```

### Cards de resumo

Os três cards — Saldo, Limite e Fatura — ficam dentro de uma div `.saldos` com Flexbox e `flex-wrap: wrap`. Isso garante que os cards ocupem a largura total disponível e quebrem naturalmente em telas menores sem quebrar o layout.

Cada card é marcado como `<article>` porque representa uma unidade de informação independente — faz sentido fora do contexto do dashboard. Dentro de cada um, um `<h3>` para o label e um `<p>` para o valor, permitindo estilização individualizada.

```html
<article class="saldo">
  <h3 class="card-label">Saldo</h3>
  <p class="card-valor">R$ 2.000,00</p>
</article>
```

As cores semânticas entram aqui: verde (`--cor-positivo`) para saldo positivo, vermelho (`--cor-negativo`) para valores de alerta como limite e fatura.

### Formulário de transferência

O formulário usa `<fieldset>` + `<legend>` para agrupar os campos semanticamente — o que melhora a leitura por leitores de tela e mantém o HTML comunicativo mesmo sem CSS.

Os radio buttons de tipo de transação compartilham o mesmo `name="transacao"`, garantindo que só um possa ser selecionado. O campo numérico e o checkbox têm `name` explícito para que os dados cheguem corretamente no envio do formulário.

```html
<fieldset>
  <legend>Transferências</legend>
  <input type="number" name="valor-transferido" id="valor-transferido">
  <input type="radio" name="transacao" value="PIX" required> PIX
  <input type="radio" name="transacao" value="TED"> TED
  <input type="radio" name="transacao" value="DOC"> DOC
  <input type="checkbox" name="salvar-contato" value="salvar">
  <button type="submit" class="btn-enviar">Transferir</button>
</fieldset>
```

---

## Paleta de Cores

Todas as cores são definidas em HSL no `base.css`, o que facilita criar variações coerentes ajustando só o `L` (lightness) ou o `S` (saturation) sem sair da família cromática.

| Variável | HSL | Uso |
|---|---|---|
| `--cor-fundo-escuro` | `hsl(0, 0%, 10%)` | Fundo do nav e do main |
| `--cor-primaria` | `hsl(0, 85%, 40%)` | Vermelho principal — header e bordas |
| `--cor-primaria-hover` | `hsl(0, 85%, 33%)` | Hover de botões e links |
| `--cor-primaria-suave` | `hsl(0, 85%, 96%)` | Fundo do input |
| `--cor-positivo` | `hsl(142, 60%, 40%)` | Verde para saldo positivo |
| `--cor-negativo` | `hsl(0, 85%, 40%)` | Vermelho para valores de alerta |

---

## Decisões Técnicas

**Por que HSL e não HEX ou RGB?**
HSL permite controlar cor, saturação e brilho de forma independente. Para criar um hover mais escuro de um vermelho, basta reduzir o `L` — sem precisar calcular nada no olho. Em RGB, essa variação seria opaca.

**Por que `<article>` nos cards?**
A spec do HTML5 define `<article>` como conteúdo auto-contido que faria sentido distribuído de forma independente. Um card de saldo faz sentido fora do dashboard — poderia estar num widget, num email, num outro contexto. Isso justifica o uso semântico.

**Por que separar `name` de `id` nos inputs?**
O `id` existe para o front — conecta o `<label>` ao input via `for` e serve como gancho pra CSS e JavaScript. O `name` existe para o back — é a chave que identifica o campo no envio do formulário. Campos sem `name` são ignorados na submissão, independente de terem `id`.

**Por que `outline: none` com substituição?**
O `outline` padrão do navegador existe por acessibilidade — usuários de teclado dependem dele para saber onde estão na página. Removê-lo sem substituir é um erro de acessibilidade. A solução adotada foi `outline: none` + `border: 2px solid var(--cor-primaria)` no `:focus`, mantendo o feedback visual com a identidade do sistema.

---

## Funcionalidades

- **Nav lateral com hover** — links com efeito de fundo vermelho e `border-radius` no `:hover`
- **Cards semânticos com cores condicionais** — verde para positivo, vermelho para negativo
- **Formulário de transferência** com campo numérico, radio buttons (PIX/TED/DOC), checkbox e botão
- **Micro-interação no botão** — `transform: scale(0.98)` no `:active` simula o clique físico
- **Focus acessível nos inputs** — borda colorida substitui o outline nativo

---

## Tecnologias Utilizadas

- **HTML5** — com uso semântico de `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<fieldset>` e `<legend>`
- **CSS3** — sem frameworks, tudo escrito à mão, com CSS Grid, Flexbox e variáveis HSL
- **GitHub Pages** — hospedagem estática

---

## Como Executar

Projeto estático, sem dependências. Basta clonar e abrir no navegador:

```bash
git clone https://github.com/sacradev/dashboard-fintech.git
cd dashboard-fintech
# Abra o index.html no navegador ou use o Live Server no VS Code
```

---

## Autor

Desenvolvido por **Victor Sacramento** — Projeto Acadêmico SENAI · 2026
