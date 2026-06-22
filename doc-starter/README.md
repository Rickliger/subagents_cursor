# Documentação de planejamento

Este diretório é a **linha central** de planejamento e implementação deste projeto.

| Pasta | Conteúdo |
|-------|----------|
| [`00-INDEX.md`](00-INDEX.md) | **Comece aqui** — vitrine de specs, backlog (Implementados/Propostos) e épicos |
| [`backlog/`](backlog/) | Fixes pequenos — **B001, B002, …** (1 arquivo por item) |
| [`specs/`](specs/) | Specs numeradas (0001, 0002, …) |
| [`epics/`](epics/) | Análises grandes multi-doc (migrations, refactors longos) |
| [`adr/`](adr/) | Architecture Decision Records |

Runbooks operacionais (deploy, sandbox, procedimentos): [`../docs/`](../docs/) na raiz do projeto — **separado** de `doc/`.

---

## Bootstrap em projeto novo

Ao iniciar um repositório, copie esta pasta inteira para a raiz do projeto como `doc/`:

```bash
cp -r doc-starter/ /caminho/do/projeto/doc/
```

Em seguida, abra [`00-INDEX.md`](00-INDEX.md) e ajuste o título do projeto. Remova ou renomeie [`specs/0001-example-feature.md`](specs/0001-example-feature.md) quando registrar a primeira spec real.

### Backlog e fechamento de itens

- O backlog no INDEX usa subseções **Implementados** e **Propostos** (não coluna Status na tabela).
- Ao concluir um item: marcar aceite `[x]` no arquivo, mover a linha para **Implementados** e preferir **verifier** antes de marcar `implementado` — quem implementou não fecha sozinho.
