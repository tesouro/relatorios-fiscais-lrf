# Relatórios Fiscais da União a partir de Dados Públicos

**Português** | [English](README.en.md)

Este repositório mostra, com código aberto, que é possível **reproduzir números oficiais das contas públicas federais usando apenas dados públicos** — sem acesso a sistemas internos do governo.

Não é uma proposta teórica: os números abaixo coincidem com o que a União publica oficialmente, na unidade em que os valores são divulgados (R$ mil).

> **Projeto experimental / piloto.** Esta é uma prova de conceito de transparência fiscal, em caráter exploratório. O objetivo é demonstrar a viabilidade da abordagem, não substituir os demonstrativos oficiais — a fonte oficial continua sendo o que a União publica no Diário Oficial. O que se demonstra aqui é a **reprodutibilidade computacional** dos valores publicados: a interpretação normativa de cada critério é a do Manual de Demonstrativos Fiscais e permanece aberta à discussão.

## 📄 Leia os relatórios

Não é preciso instalar nada nem saber programar. Cada link abaixo abre uma página
no navegador com a explicação dos critérios, o cálculo e a conferência contra o
valor publicado no Diário Oficial:

| Demonstrativo | O que mede |
|---|---|
| [**RGPS** — RREO Anexo 4](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | Déficit da Previdência do INSS |
| [**RCL** — RREO Anexo 3](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | Receita Corrente Líquida, a "régua" dos limites fiscais |
| [**Despesa com saúde** — RREO Anexo 12](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | Se a União aplicou o mínimo constitucional em saúde |
| [**Despesa com pessoal** — RGF Anexo 1](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | Quanto a União gasta com folha, no consolidado (parcela liquidada) |
| [**Pessoal por elemento** — RREO Tabela 2](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | Detalhamento da folha por tipo de despesa |

O restante deste documento explica como esses números foram obtidos e por que
isso importa.

## Por que é inovador

Não identificamos, no levantamento realizado, iniciativa governamental — no Brasil ou no exterior — que publique em conjunto os dados, o código e a conferência capazes de reproduzir seus próprios demonstrativos fiscais oficiais. Se houver, temos interesse em conhecer.

Existem iniciativas próximas, com propósitos distintos. Vale explicitar como cada uma se distingue, porque todas são instrutivas em si mesmas.

**Regras como código: Catala e OpenFisca.** O [Catala](https://catala-lang.org), desenvolvido no Inria (França), é o parente mais próximo do *método* adotado aqui. É uma linguagem específica de domínio concebida para *literate programming* legislativo: cada parágrafo do texto normativo é anotado com o código que o formaliza, e o compilador extrai tanto o programa executável quanto um documento legível por juristas. Foi desenhado em colaboração com professores de direito, incorpora a *lógica default* — a estrutura de "definição sob condições" que atravessa a redação legislativa — como recurso de primeira classe, e parte de seu compilador foi formalmente verificada. Nasceu de trabalho com a DGFiP, a administração tributária francesa, sobre o motor de cálculo do imposto de renda; a formalização das prestações familiares francesas em Catala revelou um erro na implementação oficial.

O Catala é um instrumento bem mais rigoroso do que o que usamos aqui — Quarto não é compilador verificado e R não é uma linguagem jurídica. Mas a premissa é a mesma, e vale enunciá-la: **a fidelidade entre regra e código deve ser legível pelo especialista do domínio, não aceita sob palavra de quem implementou.** A diferença está no objeto. O Catala formaliza o *texto normativo* para *calcular* um direito em um caso concreto; aqui se parte da *execução orçamentária realizada* para reconstruir um *demonstrativo agregado já publicado*. O [OpenFisca](https://openfisca.org), também de origem francesa e hoje usado em vários continentes, está no mesmo eixo: codifica a *legislação* para *simular* políticas, não para reproduzir demonstrativos a partir da execução real.

**Reproducible Analytical Pipelines (Reino Unido).** É o parente mais próximo do ponto de vista *operacional*. Criado em 2017 pelo Department for Culture, Media and Sport com o Government Digital Service e depois difundido pelo Government Statistical Service, o RAP respondeu a um problema que reconhecemos de imediato: estatísticas oficiais produzidas por processos manuais lentos e propensos a erro, com forte dependência de planilhas e software proprietário. Seu padrão mínimo — nenhuma etapa de copiar e colar, linguagens de código aberto, controle de versão, código publicado sempre que possível, documentação embutida, garantia de qualidade escrita no próprio código, revisão por pares — é o padrão que este repositório procurou atender.

A diferença de escopo merece precisão, porque é estreita. O RAP trata de como o produtor constrói *o próprio* pipeline de forma reprodutível, e diversas estatísticas britânicas já publicam o código desse pipeline. O que se acrescenta aqui é a segunda metade: uma **reconstrução independente, a partir de dado público de terceiro, conferida linha a linha contra o número que o governo publicou**. O leitor verifica o resultado sem precisar confiar nem em quem escreveu o código, nem na instituição que produz o demonstrativo.

A experiência britânica também ensina sobre o que *não* funciona: a revisão de 2021 do Office for Statistics Regulation trata sobretudo de barreiras de adoção, não de tecnologia.

**Reprodutibilidade na estatística pública (França).** O INSEE investiu na mesma direção pelo lado estatístico: o [utilitR](https://www.utilitr.org), documentação colaborativa de R escrita por e para estatísticos públicos, e o [SSP Cloud](https://www.sspcloud.fr), plataforma aberta em contêineres pensada para que o trabalho estatístico possa ser reexecutado, e não apenas documentado.

**Pacotes de acesso a dados (Brasil).** Pacotes do ecossistema R brasileiro — `tesouror`, `siconfir`, `RREORGFdataR` — dão acesso ágil às APIs do SICONFI e entregam o demonstrativo **já consolidado, na linha do anexo, como o ente o declarou**. São excelentes para análise comparativa entre entes, mas partem do resultado: a regra que transformou a execução naquele número permanece fora do alcance de quem consulta. Portais de dados abertos fazem o inverso — publicam a execução, sem a lógica que a converte em demonstrativo.

**O recorte deste projeto** é o trecho que falta entre os dois: partir do dado público de execução e chegar, com código aberto e auditável, exatamente ao número que o Tesouro publicou.

**E o que está aqui é uma amostra.** Já existe um pipeline interno, em Microsoft Fabric, que gera todos os anexos do RREO e do RGF com essa mesma abordagem — cada notebook documentado e conferido contra o Diário Oficial. Os cinco demonstrativos abaixo são os que a base pública disponível hoje permite reproduzir; o restante já está construído, aguardando apenas dado público com a granularidade necessária.

---

## 1. O ponto de partida: o caso do RGPS

Em 30/01/2026, o Jornal Nacional noticiou que o **Regime Geral de Previdência Social (RGPS)** — a Previdência dos trabalhadores privados, gerida pelo INSS — teve, em 2025, um déficit de **mais de R$ 320 bilhões** ([reportagem completa](https://g1.globo.com/google/amp/jornal-nacional/noticia/2026/01/30/gastos-com-previdencia-sao-os-que-mais-pesam-nas-contas-publicas-rombo-do-inss-em-2025-foi-de-mais-de-r-320-bilhoes.ghtml)).

Reproduzimos esse número usando só duas fontes públicas, sem qualquer acesso a sistema interno do governo:

- **Siga Brasil** (Senado Federal) — receitas arrecadadas
- **SOF** (Secretaria de Orçamento Federal) — despesas empenhadas

| Item | Calculado (R$ mil) | Oficial (R$ mil) | Diferença |
|---|---:|---:|---:|
| Total de Receitas | 709.399.406 | 709.399.406 | 0,00% |
| Total de Despesas | 1.030.366.445 | 1.030.366.445 | 0,00% |
| **Resultado Previdenciário** | **-320.967.039** | **-320.967.039** | **0,00%** |

A diferença é apurada após o arredondamento para R$ mil, unidade em que o Diário Oficial publica. Antes do arredondamento restam frações de milhar — da ordem de algumas centenas de reais sobre valores de centenas de bilhões —, próprias do ruído de ponto flutuante das bases de origem.

O resultado coincide com o que o Tesouro Nacional publicou oficialmente — o mesmo número que chegou ao Jornal Nacional. Ou seja: qualquer pessoa, sem acesso a sistema interno nenhum, pode chegar ao mesmo resultado — basta saber onde procurar o dado e como aplicar os critérios corretos.

**Onde está no repositório:**

- Código executável: [`standalones/rreo_a04_rgps.qmd`](standalones/rreo_a04_rgps.qmd) — documento Quarto que junta narrativa e cálculo no mesmo arquivo (basta rodar `quarto render`)
- Leitura direta no navegador, sem instalar nada: [versão renderizada em HTML](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html)
- Dados de entrada: `dados/rgps/receita_siga.xlsx` e `dados/rgps/despesa_sof.xlsx`
- Demonstrativo oficial para conferência: [RREO — Dezembro/2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12) (RREO Anexo 4)

**Por que isso importa:** o cidadão, o jornalista, o parlamentar — qualquer um — pode auditar essa conta independentemente do governo. A confiança no número se reforça porque ele pode ser verificado de forma independente.

---

## 2. Do caso único para cinco demonstrativos oficiais

Se conseguimos reproduzir o RGPS, o que mais dá para reproduzir com dado público?

Testamos e validamos **mais quatro demonstrativos exigidos pela Lei de Responsabilidade Fiscal (LRF)**, com convergência com os valores publicados no Diário Oficial (exercício 2025), na unidade em que são divulgados — com uma diferença residual de arredondamento na Tabela 2, da ordem de R$ 4 mil sobre R$ 431,7 bilhões, documentada no próprio arquivo:

| Demonstrativo | O que mede, em uma frase | Ler online | Fonte oficial para conferência |
|---|---|---|---|
| **RGPS** (RREO Anexo 4) | Déficit da Previdência do INSS | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a04_rgps.html) | RREO Dezembro/2025 |
| **RCL** (RREO Anexo 3) | Receita Corrente Líquida — a "régua" usada para medir limites fiscais | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a03_rcl.html) | RREO Dezembro/2025 |
| **Despesa com saúde** (RREO Anexo 12) | Se a União aplicou o mínimo constitucional em saúde | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_a12_saude.html) | RREO Dezembro/2025 |
| **Despesa com pessoal** (RGF Anexo 1, total consolidado) | Quanto a União gasta com folha de pagamento, no consolidado dos três Poderes — apurada aqui na **parcela liquidada**, sem Restos a Pagar Não Processados ([por quê](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html#nota-metodológica)) | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rgf_a01_pessoal.html) | RGF 2025 — Consolidado |
| **Pessoal por elemento da despesa** (RREO Tabela 2) | Detalhamento da folha por tipo de despesa | [abrir](https://tesouro.github.io/relatorios-fiscais-lrf/rreo_t02_pessoal.html) | RREO Dezembro/2025 |

Links das fontes oficiais: [RREO Dezembro/2025](https://www.tesourotransparente.gov.br/publicacoes/relatorio-resumido-da-execucao-orcamentaria-rreo/2025/12) · [RGF 2025 Consolidado](https://www.tesourotransparente.gov.br/publicacoes/relatorio-de-gestao-fiscal-rgf/2025/31)

Cada demonstrativo é um documento Quarto autocontido em `standalones/`: a narrativa dos critérios normativos e o código que os implementa moram no mesmo arquivo, de ponta a ponta, sem necessidade de navegar para outro lugar do repositório.

### Limitações conhecidas

Cada limitação abaixo já está documentada no arquivo correspondente. Estão reunidas aqui para leitura rápida — e porque um projeto que declara onde o dado público esbarra na apuração interna é mais útil do que um que só mostra o que fechou.

- **Despesa com pessoal (RGF Anexo 1) — escopo parcial.** Apura-se apenas a despesa *liquidada*. Os Restos a Pagar Não Processados, que integram a despesa com pessoal para fins do limite da LRF, não entram no cálculo; tampouco se apresenta a razão DTP/RCL ou a apuração de cumprimento dos arts. 19 e 20. A comparação é de valores, contra a parcela liquidada publicada no Diário Oficial. Os RP Não Processados estão disponíveis na base pública: a extensão é imediata a partir do mesmo pipeline.
- **Pessoal por elemento (Tabela 2) — hierarquia de órgão no Ministério da Defesa.** A apuração oficial separa Civil × Militar pelo órgão máximo da Unidade Gestora executora. Em 45 dos 49 órgãos máximos da União isso é indiferente, porque órgão máximo e órgão superior coincidem. No Ministério da Defesa, não: o MD é o órgão máximo, enquanto Comando da Marinha, do Exército e da Aeronáutica são órgãos superiores. A base pública expõe o órgão superior, e é dessa assimetria — restrita ao perímetro da Defesa — que vem a diferença de ~0,06% em dois itens da fronteira Civil/Militar. O total geral bate.
- **Despesa com saúde (Anexo 12) — critério de dimensão única.** O cálculo apoia-se no identificador de uso `6`, marcação que o próprio SIAFI atribui à despesa. É o critério do demonstrativo oficial e é rastreável, mas o dado público não oferece, hoje, uma segunda dimensão independente para conferir a marcação registro a registro.
- **Exaustividade das classificações — não testada automaticamente.** As regras reproduzem os valores oficiais, mas o repositório ainda não demonstra, de forma automatizada, que a classificação cobre 100% da base e que os critérios são mutuamente exclusivos. Coincidir com o oficial é evidência forte de correção computacional; não é, sozinho, prova de que cada premissa classificatória seja a única leitura possível do Manual de Demonstrativos Fiscais. Auditar as regras — e não apenas os números — é o próximo passo, e é o tipo de discussão que este repositório existe para permitir.
- **Recorte temporal e unidade.** Exercício de 2025, encerrado. Os valores oficiais usados na conferência estão registrados no corpo de cada documento; a comparação é feita em R$ mil, unidade em que o Diário Oficial publica.

**Um achado do projeto: uma única base de despesa cobre todos os demonstrativos testados.**

- `dados/despesa_unificada.csv.gz` — uma única base de despesa (Siga Brasil), sem quebra por mês — alimenta **Despesa com saúde, Despesa com pessoal, Pessoal por elemento da despesa e as deduções da RCL**. Para o exercício fechado, o saldo anual acumulado é suficiente; não é preciso organizar dado por pasta mensal.
- `dados/rcl_receita.csv.gz` — a **RCL** parte de receitas correntes, então precisa de uma base de receita própria. Sua única dedução medida pelo lado da despesa (Transferências Constitucionais) vem da **mesma base unificada** dos outros três, que passou a incluir Programa e os Restos a Pagar cancelados.
- O **RGPS** (caso do item 1) foi o primeiro demonstrativo testado, ainda com fontes separadas: receita do Siga Brasil e despesa do Portal de Dados Abertos da SOF. As descobertas posteriores mostraram que ele também pode ser gerado a partir da base unificada — mantivemos as extrações originais (`dados/rgps/`) como registro do percurso e reconhecimento à SOF, fonte importante de dados abertos.

Ou seja: não é preciso montar uma infraestrutura de dados nova para cada relatório — poucas bases públicas bem construídas já cobrem vários demonstrativos ao mesmo tempo.

**Duas lições que valem para a direção:**

- **Publicar dado não é o mesmo que garantir reprodutibilidade.** O Siga Brasil e a SOF já existem e já são públicos — o que faltava não era o dado, era a lógica de cálculo (os critérios, os filtros, as exceções). Isso está tudo neste repositório, documentado e auditável.
- **O código aberto torna cada critério inspecionável.** Qualquer pessoa pode acompanhar como o número foi calculado, linha por linha, do dado bruto ao valor publicado.

**Onde está no repositório:** pasta [`standalones/`](standalones/) — um documento Quarto por demonstrativo.

---

## 3. O motor completo: o que já está pronto

Os cinco demonstrativos acima são o que a base pública disponível hoje permite reproduzir. O pipeline interno vai bem além disso.

São **17 notebooks em R**, executados em Microsoft Fabric sobre SparkR e Delta Lake, cobrindo o conjunto dos anexos do RREO e do RGF hoje elaborados pelo Tesouro Nacional:

- **RREO** — Anexos 1, 2, 3, 4, 6, 7, 8, 9 e 12, e Tabelas 1, 2 e 3
- **RGF** — Anexos 1 a 5

Cada notebook segue a mesma abordagem deste repositório: o critério do MDF escrito em português, o código que o executa logo abaixo, e a conferência contra o valor publicado no Diário Oficial.

E não é só o cálculo. Sobre a mesma base já operam o modelo semântico em **Power BI (Direct Lake)**, a **validação cruzada entre RREO e RGF**, a construção de **série histórica** e o compartilhamento dos dados com outras áreas do Tesouro — tudo a partir de uma fonte única, sem replanilhamento.

**A limitação hoje não é o código, é a granularidade do dado público.** Os anexos que não estão neste repositório dependem de atributos hoje disponíveis apenas no Tesouro Gerencial — entre outros, saldo de conta contábil, conta corrente por entidade, situação do evento contábil e fonte de recursos por conta contábil. Na medida em que dados com essa granularidade venham a estar disponíveis publicamente, a lógica de cálculo correspondente já está construída e testada.

Esse pipeline opera sobre extrações do Tesouro Gerencial, que não são públicas — por isso não integra este repositório, cujo escopo é deliberadamente o que qualquer pessoa consegue reproduzir com dado aberto. As informações desta seção servem à contextualização arquitetural: por dependerem de dado não público, não são verificáveis por quem não tem acesso ao Tesouro Gerencial, e nada do que se afirma aqui é pressuposto dos cinco demonstrativos acima.

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
├── LICENSE
├── README.md                      # este documento (português)
└── README.en.md                   # versão em inglês
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

## Referências

- Merigoux, D., Chataing, N. & Protzenko, J. (2021). *Catala: A Programming Language for the Law.* Proc. ACM Program. Lang. 5, ICFP. <https://arxiv.org/abs/2103.03198>
- Projeto Catala — <https://catala-lang.org> · manual de referência: <https://book.catala-lang.org>
- Inria, *Catala translates law into code for more reliable administration* — <https://www.inria.fr/en/catala-software-dgfip-cnaf>
- OpenFisca — <https://openfisca.org>
- UK Government Analysis Function, *Reproducible Analytical Pipelines* — <https://analysisfunction.civilservice.gov.uk>
- Office for Statistics Regulation (2021), *Reproducible Analytical Pipelines: Overcoming barriers to adoption* — <https://osr.statisticsauthority.gov.uk>
- utilitR — <https://www.utilitr.org> · SSP Cloud — <https://www.sspcloud.fr>
- Knuth, D. E. (1984). *Literate Programming.* The Computer Journal, 27(2), 97-111.
- Brasil, Lei Complementar nº 101/2000 (LRF) e *Manual de Demonstrativos Fiscais* (STN)

## Licença

Código sob [licença MIT](LICENSE). Dados públicos, de livre uso.
