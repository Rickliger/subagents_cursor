# Spec 0001 — Exemplo de feature (template)

Status: **proposto**
Escopo: `internal/`, `frontend/src/` *(ajuste conforme o projeto)*
Relacionado: *(specs, ADRs ou issues relacionadas — ou "nenhum")*

---

## 1. Contexto / problema

Descreva o problema de negócio ou técnico que motiva esta spec. Exemplo: usuários não conseguem filtrar registros por data; o fluxo atual exige exportar para planilha.

---

## 2. Estado atual

Como o sistema se comporta hoje. Inclua endpoints, telas ou trechos relevantes sem copiar código inteiro — basta apontar paths e comportamento observado.

---

## 3. Proposta

Solução desejada em linguagem clara. Pode incluir diagrama ou fluxo se ajudar.

```
Exemplo de fluxo:
  Cliente → API → Service → Repository → resposta filtrada
```

---

## 4. Mudanças por área

| Área | Mudança |
|------|---------|
| Backend | Novo query param `?from=` e `?to=` no endpoint de listagem |
| Frontend | Controles de intervalo de datas na tela de listagem |
| Banco | Índice em `created_at` se necessário |

---

## 5. Critérios de aceite

- [ ] Filtro por intervalo de datas funciona na API
- [ ] UI reflete o filtro e atualiza a listagem
- [ ] Intervalo inválido retorna erro claro (400)
- [ ] Documentação ou INDEX atualizados ao concluir

---

## 6. Fora de escopo

- Exportação CSV
- Filtros avançados (tags, categorias)
- Notificações por e-mail

---

*Este arquivo é um **exemplo de estrutura**. Ao iniciar o primeiro item real, renomeie ou substitua por `0001-seu-assunto.md` e atualize [`../00-INDEX.md`](../00-INDEX.md).*
