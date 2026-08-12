# Relatórios Fiscais da União a partir de Dados Públicos

Este repositório mostra, com código aberto, que é possível **reproduzir números oficiais das contas públicas federais usando apenas dados públicos** — sem acesso a sistemas internos do governo.

Não é uma proposta teórica: os números abaixo batem, casa decimal por casa decimal, com o que a União publica oficialmente.

> **Projeto experimental / piloto.** Esta é uma prova de conceito de transparência fiscal, em caráter exploratório. O objetivo é demonstrar a viabilidade da abordagem, não substituir os demonstrativos oficiais — a fonte oficial continua sendo o que a União publica no Diário Oficial.

## 📄 Leia os relatórios

Não é preciso instalar nada nem saber programar. Cada link abaixo abre uma página
no navegador com a explicação dos critérios, o cálculo e a conferência contra o
valor publicado no Diário Oficial:

| Demonstrativo | O que mede |
|---|---|
| [**RGPS** — RREO Anexo 4](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | Déficit da Previdência do INSS |
| [**RCL** — RREO Anexo 3](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | Receita Corrente Líquida, a "régua" dos limites fiscais |
| [**Despesa com saúde** — RREO Anexo 12](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | Se a União aplicou o mínimo constitucional em saúde |
| [**Despesa com pessoal** — RGF Anexo 1](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | Quanto a União gasta com folha, nos três Poderes |
| [**Pessoal por elemento** — RREO Tabela 2](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | Detalhamento da folha por tipo de despesa |

O restante deste documento explica como esses números foram obtidos e por que
isso importa.

## Por que é inovador

Até onde levantamos, **não há registro de outro governo ou ente — no Brasil ou no exterior — que disponibilize, de forma aberta e conjunta, os dados e o código capazes de reproduzir seus demonstrativos fiscais oficiais, validados contra o valor publicado.**

Existem iniciativas próximas, mas com propósito diferente. Portais de dados abertos (no Brasil e no mundo) publicam os *dados* de execução, mas não a lógica de cálculo que transforma esses dados no demonstrativo oficial. Ferramentas de "regras como código" — como o [OpenFisca](https://openfisca.org), adotado por vários países — transformam a *legislação* tributária e de benefícios em código para *simular* políticas ("e se mudarmos essa regra?"), mas não reproduzem demonstrativos fiscais já publicados a partir da execução orçamentária real.

O recorte deste projeto é específico: partir de dado público de execução e chegar, com código aberto e auditável, exatamente ao número que o Tesouro publicou — conferível centavo a centavo.

---

## 1. O ponto de partida: o caso do RGPS

Em 30/01/2026, o Jornal Nacional noticiou que o **Regime Geral de Previdência Social (RGPS)** — a Previdência dos trabalhadores privados, gerida pelo INSS — teve, em 2025, um déficit de **mais de R$ 320 bilhões** ([reportagem completa](https://g1.globo.com/google/amp/jornal-nacional/noticia/2026/01/30/gastos-com-previdencia-sao-os-que-mais-pesam-nas-contas-publicas-rombo-do-inss-em-2025-foi-de-mais-de-r-320-bilhoes.ghtml)).

Reproduzimos esse número usando só duas fontes públicas, sem qualquer acesso a sistema interno do governo:

- **Siga Brasil** (Senado Federal) — receitas arrecadadas
- **SOF** (Secretaria de Orçamento Federal) — despesas empenhadas

| Item | Calculado (R$ mil) | Oficial (R$ mil) | Diferença |
|---|---|---|---|
| Total de Receitas | 709.399.406 | 709.399.406 | 0,00% |
| Total de Despesas | 1.030.366.445 | 1.030.366.445 | 0,00% |
| **Resultado Previdenciário** | **-320.967.039** | **-320.967.039** | **0,00%** |

O resultado bate, centavo a centavo, com o que o Tesouro Nacional publicou oficialmente — o mesmo número que chegou ao Jornal Nacional. Ou seja: qualquer pessoa, sem acesso a sistema interno nenhum, pode chegar ao mesmo resultado — basta saber onde procurar o dado e como aplicar os critérios corretos.

**Onde está no repositório:**

- Código executável: [`standalones/rreo_a04_rgps.qmd`](standalones/rreo_a04_rgps.qmd) — documento Quarto que junta narrativa e cálculo no mesmo arquivo (basta rodar `quarto render`)
- Leitura direta no navegador, sem instalar nada: [versão renderizada em HTML](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html)
- Dados de entrada: `dados/rgps/receita_siga.xlsx` e `dados/rgps/despesa_sof.xlsx`
- Demonstrativo oficial para conferência: [RREO — Dezembro/2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12) (RREO Anexo 4)

**Por que isso importa:** o cidadão, o jornalista, o parlamentar — qualquer um — pode auditar essa conta independentemente do governo. A confiança no número não depende de "confiar no Tesouro"; ela pode ser verificada.

---

## 2. Do caso único para cinco demonstrativos oficiais

Se conseguimos reproduzir o RGPS, o que mais dá para reproduzir com dado público?

Testamos e validamos **mais quatro demonstrativos exigidos pela Lei de Responsabilidade Fiscal (LRF)**, convergência centavo a centavo com o Diário Oficial (exercício 2025) — com uma diferença residual de arredondamento na Tabela 2, documentada no próprio arquivo:

| Demonstrativo | O que mede, em uma frase | Ler online | Fonte oficial para conferência |
|---|---|---|---|
| **RGPS** (RREO Anexo 4) | Déficit da Previdência do INSS | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | RREO Dezembro/2025 |
| **RCL** (RREO Anexo 3) | Receita Corrente Líquida — a "régua" usada para medir limites fiscais | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | RREO Dezembro/2025 |
| **Despesa com saúde** (RREO Anexo 12) | Se a União aplicou o mínimo constitucional em saúde | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | RREO Dezembro/2025 |
| **Despesa com pessoal** (RGF Anexo 1, total consolidado) | Quanto a União gasta com folha de pagamento, nos três Poderes | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | RGF 2025 — Consolidado |
| **Pessoal por elemento da despesa** (RREO Tabela 2) | Detalhamento da folha por tipo de despesa | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | RREO Dezembro/2025 |

Links das fontes oficiais: [RREO Dezembro/2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12) · [RGF 2025 Consolidado](https://www.tesourotransparente.gov.br/publicacoes/relatorio-de-gestao-fiscal-rgf/2025/31)

Cada demonstrativo é um documento Quarto autocontido em `standalones/`: a narrativa dos critérios normativos e o código que os implementa moram no mesmo arquivo, de ponta a ponta, sem necessidade de navegar para outro lugar do repositório.

**Um achado do projeto: duas bases públicas cobrem quatro desses cinco demonstrativos.**

- `dados/despesa_unificada.csv.gz` — uma única base de despesa (Siga Brasil), sem quebra por mês — alimenta **Despesa com saúde, Despesa com pessoal, Pessoal por elemento da despesa e as deduções da RCL**. Para o exercício fechado, o saldo anual acumulado é suficiente; não é preciso organizar dado por pasta mensal.
- `dados/rcl_receita.csv.gz` — a **RCL** parte de receitas correntes, então precisa de uma base de receita própria. Sua única dedução medida pelo lado da despesa (Transferências Constitucionais) vem da **mesma base unificada** dos outros três, que passou a incluir Programa e os Restos a Pagar cancelados.
- O **RGPS** (caso do item 1) usa suas próprias extrações (`dados/rgps/`) porque combina uma fonte de receita (Siga Brasil) com uma fonte de despesa diferente (SOF) — essa é uma característica específica do RGPS, não uma limitação geral da abordagem.

Ou seja: não é preciso montar uma infraestrutura de dados nova para cada relatório — poucas bases públicas bem construídas já cobrem vários demonstrativos ao mesmo tempo.

**Duas lições que valem para a direção:**

- **Publicar dado não é o mesmo que garantir reprodutibilidade.** O Siga Brasil e a SOF já existem e já são públicos — o que faltava não era o dado, era a lógica de cálculo (os critérios, os filtros, as exceções). Isso está tudo neste repositório, documentado e auditável.
- **Código aberto (R) é auditável; planilha fechada não é.** Qualquer pessoa pode ler exatamente como cada número foi calculado, linha por linha — diferente de uma "caixa-preta" onde é preciso confiar sem verificar.

**Onde está no repositório:** pasta [`standalones/`](standalones/) — um documento Quarto por demonstrativo.

---

## 3. O potencial: o motor completo já existe e já foi testado

Os cinco demonstrativos acima usam só dado público. Mas, internamente, a STN já opera um **pipeline completo em Microsoft Fabric** — testado e validado centavo a centavo contra os valores oficiais do Diário Oficial — que gera **todos** os anexos do RREO e do RGF, não só esses cinco.

Isso quer dizer: **a limitação hoje não é o código, é a disponibilidade de dado público equivalente** para os demais anexos, que hoje dependem de granularidade só disponível no Tesouro Gerencial (saldo de conta contábil, conta corrente por entidade, Unidade Gestora). Se, no futuro, essas bases forem abertas, a lógica de cálculo para replicar os anexos restantes já está pronta e testada — é só conectar.

Esse pipeline interno opera sobre extrações do Tesouro Gerencial, que não são públicas — por isso não integra este repositório, cujo escopo é deliberadamente o que qualquer pessoa consegue reproduzir com dado aberto.

---

## Estrutura do repositório

```
relatorios-fiscais-lrf/
├── standalones/                   # um documento Quarto por demonstrativo
│   ├── rreo_a04_rgps.qmd          # RGPS — o caso do Jornal Nacional
│   ├── rreo_a03_rcl.qmd           # RCL — cada dedução da LRF explicada
│   ├── rreo_a12_saude.qmd         # Despesa com saúde (mínimo constitucional)
│   ├── rgf_a01_pessoal.qmd        # Despesa com pessoal — regras uma a uma
│   └── rreo_t02_pessoal.qmd       # Pessoal por elemento — Civil × Militar
├── dados/                         # bases públicas, versionadas junto do código
│   ├── despesa_unificada.csv.gz   # base única de despesa 2025 (Saúde, RGF A1, Tab. 2, RCL)
│   ├── rcl_receita.csv.gz         # receita 2025 (RCL)
│   └── rgps/                      # bases próprias do RGPS (Siga Brasil + SOF)
├── docs/                          # versões em HTML, publicadas em tesouro.github.io/relatorios-fiscais-lrf
└── README.md
```

---

## Como reproduzir

Requer R (≥ 4.1) e [Quarto](https://quarto.org).

```r
install.packages(c("dplyr", "stringr", "readr", "readxl", "tidyr", "knitr"))
```

**Gerar um demonstrativo:**
```sh
quarto render standalones/rreo_a04_rgps.qmd
```

**Gerar todos de uma vez:**
```sh
quarto render standalones/
```

Ou, no RStudio, abra qualquer `.qmd` e clique em **Render**.

Os documentos localizam sozinhos a pasta `dados/`, então funcionam sem
configurar caminhos nem usar `setwd()`. Nenhuma credencial, acesso interno ou
sistema do governo é necessário.

**Ler sem executar nada:** os links no [início deste documento](#-leia-os-relatórios)
abrem a versão já renderizada de cada demonstrativo — narrativa, código, resultado
e a conferência linha a linha contra o Diário Oficial, em uma única página. Os
mesmos arquivos estão na pasta [`docs/`](docs/), caso queira baixá-los.

---

## Metodologia

O projeto adota os princípios do **Literate Programming** (Knuth, 1984): cada demonstrativo descreve, em linguagem natural, os critérios normativos (Manual de Demonstrativos Fiscais da STN) e os implementa em código R verificável — a metodologia fica explícita, não apenas o resultado final.

Os documentos **Quarto executáveis** (`.qmd`) são a expressão mais completa dessa ideia: juntam a explicação de cada critério e o código que o executa no mesmo arquivo, pensados para que alguém da área de contabilidade — mesmo sem experiência em programação — acompanhe cada passo. **Todos os cinco demonstrativos têm sua versão `.qmd` didática:** RGPS (`rreo_a04_rgps.qmd`), RCL (`rreo_a03_rcl.qmd`, com cada dedução da LRF explicada), Despesa com saúde (`rreo_a12_saude.qmd`), Despesa com pessoal (`rgf_a01_pessoal.qmd`, com as regras de inclusão/exclusão abertas uma a uma) e Pessoal por elemento (`rreo_t02_pessoal.qmd`, com a separação Civil × Militar explicada). Cada arquivo é autossuficiente: lê os dados, aplica os critérios, apresenta o resultado e o confere contra o valor publicado no Diário Oficial. Para gerar a versão em HTML: `quarto render <arquivo>.qmd`.

## Licença

Código sob licença MIT. Dados públicos, de livre uso.
