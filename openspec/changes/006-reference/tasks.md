# Tasks

## 1. Bibliography Provenance & IEEE Format Verification

- [ ] 1.1 Cross-check all 9 `\bibitem` entries in `ICEIB paper/main.tex` (`lines 302–331`) against `docs/references.md` for exact authors, titles, venues, dates, and DOIs.
- [ ] 1.2 Run a bidirectional `\cite` vs. `\bibitem` check ensuring every `\cite` resolves and every `\bibitem` is cited in Sections I–V with IEEE punctuation and `\url`/`doi:` formatting.
- [ ] 1.3 Compile on Overleaf and inspect `read_log` for zero `Citation ... undefined` warnings while staying within the 4–6 page limit.
