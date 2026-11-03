---
name: "apresentacoes-moura-siqueira"
description: "Cria ou revisa apresentações, decks, relatórios visuais e cases da Moura & Siqueira Consultoria no DNA Visual do escritório, entregando sempre um HTML interativo de slides e o PDF correspondente."
---

# Apresentações Moura & Siqueira — Guia operacional

Use este guia sempre que for criar ou revisar uma apresentação, deck, relatório visual ou case da Moura & Siqueira Consultoria, nas áreas contábil, tributária, financeira e empresarial. Ele resume o estudo de 15 referências (Bain, McKinsey, BCG, Palantir, Adyen, StoneCo, Linear e outras) e o manual de logo do escritório. Não é preciso consultar os links originais.

Objetivo: qualidade comparável à das grandes consultorias internacionais. Princípio: "Rigor de consultoria, calma de banco privado". Cada slide responde a uma pergunta do cliente com uma conclusão, prova essa conclusão com um dado e deixa o resto em silêncio. A interatividade deixa o cliente explorar essa prova, sem substituir a conclusão.

## 0. Entrega: sempre HTML interativo + PDF

Toda apresentação sai em dois arquivos com o mesmo nome-base, salvos na pasta de trabalho, dentro de `apresentacoes/`:

- `AAAA-MM-DD_Cliente_Tema.html`: o deck interativo, para apresentar na tela e enviar ao cliente.
- `AAAA-MM-DD_Cliente_Tema.pdf`: a mesma apresentação, um slide por página, com o estado padrão de cada interação.

Só gere .pptx se o usuário pedir.

### 0.1 Antes de começar: os arquivos da marca

Procure os arquivos do logo nesta ordem:

1. pasta `marca/` dentro da pasta de trabalho;
2. `~/MouraSiqueira/marca/` (no Windows: `%USERPROFILE%\MouraSiqueira\marca\`);
3. qualquer arquivo com "logo" ou "moura" no nome dentro da pasta de trabalho.

São necessárias duas versões: a colorida, para fundo claro, e a branca (negativa), para fundo escuro. Prefira PNG com fundo transparente ou SVG. Se não encontrar o logo, pergunte ao usuário onde ele está antes de produzir. Nunca redesenhe o logo à mão.

### 0.2 Estrutura do HTML

- Gere **um único arquivo autocontido**. Embuta o logo e as imagens em base64 (`data:image/png;base64,...`). Todo o CSS e o JavaScript ficam dentro do arquivo, em JavaScript puro, sem frameworks e sem bibliotecas externas.
- Carregue a Montserrat do Google Fonts, com os pesos 300, 400, 500, 600 e 700: `<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap" rel="stylesheet">`. Fallback: `Montserrat, "Segoe UI", Arial, sans-serif`.
- Cada slide é um `<section class="slide">` com tamanho fixo de **1920×1080 px**.
- Declare as cores da seção 3 como variáveis CSS em `:root` (`--marinho`, `--branco`, `--grafite`, `--royal`, `--aco`, `--ciano`, `--dourado`, `--indigo`, `--cinza-contexto`, `--cinza-nevoa`, `--marinho-card`) e use só essas variáveis.
- **Modo tela** (`@media screen`):
  - um slide por vez, escalado para caber na janela (`transform: scale()` calculado pela largura e pela altura) e centralizado sobre fundo `#000`;
  - o slide atual fica na URL (`#5`).
- **Modo impressão** (`@media print`):
  - `@page { size: 1920px 1080px; margin: 0; }`;
  - todos os slides visíveis, sem escala, um por página (`break-after: page`);
  - controles, menus e painéis flutuantes escondidos;
  - `-webkit-print-color-adjust: exact; print-color-adjust: exact;`.
  - Cada interação aparece no seu **estado padrão** (ver 0.3): revelações todas visíveis, cenário recomendado selecionado e detalhes essenciais abertos.
- Gráficos e diagramas são **SVG desenhado no próprio HTML**, com os valores escritos.

### 0.3 Interatividade (o máximo possível, sempre a serviço da mensagem)

A apresentação deve ser explorável, não só assistida. Em cada slide, pergunte: "que pergunta o cliente faria aqui, e como ele pode responder sozinho com um clique?". Use os recursos abaixo sempre que fizerem sentido. Cada slide de análise deve ter pelo menos um.

**A. Navegação (em todo deck)**

- Setas ← →, PageUp/PageDown e espaço. Clique na metade direita ou esquerda da tela. Deslizar o dedo no celular ou tablet.
- Barra de progresso fina no rodapé da tela, em ciano, fora do slide.
- **Menu de capítulos** (tecla M ou ícone ☰ no canto): lista os capítulos e salta direto para eles.
- **Visão geral** (tecla O): grade com miniaturas de todos os slides; clique em uma para ir até ela.
- **Agenda clicável**: cada item da agenda e do sumário executivo leva ao slide correspondente.
- Tela cheia (tecla F). Contador "3 / 18".
- **Modo apresentador** (tecla P): abre uma segunda janela com o slide atual, o próximo, um cronômetro e as notas do apresentador. Guarde as notas em `<aside class="notas">` dentro de cada slide, ocultas na tela e no PDF. As duas janelas ficam sincronizadas por `BroadcastChannel`.
- Tecla ? mostra um painel com todos os atalhos.

**B. Revelação progressiva**

- Em slides com argumento em etapas (cascata, linha do tempo, três pilares, lista de oportunidades), os elementos aparecem um a um a cada clique ou seta, com fade de 200 ms.
- Marque esses elementos com `data-passo="1"`, `"2"`... A seta só troca de slide depois do último passo.
- No PDF, todos os passos aparecem.

**C. Comparar cenários**

- Em comparativos (regime atual × proposto, Simples × Presumido × Real, com × sem holding), use **botões de alternância** (abas ou um seletor segmentado) que trocam os números, o gráfico e o título-conclusão, com transição suave dos valores.
- O estado padrão, que vai para o PDF, é o cenário recomendado. No PDF, inclua também uma pequena tabela com os números de todos os cenários.

**D. Simulador**

- Quando houver uma regra de cálculo clara, crie um slide-simulador. Exemplos: economia em função do faturamento, ou carga tributária em função da margem.
- Controle deslizante ou campo numérico; o resultado recalcula na hora, em número-herói e gráfico.
- **Regra de segurança:** o simulador só usa fórmulas, alíquotas e premissas fornecidas pelo usuário ou tiradas de documento do caso. Mostre as premissas no próprio slide ("Premissas: …") e o aviso "Simulação ilustrativa; não substitui o estudo do caso". Sem fórmula confirmada, não faça simulador: pergunte.
- Limite o controle a uma faixa realista informada pelo usuário.
- No PDF, o simulador aparece com o valor do caso real e uma pequena tabela de sensibilidade (3 a 5 valores).

**E. Detalhe sob demanda**

- Gráficos com **dica ao passar o mouse ou tocar**: valor exato, período e fonte.
- Clicar numa barra ou num item da legenda destaca aquela série e apaga as outras para o cinza.
- Botão discreto "Ver detalhe" (ou ⓘ) que abre um painel lateral com a memória de cálculo, a base legal citada pelo usuário ou a tabela completa. O slide continua limpo; a profundidade fica a um clique.
- No PDF, os detalhes essenciais (memória de cálculo) vão para um slide de anexo no final, e não dentro do slide.

**F. Diagramas exploráveis**

- Estrutura societária e holding: clicar em uma empresa ou sócio mostra participação, papel e o que muda.
- Linha do tempo: clicar num marco mostra data, responsável e entregáveis.
- Matriz 2×2: clicar num ponto mostra a ação e o ganho estimado.

**G. Fechamento interativo**

- O slide de próximos passos tem um checklist clicável das decisões pedidas e, se o usuário informar, um botão com link para WhatsApp ou e-mail do escritório ("Agendar reunião").
- Nunca invente telefone, e-mail ou link.

**Regras da interatividade**

- O título-conclusão e a mensagem principal ficam visíveis sem clique nenhum. A interação aprofunda; não esconde a resposta.
- Toda área clicável tem sinal visual discreto: cursor de mão, sublinhado fino em azul aço ou ciano, ou o ícone ⓘ. Nada de botões grandes coloridos.
- Animações: só fade e transição de valores, entre 200 e 400 ms. Nada de giros, saltos, zoom ou efeitos de entrada decorativos.
- Respeite `prefers-reduced-motion`: com ele ativo, troque as animações por mudança imediata.
- Tudo deve funcionar no teclado, no mouse e no toque, inclusive no celular, onde o cliente costuma abrir o arquivo recebido.
- Teste cada interação antes de entregar. Se o Playwright estiver disponível, clique nos controles por script e confira que nada quebra nem gera erro no console.

### 0.4 Como gerar o PDF a partir do HTML

O PDF é sempre impresso do próprio HTML, em modo impressão, para que os dois arquivos fiquem idênticos. Tente nesta ordem:

1. **Playwright (Python ou Node), se estiver instalado:** abra o HTML, espere as fontes carregarem (`document.fonts.ready`) e use `page.pdf(width="1920px", height="1080px", print_background=True, margin=0)`.
2. **Microsoft Edge ou Google Chrome em modo headless** (no Windows, o Edge já vem instalado):
   ```powershell
   & "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --headless --disable-gpu --no-pdf-header-footer --virtual-time-budget=5000 --print-to-pdf="C:\caminho\arquivo.pdf" "file:///C:/caminho/arquivo.html"
   ```
   Se o Edge não estiver nesse caminho, procure `msedge.exe` ou `chrome.exe` em `Program Files` e `Program Files (x86)`. No macOS e no Linux, use o Chrome ou o Chromium com os mesmos argumentos.
3. Se nada funcionar, entregue o HTML e explique ao usuário como gerar o PDF: abrir o HTML no navegador, apertar Ctrl+P, escolher o destino "Salvar como PDF", margens "Nenhuma" e marcar "Gráficos de plano de fundo".

Depois, **confira o PDF**:

- número de páginas = número de slides (mais os anexos);
- páginas em 16:9;
- fundos escuros presentes;
- fonte Montserrat;
- interações no estado padrão;
- nenhum controle, menu ou texto cortado.

Se possível, rasterize 2 ou 3 páginas e olhe as imagens. Se algo falhar, corrija o HTML e gere de novo.

## 1. Antes de desenhar: a história

1. Escreva primeiro só os títulos de todos os slides, indicando ao lado de cada um a interação planejada (ex.: "alternância entre 3 regimes", "simulador por faturamento"). Lidos em sequência, os títulos precisam contar a história completa. Mostre esse roteiro ao usuário e peça validação antes de produzir o HTML.
2. Use a pirâmide: a resposta vem primeiro (sumário executivo), depois as provas.
3. Siga situação → complicação → resolução. Exemplo: "Sua empresa paga X" → "O regime atual custa Y a mais" → "Com o planejamento, a economia é Z".
4. O último slide pede uma decisão concreta: aprovar, agendar ou assinar.
5. O título é sempre uma conclusão com número ("A troca para o Lucro Presumido reduz a carga em 18%"), nunca um rótulo ("Comparativo de regimes").
6. Nunca invente números, fontes ou fundamentos legais. Dado que não veio do usuário nem de um documento fica marcado como "a confirmar".

## 2. Logo

O logo tem um símbolo MS em linhas diagonais a 45°, com pontas arredondadas e degradê de #18073D (índigo, embaixo) a #055695 (azul aço, no topo). O nome vem em Montserrat: "MOURA & SIQUEIRA" em Bold e "CONSULTORIA" em Light espaçado, na cor #333333. Há versões horizontal e vertical, cada uma colorida ou monocromática.

| Fundo | Versão do logo |
|---|---|
| Branco ou cinza-névoa | Colorida (degradê + nome em #333333) |
| Azul-marinho ou foto escura | Branca (negativa) |
| Foto clara ou movimentada | Grafite ou branca, sobre uma faixa sólida |

- Nunca use o logo em degradê sobre fundo escuro: o índigo some.
- Nunca estique, recolora ou aplique sombra.
- Capa e encerramento usam a versão vertical. O rodapé dos slides internos usa só o símbolo MS, com 32 px de altura.
- Deixe em volta do logo um respiro mínimo igual à altura do "M" do nome.
- Elemento gráfico da marca:
  - traço fino a 45°, em ciano ou branco, ao lado do número do capítulo;
  - símbolo grande em branco, com 8–10% de opacidade, como marca d'água sangrada na capa;
  - cantos arredondados com raio de 8 px em cards, barras e botões.
- Nenhum outro grafismo (ondas, pontos, formas orgânicas).

## 3. Paleta (só estas cores)

| Cor | Hex | Papel |
|---|---|---|
| Azul-marinho | #080D35 | Fundo de capas, aberturas e KPIs (~50% do deck) |
| Branco | #FFFFFF | Fundo de análise; texto e logo em fundo escuro (~25%) |
| Grafite do logo | #333333 | Texto principal em fundo claro (~8%) |
| Azul royal | #1646B8 | Série principal dos gráficos, ícones, controle selecionado em fundo claro (~8%) |
| Azul aço do logo | #055695 | Destaque em fundo claro: rótulos, dado-prova, links (~4%) |
| Ciano | #00C7E8 | Dado-prova, controle ativo e barra de progresso; SÓ em fundo marinho (~3%) |
| Dourado | #D5AC48 | Resultado final para o cliente, uma vez por slide, SÓ em fundo marinho (~2%) |
| Índigo do logo | #18073D | Só dentro do logo e da marca d'água |
| Cinza-contexto | #8A90A6 | Séries de comparação, notas, fontes, controles inativos |
| Cinza-névoa | #E9ECF3 | Cards, faixas e painéis de detalhe em slides claros |
| Marinho-card | #141B4D | Cards e painéis sobre fundo marinho |

Contraste: ciano e dourado sobre branco ficam em torno de 2:1, e essa combinação é proibida. Em fundo claro, o destaque é o azul aço ou o royal.

## 4. Tipografia (uma família: Montserrat)

Para um slide de 1920×1080:

- Capa: Bold, 72–88 px.
- Número-herói (KPI): Bold, 120–160 px.
- Título-conclusão: SemiBold, 36–44 px, no máximo 2 linhas.
- Rótulo de seção: Light, maiúsculas, espaçamento +15%, 14–16 px, em azul aço (fundo claro) ou ciano (fundo escuro). Exemplo: "02 · DIAGNÓSTICO".
- Subtítulo: Light, 24–28 px.
- Corpo: Regular, 20–22 px.
- Legenda de dado e texto de controles: Medium, 14–16 px.
- Fonte e notas: Regular, 12–13 px, em cinza-contexto.
- Números em tabelas e simuladores: `font-variant-numeric: tabular-nums`.

Se a fonte oficial do logo for outra (por exemplo, Gotham), troque só o nome da família e mantenha os pesos.

## 5. Composição e espaçamento

- Uma mensagem por slide. Se o título precisa de "e", são dois slides.
- No máximo 3 elementos principais por slide, com até 6 linhas e 40 palavras de corpo. O que passar disso vai para o painel "Ver detalhe".
- Grade de 12 colunas. O texto ocupa no máximo 7 colunas.
- Alinhamento à esquerda. Centralize só a capa e as frases de impacto.
- Unidade-base de 8 px. Margens: 96 px nas laterais, 80 px em cima e 64 px embaixo.
- Espaços: 16 px entre rótulo e título, 48 px entre título e conteúdo, 24 px entre cards, 32 px de padding interno.
- Pelo menos 30% de cada slide fica vazio.
- Rodapé dos slides internos: símbolo MS à esquerda, nome do cliente ao centro, número da página à direita.
- Alterne slides escuros (capa, aberturas, KPIs, próximos passos) com slides claros (análise). Não repita a mesma composição em slides seguidos.

## 6. Gráficos

- Tudo em cinza-contexto. Só a série que prova o título ganha cor: azul aço ou royal no fundo claro, ciano no fundo escuro.
- Sem grade de fundo, borda, 3D ou sombra. Escreva o valor direto na barra ou no fim da linha.
- Mostre a variação numa pílula sob o número: ▲ 12% em azul, ▼ 8% em cinza.
- Escolha o gráfico pelo formato do dado:
  - evolução no tempo: linha ou colunas;
  - antes × depois: duas colunas;
  - de onde vem a economia: cascata (waterfall);
  - composição: uma barra empilhada; nunca pizza com mais de 3 fatias.
- Todo gráfico tem dica ao passar o mouse e destaque de série ao clicar (seção 0.3 E).
- Todo gráfico leva rodapé no formato "Fonte: … · Base: … · Valores em R$ mil". Todo número tem unidade e período, no padrão brasileiro (R$ 1.234,56).

## 7. Diagramas

- Cascata: de onde vem a economia tributária (com revelação progressiva).
- Matriz 2×2: impacto × esforço (pontos clicáveis).
- Linha do tempo com marcos: cronograma de implantação (marcos clicáveis).
- Três pilares: resumo da proposta.
- Fluxo antes → depois: estrutura societária atual × proposta, como numa holding (alternância antes/depois e entidades clicáveis).
- Cards numerados: lista de oportunidades (expansíveis).

Desenhe todos em SVG ou HTML/CSS, nunca como imagem.

## 8. Fotografia

- Use arquitetura, paisagens calmas ou mãos trabalhando com documentos. Nada de aperto de mãos de banco de imagem.
- Tratamento frio e dessaturado. Use sobreposição marinho de 40–60% quando houver texto por cima.
- Foto sangrada só na capa e nas aberturas. Dentro do slide, recorte de 1/3 ou 1/2 da largura.
- Rostos reais só da equipe Moura & Siqueira, com autorização.
- Só use fotos fornecidas pelo usuário ou de licença livre. Sem foto adequada, use fundo marinho com a marca d'água do símbolo.

## 9. Tipos de slide

| Tipo | Fundo | Conteúdo | Interação típica |
|---|---|---|---|
| Capa | Marinho + foto ou marca d'água | Título, cliente, data, logo vertical branco | — |
| Agenda / sumário executivo | Branco | 3 a 4 conclusões com número | Itens clicáveis levam ao slide |
| Abertura de capítulo | Marinho | "02" grande + traço a 45° + nome | — |
| Número-herói | Marinho | Um KPI de 120–160 px em ciano ou dourado + frase | Contagem do número até o valor (400 ms) |
| Gráfico com conclusão | Branco | Título-conclusão + gráfico + fonte | Dica ao passar o mouse, destaque de série |
| Comparativo de cenários | Branco | 2 a 3 cenários, recomendado destacado | Alternância entre cenários |
| Simulador | Branco ou marinho | Controle + resultado + premissas | Controle deslizante recalcula |
| Painel de KPIs | Marinho | 4 a 6 números em grade | Clique abre o detalhe de cada KPI |
| Diagrama | Branco | Um modelo da seção 7 | Revelação ou elementos clicáveis |
| Case | Branco + foto | Ficha técnica, desafio, solução, resultado | Abas "Desafio / Solução / Resultado" |
| Próximos passos | Marinho | Linha do tempo + decisão pedida | Checklist clicável e botão de contato |
| Anexo | Branco | Memória de cálculo e premissas | — |

## 10. Proibido

- Título descritivo no lugar de título-conclusão.
- Esconder a conclusão atrás de um clique.
- Simulador com fórmula, alíquota ou premissa que não veio do usuário ou de documento do caso.
- Animações decorativas (giro, salto, zoom, voo de elementos) e transições acima de 400 ms.
- Ciano ou dourado sobre branco. Dourado como decoração.
- Logo em degradê sobre fundo escuro. Logo esticado, recolorido ou redesenhado.
- Gradiente no fundo inteiro, sombra projetada, gráfico 3D.
- Mais de duas cores de destaque no mesmo slide. Mais de uma família tipográfica.
- Clip-art, ícones de estilos misturados, fotos genéricas.
- Bloco longo de texto centralizado. Tabela com borda em todas as células.
- Número sem fonte, unidade ou período. Número inventado.
- Bibliotecas externas de JavaScript ou dependência de internet além da fonte.
- Copiar o layout literal de qualquer referência externa.

## 11. Checklist final (rodar antes de entregar)

- [ ] Roteiro de títulos e interações validado pelo usuário
- [ ] Os títulos, lidos em sequência, contam a história completa
- [ ] Uma mensagem por slide, no máximo 3 elementos principais
- [ ] Cada slide de análise tem pelo menos uma interação útil
- [ ] A conclusão aparece sem nenhum clique
- [ ] Navegação, menu, visão geral, modo apresentador e atalhos funcionando
- [ ] Todas as interações testadas no teclado, no mouse e no toque, sem erro no console
- [ ] Simuladores só com premissas confirmadas, exibidas no slide
- [ ] Só cores da paleta, um destaque por slide
- [ ] Ciano e dourado apenas sobre o marinho
- [ ] Logo original, na versão certa para cada fundo
- [ ] Só Montserrat em todo o deck
- [ ] Todo gráfico tem título-conclusão, série destacada e fonte
- [ ] Todo número tem unidade e período, no padrão brasileiro
- [ ] Margens de 96 px e pelo menos 30% de espaço vazio
- [ ] Rodapé com símbolo, cliente e página
- [ ] O último slide pede uma decisão
- [ ] Valores conferidos com a planilha de origem
- [ ] PDF com uma página por slide, interações no estado padrão, fundos e fonte corretos, nada cortado

Ao entregar, informe ao usuário:

- o caminho dos dois arquivos;
- os atalhos principais (setas, M, O, P, F, ?);
- um resumo de uma linha da narrativa;
- a lista de dados marcados como "a confirmar".