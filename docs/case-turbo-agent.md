Listed directory src

Excelente! Olhando para os nossos projetos atuais, o **Turbo-Agent** (e sua integração com a extensão do VS Code) é um prato cheio para problemas complexos de arquitetura. 

Vou estruturar um "case" real e altamente técnico baseado no que temos no repositório do `turbo-agent`. Esse cenário aborda concorrência, arquitetura de sistemas assíncronos e integração com IA, o que impressiona bastante em entrevistas.

Aqui está o roteiro da resposta:

---

### O Contexto e o Problema (O que era)
"No desenvolvimento do **Turbo-Agent**, que é um agente de IA local integrado ao VS Code, tínhamos um requisito crítico: a IA precisava ser capaz de executar comandos no terminal do usuário (como rodar testes, compilar código ou listar diretórios) para interagir com o ambiente real. 

O problema difícil surgiu ao lidar com **comandos de longa duração** (como um `npm install` ou um build demorado). A execução inicial bloqueava o *event loop* do Node.js e mantinha a conexão com o LLM (modelo de linguagem) aberta e travada. Pior ainda: quando o modelo decidia chamar múltiplas ferramentas em paralelo, o estado do grafo de execução (nossa máquina de estados em `graph/state.ts`) corrompia. Tínhamos *race conditions*: o agente perdia o contexto de qual terminal pertencia a qual comando, misturando os logs de saída."

### A Investigação (Como investiguei)
"Como eu não tinha um engenheiro mais sênior ou arquiteto para me guiar, tive que ir fundo na raiz do problema sozinho. 
1. Primeiro, reproduzi o problema isolando as chamadas ao `terminal.ts` e adicionei logs detalhados para rastrear o ciclo de vida dos processos.
2. Percebi que o problema não era a IA em si, mas a nossa arquitetura de comunicação síncrona (Request/Response clássica). O agente ficava em *polling* (perguntando 'já acabou?') ou travava esperando o processo no SO terminar, o que causava o timeout."

### As Decisões (Que caminhos avaliei)
"Eu tinha duas opções:
1. **O caminho fácil:** Aumentar os tempos de *timeout* e forçar a IA a rodar uma ferramenta por vez (síncrono). Mas isso deixaria o agente incrivelmente lento e a experiência do usuário na extensão do VS Code seria péssima.
2. **O caminho difícil (que eu escolhi):** Refatorar completamente a execução de ferramentas para um modelo **assíncrono e orientado a eventos (Reactive Wakeup)**, com terminais persistentes que sobrevivem às invocações do LLM."

### A Solução (Como resolvi)
"Eu redesenhei o módulo de terminal (`terminal.ts`) e o fluxo de estado (`state.ts`). 

1. **Terminais Persistentes em Background:** Mudei a execução para que, ao invés de bloquear o fluxo, o comando fosse enviado para um processo em background, retornando imediatamente um `TaskID` para a IA.
2. **Reatividade (Reactive Wakeup):** Configurei o sistema para 'dormir' (liberando a conexão e recursos). Quando o comando do terminal emitia o evento de finalização (ou erro), o sistema automaticamente 'acordava' o agente de IA, injetando o log do terminal diretamente no `historyManager` (memória do agente).
3. **Gerenciamento de Estado Concorrente:** Para resolver a *race condition*, implementei um controle rigoroso no estado do grafo, garantindo que o agente pudesse ler saídas de múltiplos terminais em paralelo sem que as mensagens se misturassem.

**O Resultado:** O Turbo-Agent passou a conseguir executar comandos complexos e demorados de forma fluida. A extensão do VS Code não travava mais, e a IA ganhou a habilidade real de rodar tarefas em segundo plano enquanto continuava conversando com o usuário."

---

**Por que essa resposta brilha aos olhos dos entrevistadores?**
*   **Mostra autonomia:** Você detalha que isolou o problema, investigou os logs e tomou a decisão sozinho.
*   **Vocabulário Técnico Avançado:** Você utiliza termos como *Race Conditions*, *Event Loop*, *Polling*, *Arquitetura Orientada a Eventos* e *Concorrência*.
*   **Foco no Negócio/Usuário:** A justificativa para você escolher o caminho mais difícil foi a "experiência do usuário na extensão", mostrando que você não programa apenas para as máquinas, mas para quem usa o software.
