# Plano de refatoração visual — Fase 1

Premissa confirmada pelo autor: 537 prontuários, sem Friedman/Wilcoxon/
bootstrap (ver `INVENTARIO.md`, seção 6.1). O documento já está mais maduro
visualmente do que o briefing original supunha: Cap02, Cap03 e Cap04 já têm
TikZ/pgfplots nativos, não apenas prosa. Este plano foca no que falta ou
pode ser melhorado, não recria o que já existe.

## Cap00 — Elementos pré-textuais

Nenhuma intervenção. Abreviaturas, Abstract, Agradecimentos, Epígrafe,
Resumo e Símbolos não comportam elementos visuais por convenção ABNT.

## Cap01 — Introdução (43 linhas)

**Desnecessária.** Capítulo curto, só prosa introdutória e lista de
objetivos. Um diagrama aqui seria redundante com o fluxograma de visão
geral que já existe no Cap03 (`fig:met-visao-geral`). Não recomendo nada.

Observação fora do escopo visual: os objetivos específicos ainda citam
"modelagem temporal", "TF-IDF e t-SNE" e "reconhecimento de entidades
nomeadas" para anonimização — nenhum desses aparece como entregue no
Cap03/Cap04. É uma possível defasagem de texto a revisar com o orientador,
não uma questão de diagramação.

## Cap02 — Fundamentação Teórica (281 linhas)

Já contém 4 figuras nativas de boa qualidade: pipeline do GraphRAG (TikZ),
hierarquia de Leiden (raster, extraída de Edge et al. com citação correta),
modos de busca (TikZ) e "lost in the middle" (pgfplots). Avaliação por
oportunidade:

| Intervenção sugerida pelo briefing | Classificação | Motivo |
|---|---|---|
| Diagrama do pipeline GraphRAG | **Já existe** | `fig:graphrag-pipeline`, TikZ |
| Hierarquia de community-levels | **Já existe** | `fig:leiden-hierarquia` (raster didático) — a tabela numérica real (46/501/.../7 comunidades) já está no Cap03, Tabela `tab:met-niveis`, não duplicar aqui |
| Diagrama dos modos de busca | **Já existe** | `fig:modos-busca`, TikZ |
| RAG vetorial × GraphRAG lado a lado | Recomendável | Não existe; poderia reforçar a Seção 2.x que descreve RAG tradicional, mas o texto já é claro em prosa — risco de redundância. Proponho como item opcional, baixa prioridade |
| Hierarquia causal das doenças (p/ justificar rótulo único) | **Não aplicável** | O projeto final é multirrótulo, não rótulo único (ver INVENTARIO 6.1) — este diagrama justificaria uma decisão que não é a adotada. Não implementar até o texto da linha 279 ser corrigido |
| Caixas de destaque (`tcolorbox`) para definições-chave | Recomendável | Termos como "community level" e "lost in the middle" aparecem só em prosa corrida; uma caixa de definição no primeiro uso ajudaria a leitura, sem alterar o conteúdo |
| Tabela comparativa de trabalhos relacionados (Baptista, Esteves, Rakuljic, KARE) | Recomendável | Os 4 trabalhos já são discutidos em prosa (linhas 269–275), bem escritos e específicos; uma tabela-síntese adicional é mais ganho de legibilidade que de conteúdo novo. Prioridade baixa a média |

## Cap03 — Metodologia (360 linhas)

Já tem o fluxograma geral (`fig:met-visao-geral`, TikZ) e 3 tabelas
`booktabs` (distribuição de classes, sistemas avaliados, níveis de
comunidade). Oportunidades:

| Intervenção | Classificação | Motivo |
|---|---|---|
| Equação do teste de McNemar exato | **Já existe** | `eq:mcnemar`, Seção 3.6 |
| Tabela da evolução do prompt (3 versões) | **Já existe** | `tab:met-prompts` |
| Diagrama da estrutura dos 3 ajustes de infraestrutura (JSON forçado, reasoning_effort, paralelização) | Recomendável | Hoje é só prosa (Seção "Modelo de linguagem e infraestrutura"); um pequeno diagrama de blocos poderia ajudar, mas o texto já é claro e linear — prioridade baixa |
| Caixa de destaque para a ressalva de PBE (2 casos positivos) | Recomendável | É um aviso metodológico importante hoje "escondido" em parágrafo corrido; uma `tcolorbox` de alerta aumentaria a visibilidade |

## Cap04 — Resultados (222 linhas)

Já tem 2 gráficos `pgfplots` nativos (barras agrupadas de métricas, F1 por
doença), 3 tabelas `booktabs` e 5 `\includegraphics` (2 matrizes de
confusão, 3 figuras de grafo). É o capítulo com mais espaço pra melhoria,
mas a maior parte do essencial já está coberta.

| Intervenção | Classificação | Motivo |
|---|---|---|
| Tabela de McNemar (basic/local/drift) | **Já existe** | `tab:res-mcnemar` |
| Tabelas H1–H4 das duas investigações estatísticas | **Já existem** | `tab:res-h1-h4-acerto`, `tab:res-h1-h4-erro` |
| Reconstruir as matrizes de confusão (`matriz_confusao_S7/S8.png`) como `pgfplots` matrix plot nativo em vez de raster matplotlib | Recomendável | Ganha consistência tipográfica com o resto do documento (mesma fonte, mesma paleta); risco: 8 matrizes 2×2 por sistema exigem um layout cuidadoso em TikZ, mais trabalho que o raster atual, que já é legível. Prioridade média |
| Gráfico de dispersão acerto/erro do drift por tamanho de prontuário (`grafico_acertos_erros_drift.png`, gerado nesta sessão) | Recomendável | Reforça visualmente o achado central de H1/H2 (comprimento do texto como fator); hoje só está descrito em números na prosa. Como é 1 ponto por par paciente-doença (milhares de pontos), fica mais prático como raster matplotlib bem ajustado do que recriado em `pgfplots` — proponho inserir a versão já gerada, com pequenos ajustes de estilo (fonte, cores) pra combinar com o resto do documento, em vez de uma recriação nativa cara e com baixo retorno |
| Exemplos qualitativos (592097 alucinação, 890194 negação ignorada, 1028182 confusão familiar) | Recomendável | Hoje citados só em texto corrido; um quadro/`tcolorbox` com 1–2 excertos verbatim do prontuário ao lado da resposta do drift tornaria o mecanismo de erro mais concreto para o leitor. Os `.txt` já exportados em `tcc-results-01/casos_falso_positivo_binarias/` e `casos_acerto_inferencia/` são a fonte pronta pra isso |
| Heatmap sistema × doença (F1 de todas as 7 classes, todos os 6 sistemas, não só os 3 já plotados) | Recomendável | A Figura `fig:res-f1-doenca` atual só compara S1/S3/S7; um heatmap completo (pgfplots matrix plot, 6×7) daria panorama mais completo, mas aumenta a densidade de informação — avaliar se cabe sem sobrecarregar o capítulo |

## Conc — Conclusão (0 linhas)

Fora do escopo desta tarefa (conteúdo textual, não visual) — não produzo
texto de conclusão. Registro apenas que o capítulo está vazio, pois
qualquer figura de "síntese visual dos achados" (ex.: um resumo gráfico
final) só faz sentido depois que o texto existir.

## Resumo de prioridades

**Essencial, ainda pendente:** nenhum item — o essencial já foi implementado
nas sessões anteriores (pipeline, modos de busca, Leiden, lost-in-the-middle,
fluxograma de metodologia, tabelas de métricas e de testes estatísticos,
matrizes de confusão, figuras de grafo).

**Recomendável, por ordem de proposta de execução:**
1. Inserir `grafico_acertos_erros_drift.png` no Cap04 com ajuste de estilo.
2. Quadro(s) com excertos verbatim dos casos qualitativos (592097, 890194, 1028182) no Cap04.
3. Caixa de alerta sobre PBE (Cap03) e caixas de definição (Cap02).
4. Tabela-síntese de trabalhos relacionados (Cap02).
5. Heatmap completo sistema × doença (Cap04) — avaliar se não satura o capítulo.
6. Matrizes de confusão nativas em `pgfplots` (Cap04) — maior esforço, menor urgência.
7. Diagrama RAG vetorial × GraphRAG (Cap02) e diagrama de infraestrutura (Cap03) — baixa prioridade.

**Não implementar:** hierarquia causal/rótulo único (Cap02), por
incompatibilidade com o método final multirrótulo.

**Fora do escopo visual, sinalizado para revisão textual à parte:**
Friedman/Wilcoxon no Cap02 (linha 245), objetivos específicos do Cap01
desatualizados, inconsistência rótulo único vs. multirrótulo (Cap02, linha
279), Conclusão vazia.

---

Aguardando aprovação para iniciar a Fase 2, começando pelos itens 1 e 2 do
Cap04 (maior retorno, já com material pronto em `tcc-results-01/`).
