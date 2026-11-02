# ufal-tesis

Código-fonte LaTeX do Trabalho de Conclusão de Curso *"GraphRAG as a
multilabel classifier for liver diseases in electronic health records"*
(UFAL, ciência da computação).

- [`tesis/`](tesis/): o texto completo do TCC (introdução, trabalhos
  relacionados, fundamentação teórica, metodologia, resultados, conclusão).
- Os slides de defesa (LaTeX/Beamer) foram movidos para um repositório
  separado: [ufal-presentation-tesis](https://github.com/TETEURYAN/ufal-presentation-tesis)
  (histórico preservado via `git subtree split`).

## Compilação

```bash
cd tesis
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```
