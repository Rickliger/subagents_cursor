# Índice central — este projeto

**Fonte única de verdade** para specs, backlog e épicos. Humanos e agents (`.cursor/`) consultam **antes** de planejar ou implementar.

## Como usar

| Tipo | Onde | ID |
|------|------|-----|
| Fix pequeno / bug | `doc/backlog/BNNN-slug.md` | B001, B002, … — **1 arquivo por item** |
| Feature / integração | `doc/specs/NNNN-slug.md` | 0001, 0002, … |
| Épico grande | `doc/epics/<nome>/` | E001, … |

1. **Status** no topo de **cada** arquivo (`Status: proposto | em andamento | implementado | …`).
2. **INDEX** — vitrine: specs/épicos usam coluna Status; backlog usa subseções **Implementados** / **Propostos** (abaixo).
3. **Em andamento** — no máximo 1–3 itens na tabela § "Em andamento agora".
4. **Fechar item** — critérios `[x]` no arquivo → mover backlog para Implementados **ou** status `implementado` na spec → preferir **verifier** (ou revisão doc+código) antes de marcar; auto-declaração do agent que implementou **não basta** sozinha.

### Chat zerado — frases úteis

| Objetivo | Pedido |
|----------|--------|
| Panorama | *"Lê este INDEX e resume o que está proposto, parcial e implementado."* |
| Auditoria | *"Revisa o que falta: INDEX + arquivos + código; não implementa."* |
| Implementar | *"Implementa Backlog B001"* ou *"Implementa Spec 0001"* |

---

## Em andamento agora

| ID | Título | Nota |
|----|--------|------|
| — | *(vazio)* | Marque aqui ao iniciar B/Spec |

---

## Backlog (fixes pequenos)

Arquivos em [`backlog/`](backlog/). Cada item: `Status:` no arquivo + aceite `[ ]` / `[x]`.

### Implementados

*(Aceite `[x]`; conferidos no código — mover para cá após verifier ou OK explícito.)*

| ID | Título | Escopo |
|----|--------|--------|
| — | *(nenhum item implementado ainda)* | — |

### Propostos

| ID | Título | Escopo |
|----|--------|--------|
| — | *(nenhum item registrado)* | — |

Próximo ID livre: **B001**.

---

## Vocabulário de status

| Status | Significado |
|--------|-------------|
| **proposto** | Registrado; ninguém implementando |
| **em andamento** | Trabalho ativo (branch, PR, sessão) |
| **implementado** | Aceite atendido; conferido (verifier ou revisão doc+código) |
| **parcial** | Parte entregue; checklist ainda aberto |
| **adiado** | Válido, sem prioridade |
| **cancelado** | Não será feito (motivo no arquivo) |

---

## Registro de specs

| ID | Título | Status | Escopo | Notas |
|----|--------|--------|--------|-------|
| — | *(nenhuma spec registrada além do exemplo)* | — | — | — |

Próximo spec: **0001** *(exemplo em [`specs/0001-example-feature.md`](specs/0001-example-feature.md) — substitua ao iniciar o primeiro item real)*.

---

## Registro de épicos

| ID | Título | Status | Escopo | Índice |
|----|--------|--------|--------|--------|
| — | *(nenhum épico registrado)* | — | — | — |

---

## Template para nova spec

Copie para `doc/specs/NNNN-meu-assunto.md`:

```markdown
# Spec NNNN — Título curto

Status: proposto
Escopo: pastas/pacotes afetados
Relacionado: specs, ADRs, issues

## 1. Contexto / problema

## 2. Estado atual

## 3. Proposta

## 4. Mudanças por área

## 5. Critérios de aceite
- [ ] ...

## 6. Fora de escopo
```

---

## Tipos de documento (quando usar o quê)

| Tipo | Onde | Quando |
|------|------|--------|
| **Backlog** | `doc/backlog/BNNN-*.md` | Bug ou fix pequeno — **1 arquivo por item** |
| **Spec** | `doc/specs/` | Feature ou integração grande |
| **Epic** | `doc/epics/<nome>/` | Análise multi-doc, migration grande (semanas) |
| **ADR** | `doc/adr/` | Decisão irreversível (“por que Y e não Z”) |
| **Runbook** | `docs/` na raiz | Deploy, sandbox, operação |
| **Review** (arquivo) | `docs/reviews/` ou pasta equivalente | Investigação pontual / legado — **não** substitui spec |

---

## Integração com agents e git

### Cursor (`.cursor/`)

- **Arquiteto** (`rules/00-architect.mdc`): INDEX → classifica **B** vs **000N** → delega com ID + aceite.
- **Subagents**: prompt com `Backlog B001` ou `Spec 0001` + paths.
- **TEAM_STRUCTURE.md**: referência de papéis e skills em `.cursor/`.

### Git

- PR/commit menciona `Spec 0001` ou `Backlog B001`.
- **Fechar:** `[x]` no arquivo + INDEX; backlog implementado → subseção **Implementados**.
- Preferir **verifier** (ou revisão doc+código) antes de marcar `implementado`; quem implementou não fecha sozinho.

**Não duplicar:** uma spec = um arquivo; status vive no **INDEX + campo Status no topo** da spec (INDEX é a vitrine).
