Viewed package.json:1-12

Para uma funcionalidade nova em um sistema crítico de produção, a palavra-chave é **mitigação de risco**. O fluxo não é apenas escrever código, mas garantir que o que já funciona continue funcionando, enquanto entregamos valor com segurança.

Aqui está um passo a passo detalhado e profissional de como eu conduzo esse processo, que você pode usar como base para a sua resposta:

---

### 1. Entendimento e Refinamento (Discovery)
Antes de escrever qualquer linha de código, preciso ter certeza do que estamos resolvendo.
*   **Análise de Requisitos:** Leio a task/issue (Jira, Linear, etc) e converso com o Product Manager (PM) ou com o cliente para alinhar as regras de negócio.
*   **Casos Extremos (Edge Cases):** Faço perguntas sobre os cenários de erro. *"O que acontece se o serviço de terceiros cair?"*, *"O que acontece se o usuário submeter dados inválidos?"*
*   **Análise de Impacto:** Avalio onde essa nova feature toca em funcionalidades antigas. Isso me ajuda a mapear quais testes de regressão serão mais importantes.

### 2. Setup e Estratégia de Branching
*   Atualizo minha branch local a partir da branch principal (`main` ou `develop`).
*   Crio uma nova *feature branch* seguindo o padrão da equipe (ex: `feat/TICKET-123-nome-da-feature`).
*   **Feature Flags (Toggle):** Como o sistema já está em produção e tem tráfego constante, planejo desde o início colocar essa nova funcionalidade atrás de uma *Feature Flag*. Isso me permite fazer o deploy do código para produção de forma invisível para os usuários, ativando apenas para testes internos primeiro.

### 3. Desenvolvimento e Testes (Hands-on)
*   **TDD (Test-Driven Development) ou Testes Nativos:** Começo escrevendo (ou planejando) os testes unitários da regra de negócio. Se for uma API, escrevo os testes de integração das novas rotas.
*   **Desenvolvimento:** Implemento o código seguindo os padrões do projeto (Clean Code, princípios SOLID). Presto muita atenção à performance (ex: verificando se não estou criando consultas *N+1* no banco de dados).
*   **Tratamento de Erros e Observabilidade:** Adiciono logs estratégicos (`info`, `warn`, `error`) para que, quando a feature for para o ar, eu consiga rastrear o que está acontecendo através do Datadog, Kibana ou CloudWatch.

### 4. Revisão Local e Pull Request (PR)
*   Rodo os linters, formatadores e a suíte de testes localmente para garantir que não quebrei nada.
*   Subo a branch e abro o Pull Request (PR).
*   **Descrição do PR:** Escrevo uma descrição clara explicando *o que* foi feito, *por que* foi feito e anexo evidências (screenshots, vídeos curtos ou payloads de API).
*   **Code Review:** Solicito a revisão de pelo menos um (idealmente dois) engenheiros da equipe. Fico aberto a feedbacks e faço os ajustes necessários.

### 5. CI/CD e Ambiente de Staging (QA)
*   Ao abrir o PR, a esteira de CI (Integração Contínua - ex: GitHub Actions) vai rodar automaticamente todos os testes automatizados, análise de segurança e linting.
*   Após o *merge*, o código é automaticamente implantado (Deploy) no ambiente de **Staging** (homologação).
*   **Validação:** No ambiente de Staging (que deve ser uma réplica fiel da produção), eu e/ou o time de QA testamos a funcionalidade de ponta a ponta, simulando o uso real.

### 6. Deploy em Produção (Rollout Seguro)
Não fazemos deploy "cruzando os dedos". Usamos estratégias seguras:
*   O merge vai para a `main` e a pipeline de CD faz o deploy em Produção.
*   Como a funcionalidade está atrás de uma **Feature Flag**, o código novo entra em produção desativado para os clientes finais.
*   Ativo a flag apenas para a equipe interna ou para um grupo de usuários beta (Canary Release). Validamos em produção real se não há quebras.
*   Se tudo estiver ok, abro a flag para 10%, 50% e, por fim, 100% da base de clientes.

### 7. Pós-Deploy e Monitoramento
A tarefa não termina no momento do deploy.
*   **Acompanhamento Inicial:** Passo os próximos minutos/horas monitorando as ferramentas de APM (Application Performance Monitoring) e de rastreamento de erros (como o Sentry).
*   Verifico se a taxa de erros do sistema aumentou, se os tempos de resposta da API subiram ou se há picos de uso de CPU/Memória.
*   **Rollback Rápido:** Se algo der muito errado, eu simplesmente desligo a *Feature Flag* (o que leva segundos) em vez de precisar fazer um *revert* de código e um novo deploy.

---

**Dica de Entrevista:** 
O que os recrutadores ou líderes técnicos mais querem ouvir em uma pergunta como essa não é se você sabe a sintaxe da linguagem, mas sim se você se preocupa com:
1. **Não quebrar o que já funciona** (Testes e Feature Flags).
2. **Entender o problema antes de codar** (Alinhamento de requisitos).
3. **Observabilidade** (Saber o que está acontecendo com o seu código quando ele sai da sua máquina).
