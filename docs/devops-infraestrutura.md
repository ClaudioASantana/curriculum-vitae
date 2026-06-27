Proximo:
Essa é uma ótima oportunidade para mostrar que você não é apenas o desenvolvedor que "joga o código por cima do muro" e espera que a equipe de infra resolva. O perfil ideal hoje é o desenvolvedor que tem a mentalidade DevOps.

Com base no que vi no seu ambiente (onde temos configurações de Docker, Azure DevOps Agents `azp-agent`, diretório `.kube` indicando Kubernetes e ambientes em WSL), construí uma resposta bem alinhada com a realidade de um Full-Stack com forte vivência em infraestrutura.

Aqui está a sugestão de resposta:

---

Embora o meu foco principal seja o desenvolvimento Full-Stack (Engenharia de Software), tenho uma vivência muito forte e prática com Infraestrutura e DevOps. Acredito que o desenvolvedor moderno precisa saber exatamente onde e como o seu código vai rodar. 

Aqui estão as minhas principais experiências e responsabilidades nessa área:

### 1. Containerização e Orquestração
*   **Docker:** Faço o desenho e a escrita dos `Dockerfiles` das aplicações, garantindo que as imagens sejam leves (usando *multi-stage builds*) e seguras para produção. No dia a dia, uso `docker-compose` para espelhar o ambiente de produção na máquina de desenvolvimento.
*   **Kubernetes (K8s):** Tenho experiência prática interagindo com clusters Kubernetes. Sei criar e gerenciar *Deployments*, *Pods*, configurar *ConfigMaps/Secrets* e escalar serviços quando necessário.

### 2. CI/CD (Integração e Entrega Contínuas)
*   **Pipelines Automatizadas:** Tenho forte atuação na criação e manutenção de esteiras de CI/CD. Já trabalhei bastante com ferramentas como **Azure DevOps** (inclusive configurando *Self-hosted Agents* em Docker) e **GitHub Actions**. 
*   **Automação do Ciclo de Vida:** Nas minhas esteiras, garanto a automação de testes unitários, análise de qualidade de código (linting), build das imagens Docker e o deploy automatizado nos ambientes de Staging e Produção.

### 3. Gestão de Ambientes Cloud e Linux
*   **Linux/WSL:** Tenho bastante conforto operando terminais Linux, gerenciando permissões, escrevendo scripts bash para automação de tarefas rotineiras e diagnosticando problemas em servidores.
*   **Cloud Providers (AWS / Azure):** Experiência em provisionar e gerenciar recursos na nuvem, seja configurando bancos de dados gerenciados, buckets de armazenamento (S3) ou configurando os serviços que vão hospedar os containers.

### 4. Observabilidade e Resolução de Problemas em Produção
*   Quando o código vai para o ar, não considero o trabalho finalizado. Tenho o hábito de monitorar ativamente as aplicações.
*   Sei investigar gargalos de performance e erros analisando logs em ferramentas centralizadas (como Datadog, CloudWatch ou ELK Stack) e gerenciar alertas críticos de produção.

---

**Resumo da Ópera:** 
Eu não sou um SRE (Site Reliability Engineer) 100% dedicado, mas tenho total autonomia para pegar um projeto do zero, estruturar o repositório, containerizar, criar a esteira de CI/CD e colocar para rodar com segurança na nuvem, mantendo a saúde do ambiente de produção.

*(Dica: Se quiser que eu dê mais ênfase em alguma nuvem específica, como AWS ou Azure, ou ferramenta que você domina mais, é só me falar!)*
