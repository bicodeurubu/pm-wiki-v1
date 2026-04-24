# /wiki-refine

Conduz uma sessão de refinamento para preencher lacunas identificadas em páginas wiki. Transforma perguntas abertas (`refinement_questions`) em conteúdo concreto através de uma conversa estruturada com o PM.

**Agent:** librarian  
**Skills:** validate-frontmatter · detect-connections · propagate-changes

---

## Quando usar

- Quando uma página wiki foi gerada com `refinement_questions` preenchidas (campo no frontmatter)
- Quando o PM quer aprofundar uma página existente com `confidence: low` ou `uncertain`
- Quando o `wiki/index.md` lista pendências de refinamento e o PM quer resolvê-las

## Sintaxe

```
/wiki-refine                        → lista todas as páginas com refinement_questions pendentes
/wiki-refine [[nome-da-pagina]]     → inicia sessão de refinamento para uma página específica
```

---

## Comportamento sem argumento

Quando chamado sem argumento:

1. Escanear todos os arquivos em `wiki/` que tenham `refinement_questions` não vazio
2. Listar em ordem de prioridade:
   ```
   Páginas com refinamento pendente:

   🔴 Alta prioridade:
   - [[specs/exportar-pdf]] — 2 perguntas (spec sem métricas de sucesso, requisito ambíguo)

   🟡 Média prioridade:
   - [[users/persona-gestor]] — 1 pergunta (segmento de usuário indefinido)

   Qual página você quer refinar agora?
   ```
3. Aguardar escolha do PM ou aceitar `/wiki-refine [[pagina]]` como resposta

---

## Comportamento com argumento (sessão de refinamento)

### Passo 1 — Carregar a página

Ler o arquivo wiki da página alvo. Se não existir: informar o PM e encerrar.

### Passo 2 — Verificar refinement_questions

Se `refinement_questions` estiver vazio (`[]`):
```
A página [[nome-da-pagina]] não tem perguntas de refinamento pendentes.
Confidence atual: [valor]. Status: [valor].
Deseja fazer alguma alteração manual nesta página?
```
Encerrar o fluxo de refinamento.

### Passo 3 — Apresentar contexto antes de começar

```
Vou fazer [N] pergunta(s) sobre [[nome-da-pagina]].
Responda com o que souber — se não souber ainda, diga "não sei" e seguiremos em frente.

Começando com as de alta prioridade.
```

### Passo 4 — Conduzir as perguntas em sequência

Para cada item em `refinement_questions`, ordenado por `priority` (high → medium → low):

```
📋 Pergunta [N/Total]:

[question]

Contexto: [context]
```

Aguardar resposta do PM. Aceitar:
- Resposta direta → incorporar no conteúdo
- "não sei" / "ainda não definido" → manter a pergunta com nota; atualizar `priority` para `low`
- "pular" → mover para a próxima sem alterar

### Passo 5 — Reescrever as seções afetadas

Após todas as perguntas respondidas:

1. Identificar quais seções da página cada resposta afeta
2. Reescrever apenas essas seções — não alterar o restante da página
3. Mostrar o diff para o PM revisar antes de salvar:
   ```
   Aqui estão as alterações que vou fazer:

   **Seção: Success Metrics**
   Antes: [conteúdo anterior]
   Depois: [conteúdo novo]

   Confirma? (sim / ajustar)
   ```

### Passo 6 — Atualizar frontmatter

Após confirmação do PM:

- Remover perguntas respondidas de `refinement_questions`
- Se todas resolvidas: `refinement_questions: []`
- Se algumas pendentes ("não sei"): manter apenas as não resolvidas com `priority: low`
- Atualizar `confidence` baseado no novo estado:
  - Todas resolvidas + conteúdo completo → `medium` ou `high`
  - Algumas pendentes → `low`
- Atualizar `date_modified` para hoje
- Atualizar `ingest_state: processed`

### Passo 7 — Atualizar o Change Log

Adicionar entrada ao Change Log da página:

```markdown
| YYYY-MM-DD | Refinamento via /wiki-refine — [resumo do que foi adicionado] | PM session |
```

### Passo 8 — Propagação e log

1. Executar `propagate-changes` para notificar dependentes se conteúdo crítico mudou
2. Registrar no `wiki/log.md`:
   ```
   YYYY-MM-DD | wiki-refine | [[pagina]] | [N] perguntas resolvidas, [M] pendentes | confidence: [novo valor]
   ```
3. Git commit:
   ```
   git commit -m "feat(refine): resolve [N] refinement questions in [pagina]"
   ```

---

## Modo síncrono durante o ingest

O comando `/wiki-ingest` aceita a flag `--refine` para modo acompanhado:

```
/wiki-ingest raw/arquivo.pdf --refine
```

Com `--refine`:
- A AI processa o arquivo normalmente
- Antes de escrever a página, pausa e conduz as perguntas de `priority: high` em tempo real
- O PM responde na mesma sessão
- A página é escrita já com as respostas incorporadas
- Útil para arquivos estratégicos onde o PM sabe que quer estar presente

Sem `--refine` (comportamento padrão):
- A AI processa, escreve a página, e registra as perguntas no frontmatter para resolução posterior

---

## Output esperado

- Página wiki atualizada com conteúdo das respostas
- `refinement_questions` atualizado (vazio se tudo resolvido)
- `confidence` atualizado
- Change Log da página com nova entrada
- `wiki/log.md` com registro da sessão
- `wiki/index.md` atualizado (se TLDR mudou)
- Git commit com as mudanças
