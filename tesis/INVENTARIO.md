# Inventário — Fase 0 (refatoração visual do TCC)

## ⚠️ Divergência crítica encontrada — leia antes de tudo

O briefing desta tarefa descreve o trabalho como: **445 prontuários (141 com
ground truth anotado)**, **8 sistemas** avaliados com **Friedman, Wilcoxon
com correção de Bonferroni e bootstrap**.

O conteúdo já escrito nos capítulos (Cap03 e Cap04, revisados e corrigidos
nas últimas sessões de trabalho) descreve consistentemente: **537
prontuários** com rótulo de referência (531 para o sistema S8, nível 2 do
drift), **8 sistemas** (basic/local/global/drift × nível de comunidade 1/2,
com S5/S6 — global — não concluídos de forma confiável), avaliados com
**teste de McNemar exato** (comparação entre níveis de comunidade) e os
testes **U de Mann-Whitney** e **qui-quadrado** (investigação dirigida ao
comportamento do modo drift). Não há menção a Friedman, Wilcoxon+Bonferroni
ou bootstrap em nenhum capítulo atual — embora ambos os testes estejam
descritos na Fundamentação Teórica (Cap02, linha 245) como metodologia
**planejada**, e `friedman1937`/`wilcoxon1945` já existam no `.bib`.

**Hipótese mais provável, a confirmar com o autor:** o número 445/141 vem de
um estágio anterior do projeto — a pasta `figures/` (fora de `ufal-tesis/`)
documenta explicitamente duas famílias de experimento distintas: uma
comparação baseline-vs-prompt-tuning mais antiga (`fig1_*` a `fig6_venn`,
`fig_falsos_positivos_puros`, `fig_ground_truth`, `fig_matriz_confusao_tuned`,
`fig_metricas_por_classe`, datadas de 24/08) e o grid de 8 sistemas mais
recente (`fig_grid_metricas`, `fig_heatmap_acertos`). A amostra de 537
prontuários e o teste de McNemar são posteriores a essa primeira fase.

**Não resolvi essa divergência.** Per a regra de ouro desta tarefa, sinalizo
e aguardo confirmação antes de tratar qualquer um dos dois conjuntos de
números como correto. Nenhuma figura ou tabela baseada em 445/141,
Friedman ou Wilcoxon deve ser criada até esse ponto ser esclarecido.

---

## 1. Estrutura do documento (.tex / .bib / .cls / .sty)

| Arquivo | Linhas | Papel |
|---|---|---|
| `main.tex` | 151 | Preâmbulo, pacotes (já inclui `tikz`, `pgfplots` compat=1.17, `booktabs`, `float`, `subfig`, `svg`, `hyperref`, `natbib`, `easyReview`), `\include`s |
| `JITH.cls` | — | Classe do template (não listada para alteração — regra 8 proíbe trocar) |
| `macros.tex` | 85 | Macros matemáticas (robótica/controle, não usadas nos capítulos atuais) |
| `Identificacao.tex` | — | Folha de identificação |
| `Cap00/*.tex` | 18+8+21+8+5+6 | Abreviaturas, Abstract, Agradecimentos, Epígrafe, Resumo, Símbolos |
| `Cap01/Capitulo1.tex` | 43 | Introdução |
| `Cap02/Capitulo2.tex` | 7 | Stub/wrapper que inclui `FundamentacaoTeorica.tex` |
| `Cap02/FundamentacaoTeorica.tex` | 281 | Fundamentação teórica — LLM, RAG, GraphRAG, Leiden, "lost in the middle", métricas, testes estatísticos, trabalhos relacionados (já cita baptista2025/2026, kare2025, rakuljic2026, esteves2025 em prosa) |
| `Cap03/Capitulo3.tex` | 360 | Metodologia — já tem 1 figura TikZ (fluxograma `fig:met-visao-geral`), 3 tabelas `booktabs`, 3 equações |
| `Cap04/Capitulo4.tex` | 222 | Resultados — já tem 1 gráfico `pgfplots` (barras agrupadas), 1 gráfico `pgfplots` (F1 por doença), 3 tabelas `booktabs`, 5 `\includegraphics` (matrizes de confusão + grafos) |
| `Conc/Conclusao.tex` | **0** | **Vazio** |
| `Ref/SampleReferences.bib` | 391 | 30 entradas, inclui todas as citações de trabalhos relacionados do briefing |

`Cap02`, `Cap03` e `Cap04` já estão em boa parte do "Fase 2" do briefing:
contêm TikZ/pgfplots nativos, não apenas prosa. A tarefa de refatoração
visual é mais um complemento/polimento do que uma criação do zero.

## 2. Figuras existentes

### 2.1. Já incluídas e referenciadas no `.tex` (nenhuma pendência)

Todas resolvidas — nenhum `\includegraphics` aponta para arquivo ausente.

| Arquivo | Usado em | Formato | Observação |
|---|---|---|---|
| `Cap02/figuras/Level0Multihop.jpg`, `Level1Multihop.jpg` | Cap02 (multihop) | JPG raster | Ilustrativo genérico, não gerado por este projeto |
| `Cap04/figuras/matriz_confusao_S7.png`, `matriz_confusao_S8.png` | Cap04 | PNG raster (matplotlib) | Matrizes de confusão por doença, sistemas drift |
| `Cap04/figuras/subgrafo_paciente_1800283_comunidades_457_460.png` | Cap04 | PNG raster (NetworkX+matplotlib) | Caso de fragmentação topológica entre comunidades |
| `Cap04/figuras/comparacao_maiores_grafos.png` | Cap04 | PNG raster | 6 maiores subgrafos da amostra |
| `Cap04/figuras/analise_estrutural_subgrafos.png` | Cap04 | PNG raster | Histograma agregado (entidades/relações/comunidades) |

### 2.2. Existentes no repositório, NÃO referenciadas em `ufal-tesis/` (candidatas, uso condicionado à divergência da seção 0)

| Arquivo | Pasta | Possível capítulo | Observação |
|---|---|---|---|
| `fig_grid_metricas.png`, `fig_heatmap_acertos.png` | `figures/`, `resultados_grid/`, `resultados_grid_corrigido_nenhuma/` (3 cópias quase idênticas, datas 14–24/08) | Cap04 | **Superseded**: o Cap04 já tem gráfico `pgfplots` nativo com os números finais verificados (537 amostras); estas rasters são de execuções anteriores — não reutilizar sem conferir se os números batem |
| `fig1_estatisticas_gerais.png` … `fig6_venn.png`, `fig_falsos_positivos_puros.png`, `fig_ground_truth.png`, `fig_matriz_confusao_tuned.png`, `fig_metricas_por_classe.png` | `figures/` | Possivelmente Cap03 (funil de amostra) ou um apêndice | Da família de experimento `prompt_tuning_comparison`, **provável origem do número 445/141** da seção 0 — não usar até a divergência ser resolvida |
| `grafico_acertos_erros_drift.png` | `tcc-results-01/` | Cap04 | Gerado nesta sessão (scatter tamanho de texto × acerto/erro do drift); candidato a figura nativa (seria melhor refeito em `pgfplots` a partir do `.csv`, não como raster) |
| `subgrafo_paciente_1800283.png` | `tcc-results-01/` | Cap04 | Versão sem destaque de comunidade do mesmo caso já incluído — redundante com a versão já usada |
| `grafos_por_prontuario/` (537 arquivos) | `tcc-results-01/` | — | Não usar individualmente; é o material-fonte de onde `comparacao_maiores_grafos.png` foi derivado |

## 3. Tabelas `.csv`

| Arquivo | Pasta | Conteúdo | Já usado no Cap04? |
|---|---|---|---|
| `metricas_parcial_sem_global.csv` | `tcc-results-01/` | Métricas dos 6 sistemas finais (537 amostras) | **Sim** — fonte confirmada da Tabela de métricas agregadas e dos gráficos `pgfplots` atuais |
| `metricas_grid_multilabel.csv` | `tcc-results-01/` | Execução antiga (modelo anterior, números bem mais baixos) | Não — não usar, é de uma rodada diferente/anterior |
| `metricas_ling30_s1s6.csv` | `tcc-results-01/` | Tentativa parcial com modelo novo (só S1/S6, S6 vazio) | Não — mencionada só textualmente no Cap03 como tentativa incompleta |
| `stats_subgrafos_por_prontuario.csv` | `tcc-results-01/` | Entidades/relações/comunidades por paciente | Sim, indiretamente (fonte de `analise_estrutural_subgrafos.png`) |
| `wilcoxon_posthoc.csv`, `pivot_sistemas.csv`, `analise_concordancia.csv`, `resultados_consolidados.csv`, `metricas_grid.csv` | `resultados_grid/`, `resultados_grid_corrigido_nenhuma/` | Testes post-hoc de Wilcoxon já calculados numa rodada anterior | **Atenção**: confirma que o teste de Wilcoxon foi de fato executado em algum momento — possivelmente a mesma rodada de origem do 445/141. Não usar sem reconciliar com a seção 0 |
| `matriz_confusao_tuned.csv`, `metricas_por_classe_tuned.csv` | `figures/`, `resultados_avaliacao/` | Baseline vs. prompt-tuning | Mesma família de origem do possível 445/141 |

## 4. Relatórios `.md` / `.txt`

| Arquivo | Pasta | Conteúdo |
|---|---|---|
| `metodologia_completa.txt` | `tcc-results-01/` e `ufal-tesis/tesis/` (cópia) | Prosa consolidada que serviu de base para Cap03 — já incorporada |
| `relatorio_avaliacao.txt` | `resultados_avaliacao/` | Relatório da rodada antiga (mesma família do 445/141) |
| `analise_resultados_grid.md`, `analise_grid_sem_prontuarios_vazios.md` | `docs/` | Análises de apoio já usadas nas sessões anteriores de escrita |
| `diagnostico_falha_drift.txt` | `docs/` | Notas da investigação de infraestrutura do modo drift (não é conteúdo de capítulo) |
| `metodologia.md`, `arquitetura.md`, `guia-graphrag.md`, `arquivos_parquet.md` | `docs/` | Documentação técnica do repositório, não do TCC |

## 5. Scripts e notebooks (apenas leitura, não alterar)

Scripts relevantes já usados para gerar as figuras/tabelas atuais do Cap04:
`experiments/grid_search/gera_todos_subgrafos.py`,
`experiments/grid_search/plota_subgrafo_paciente.py`,
`experiments/grid_search/plota_acertos_erros_drift.py`,
`experiments/grid_search/testa_hipoteses_drift.py`,
`experiments/grid_search/testa_hipoteses_drift_erro.py`. Não serão
modificados nesta tarefa.

## 6. Checagem de `\includegraphics`

- Referenciado no `.tex` e ausente no disco: **nenhum caso encontrado.**
- Presente no disco e não referenciado: listado na seção 2.2.

## 6.1. Resolução da divergência (confirmada pelo autor)

- **Amostra:** 537 prontuários (não 445/141) — confirmado.
- **Testes de hipótese:** Friedman, Wilcoxon+Bonferroni e bootstrap **não
  devem ser usados** — confirmado. O Cap02 (linha 245) ainda descreve esses
  testes como metodologia planejada; como o Cap03/Cap04 usam McNemar +
  Mann-Whitney + qui-quadrado, esse trecho do Cap02 ficou desalinhado com o
  método efetivamente aplicado. É uma correção de texto, fora do escopo
  visual desta tarefa, mas registro aqui para o autor avaliar à parte.
- **Rótulo único vs. multirrótulo (nova divergência, não perguntada antes):**
  Cap02 linha 279 descreve a tarefa como classificação em **rótulo único**
  e chega a justificar essa escolha pela estrutura causal entre as doenças.
  Cap03/Cap04 descrevem e usam **classificação multirrótulo** (ver Tabela de
  distribuição de classes, "1,49 doenças confirmadas por paciente em
  média"). O diagrama de "hierarquia causal para justificar o rótulo único",
  sugerido no briefing, **não se aplica** nesse cenário — vou tratá-lo como
  não aplicável no plano abaixo, mas o texto do Cap02 permanece
  factualmente inconsistente com o resto do trabalho até alguém revisá-lo.

## 7. Próximo passo

Aguardando confirmação sobre a divergência da seção 0 antes de montar o
`PLANO_REFATORACAO.md` (Fase 1) — em particular: (a) qual é o número real
de prontuários/ground truth a reportar na Introdução e no Abstract, que
ainda não mencionam amostra; (b) se Friedman/Wilcoxon devem ser citados
como método efetivamente aplicado ou removidos do Cap02 por não terem sido
usados na versão final; (c) se as figuras da família `prompt_tuning_comparison`
(445/141) pertencem a um apêndice de validação preliminar ou devem ser
descartadas do trabalho final.
