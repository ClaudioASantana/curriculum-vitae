# Wiki de carreira — regras de operação

Este repo contém o currículo (`cv.md`, `cv.pdf`), documentos de projetos (`docs/`) e uma **wiki mantida pelo LLM**
a partir de descrições de vagas. Idioma: português do Brasil.

## Camadas

| Camada | Pasta | Quem escreve |
|---|---|---|
| Fontes brutas | `vagas/raw/` | Usuário (cola a vaga). **O LLM nunca edita.** |
| Wiki | `wiki/` | LLM |
| Verdade sobre o usuário | `cv.md`, `docs/` | Usuário (o LLM só propõe mudanças em `cv.md`) |

## Regra de ouro (honestidade)

Só entra no CV, em post ou nas "minhas histórias" do preparo algo que esteja em `cv.md`, em `docs/` ou que o
usuário tenha contado na conversa (e que foi registrado em `wiki/evidencias.md` com a fonte).
**Nunca inventar experiência, tecnologia, métrica ou período.**
Se uma vaga pede algo sem evidência, o LLM **pergunta ao usuário**; se ele não tiver, vira item em
`wiki/gaps.md`, nunca linha no CV. Em caso de dúvida, perguntar.

## Operações

### Ingest — "processar vaga X" / "ingest"
1. Ler o arquivo em `vagas/raw/`. Se faltar front matter, perguntar empresa, data e link.
2. Extrair: requisitos obrigatórios, desejáveis, senioridade, stack, modelo (remoto/híbrido), faixa salarial se houver.
3. Atualizar, nesta ordem:
   - `wiki/skills/<skill>.md`: criar se não existir (usar `wiki/_templates/skill.md`), incrementar contagem, listar a vaga.
   - `wiki/mercado.md`: recalcular padrões (top skills por frequência, senioridade, modelos).
   - `wiki/gaps.md`: cruzar com `cv.md`, `docs/` e `wiki/evidencias.md`.
   - `wiki/index.md`: incluir a vaga e novas páginas.
4. Marcar `processada: true` no front matter da vaga e acrescentar linha em `wiki/log.md`.
5. Responder com resumo: o que mudou, novos gaps, contradições encontradas. Oferecer gerar o preparo da vaga.

### Preparo — "preparar para a vaga X"
Objetivo: o usuário tem experiência, mas dificuldade de articulá-la. O preparo ensina a **falar** do tema, não só a entendê-lo.
1. Gerar `wiki/preparo/<vaga>.md` com o template `wiki/_templates/preparo.md`.
2. Para cada requisito relevante da vaga:
   - **Conceito:** explicação curta (3 a 6 linhas), em linguagem falável. O conceito detalhado fica em `wiki/skills/<skill>.md` e é só referenciado.
   - **Perguntas prováveis** de entrevista (técnicas e comportamentais), incluindo um follow-up difícil.
   - **Minha história:** procurar em `cv.md` e `docs/` o que o usuário já fez com o tema e montar situação, ação e resultado. Se não houver material suficiente ou faltar resultado/métrica, **perguntar ao usuário** em vez de preencher. Histórias confirmadas vão para `wiki/evidencias.md` com a fonte.
   - **Domínio:** campo que o usuário preenche (1 a 5). Abaixo de 3, listar o que estudar e levar o item a `wiki/estudo.md`.
3. Apontar quando o usuário minimiza algo que é valor real (sustentar sistema crítico, migrar legado, mentoria) e sugerir um enquadramento mais forte, sem exagerar os fatos.
4. Fechar com um pitch de apresentação de 60 a 90 segundos ajustado à vaga, baseado só em fatos confirmados.

### Simulado — "simular entrevista da vaga X"
1. Ler o preparo da vaga. Atuar como entrevistador, **uma pergunta por vez**, começando pelos requisitos com domínio baixo ou obrigatórios.
2. O usuário responde (escrito ou ditado). Dar feedback curto: o que ficou claro, o que ficou vago, se faltou resultado concreto, se passou de ~90 s. Fazer um follow-up como um entrevistador real faria.
3. Oferecer reescrever a resposta em versão melhor **usando apenas fatos que o usuário disse**.
4. Ao final, registrar em `wiki/preparo/<vaga>.md` a seção "Pontos a reforçar" e atualizar `wiki/estudo.md`.
Não transformar respostas em roteiro decorado: treinar estrutura e fatos, não texto palavra por palavra.

### Query — perguntas sobre o mercado ou o CV
Ler `wiki/index.md` primeiro, depois as páginas relevantes. Citar as vagas de origem. Se o resultado for útil e
durável, oferecer salvá-lo como página da wiki.

### CV — "atualizar CV"
Comparar `cv.md` com `wiki/evidencias.md` e `wiki/mercado.md`. Propor **diff** (não sobrescrever sem aprovação):
reordenar por relevância, ajustar palavras-chave do mercado que o usuário realmente domina, cortar ruído.
Variante por vaga: gerar em `wiki/cv-variantes/<empresa>.md`, sem tocar no `cv.md`.

### Estudo — "plano de estudo"
Atualizar `wiki/estudo.md`: priorizar gaps por (frequência nas vagas) × (esforço estimado) e por quanto
destravam vagas. Sugerir um entregável prático para cada item (projeto pequeno que vira evidência).

### Posts — "ideias de post"
Gerar em `wiki/posts/AAAA-MM-DD-tema.md`: ângulo, rascunho, e a evidência que o sustenta.
Tom: primeira pessoa, concreto, sem clichê de LinkedIn. O usuário revisa e publica.

### Lint — "lint"
Reportar sem corrigir às cegas:
- vagas em `raw/` com `processada: false`;
- skills no `cv.md` sem entrada em `evidencias.md` (informativo; a página é preenchida pelos preparos);
- vagas processadas sem arquivo em `wiki/preparo/`;
- requisitos de preparo com domínio < 3 e sem item em `estudo.md`;
- páginas de skill sem vaga associada ou com contagem desatualizada;
- gaps que já têm evidência;
- afirmações de mercado baseadas em menos de 10 vagas (marcar como "amostra pequena");
- vagas com mais de 6 meses (dados velhos).

## Convenções
- Nomes de arquivo em minúsculas com hífen: `wiki/skills/postgresql.md`, `vagas/raw/2026-10-empresa-cargo.md`.
- Links internos no formato `[[nome-da-pagina]]` (compatível com Obsidian).
- Toda página da wiki começa com front matter (`atualizado: AAAA-MM-DD`).
- Nunca apagar vagas; marcar como `status: encerrada` se for o caso.
- Commits em português, mensagem curta no imperativo/descritiva, como o histórico existente.
