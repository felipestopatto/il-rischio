# 🇮🇹 Il Rischio
### Monitor de Estresse Soberano Italiano e Impacto no Brasil

> Developed by **Felipe S. S. Alves** — Economics & Finance, Università Bocconi  
> In the context of the **Bocconi Students Emerging Markets Club**

---

## O Problema

Equipes de crédito e research no Brasil identificam deterioração setorial **depois** que ela já aconteceu — quando o spread já abriu, quando a inadimplência já subiu, quando o rating já caiu.

A Itália carrega quase **140% de dívida pública sobre PIB** e passa por episódios recorrentes de estresse soberano. Esses episódios não ficam na Europa: eles se propagam para economias emergentes via canal de liquidez, custo de funding e sentimento de mercado — **antes que qualquer dado local capture o movimento**.

O Il Rischio resolve esse problema: transforma sinais europeus em inteligência acionável para decisões de crédito e investimento no Brasil.

---

## Como Funciona

O monitor coleta semanalmente **12 indicadores** de 4 fontes públicas e gratuitas, calcula um **score de estresse normalizado via Z-score** para 3 dimensões e gera automaticamente um **relatório PDF** com interpretação em linguagem natural.

### Arquitetura

```
Fontes de dados          Processamento           Output
─────────────────        ─────────────────       ──────────────────
FRED API          ──┐
yfinance          ──┤──▶ Normalização    ──▶ Score Itália  ──┐
Banco Central BR  ──┤    Z-score +           Score Energia ──┤──▶ PDF Semanal
                  ──┘    Pesos               Score Agro    ──┤    Dashboard
                                             Score Geral   ──┘
```

### Indicadores por Dimensão

**🇮🇹 Risco Soberano Italiano (peso 40% no score geral)**
| Indicador | Fonte | Peso |
|---|---|---|
| BTP-Bund Spread (10Y) | FRED API | 35% |
| Capital Flow (taxa overnight) | FRED API | 20% |
| EUR/BRL | yfinance | 20% |
| EWI — iShares MSCI Italy ETF | yfinance | 15% |
| FTSE MIB | yfinance | 10% |

**⚡ Energia & Infraestrutura (peso 30% no score geral)**
| Indicador | Fonte | Peso |
|---|---|---|
| Selic (% a.a.) | Banco Central do Brasil | 40% |
| CPFE3 — CPFL Energia | yfinance | 25% |
| TAEE11 — Taesa | yfinance | 20% |
| EUR/BRL | yfinance | 15% |

**🌾 Agronegócio (peso 30% no score geral)**
| Indicador | Fonte | Peso |
|---|---|---|
| Inadimplência setorial agro | Banco Central do Brasil | 30% |
| Soja (CBOT) | yfinance | 25% |
| Açúcar (ICE) | yfinance | 20% |
| AGRO3 — Brasil Agro | yfinance | 15% |
| SLCE3 — SLC Agrícola | yfinance | 10% |

### Metodologia do Score

Cada indicador é normalizado via **Z-score** em relação aos últimos 6 meses:

```
Z = (valor_atual - média_histórica) / desvio_padrão
```

O Z-score é convertido para percentil via função sigmóide e multiplicado pelo peso do indicador. O score final varia entre **0 e 100**:

| Score | Status | Interpretação |
|---|---|---|
| 0 – 30 | 🟢 Normal | Sem sinais de estresse |
| 30 – 60 | 🟡 Atenção | Sinais moderados |
| 60 – 80 | 🟠 Alerta | Estresse relevante |
| 80 – 100 | 🔴 Crítico | Transmissão provável para o Brasil |

---

## Output — Relatório Semanal

O projeto gera automaticamente um **PDF semanal** contendo:

- Score geral Il Rischio + scores setoriais com classificação de alerta
- Gráfico de evolução histórica dos scores (6 meses)
- Interpretação automática em linguagem natural
- Tabela detalhada de todos os indicadores com variação semanal

📄 **[Ver exemplo de relatório — 09 de maio de 2026](il_rischio_20260509.pdf)**

---

## Como Rodar

O projeto roda inteiramente no **Google Colab** — sem instalação local necessária.

**1.** Abra o notebook no Colab:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

**2.** Obtenha uma chave gratuita da FRED API em:  
[fred.stlouisfed.org/docs/api/api_key.html](https://fred.stlouisfed.org/docs/api/api_key.html)

**3.** Substitua no código:
```python
SUA_CHAVE_FRED = "sua_chave_aqui"
```

**4.** Execute todas as células em ordem — o PDF é gerado automaticamente ao final.

### Dependências
```
pandas
yfinance
requests
fredapi
matplotlib
reportlab
```

---

## Contexto e Motivação

Este projeto foi desenvolvido durante o primeiro ano de graduação em **Economia Internacional e Finanças na Università Bocconi**, em Milão — no contexto das atividades da divisão de Américas do **Bocconi Students Emerging Markets Club**.

Vivendo na Itália, acompanho de perto a dinâmica do risco soberano italiano e sua relação com os mercados globais. A hipótese central do projeto — que episódios de estresse soberano europeu precedem deterioração de crédito em mercados emergentes — é fundamentada na literatura de contágio financeiro internacional e na observação direta do comportamento dos spreads italianos em períodos de tensão política e fiscal.

A escolha dos setores de **Energia & Infraestrutura** e **Agronegócio** reflete sua relevância para o mercado de capitais de dívida (DCM) brasileiro: são os dois setores com maior volume de emissões de debêntures incentivadas e CRAs, respectivamente, e os mais expostos a variações no custo de funding de longo prazo.

---

## Autor

**Felipe S. S. Alves**  
Economics & Finance — Università Bocconi (2024–presente)  
[linkedin.com/in/felipestopatto](https://linkedin.com/in/felipestopatto)

---

*Il Rischio é um projeto acadêmico independente. As informações contidas nos relatórios têm caráter exclusivamente analítico e não constituem recomendação de investimento.*
