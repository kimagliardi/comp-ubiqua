**Disciplina:** Desenvolvimento de Software Orientado à Computação Móvel e Ubíqua

---

## 1. Descrição do Projeto

### Visão Geral

O projeto visa desenvolver um **Agente Inteligente** em formato de xApp/rApp para a arquitetura **O-RAN (Open Radio Access Network)**, enquadrando-se no contexto de **Computação Móvel e Ubíqua** ao gerenciar e otimizar recursos de rede de acesso dinamicamente. O software endereçará a necessidade de automação cognitiva no core da rede de acesso, resolvendo a dificuldade de traduzir **intenções de alto nível** (análise de métricas de serviço / prompts de usuário) em **comandos de rede de baixo nível**.

### Objetivos

* **Principal:** Implementar uma **Prova de Conceito (PoC)** de um Agente Inteligente que simula o uso de **Modelos de Linguagem de Grande Escala (LLMs)** para traduzir uma intenção de alto nível (ex: "Melhorar a experiência de vídeo na área X") em um ou mais parâmetros de configuração de rede O-RAN específicos.
* **Funcionalidades-Chave:**
    * **Módulo de Intenção:** Receber a intenção como *string* de texto via API.
    * **Módulo de Tradução/Decisão:** Mapear a intenção para uma ação específica na O-RAN simulada (ex: ajustar a política de prioridade de uma fatia de rede).
    * **Módulo de Atuação:** Integrar-se com uma interface simulada do Controlador Inteligente de RAN (RIC) para enviar o comando de configuração.
* **Requisitos:** O software deve ser modular e aderir, conceitualmente, aos princípios de uma xApp/rApp O-RAN.

### Escopo

* **Principais Características:** O projeto se concentrará no desenvolvimento da lógica do Agente Inteligente (o núcleo da xApp/rApp) e na simulação da sua interface de entrada (intenção) e saída (comando para o RIC).
* **Limitações:** O projeto não incluirá a implementação de uma rede 6G física. A integração com o RIC será via ambiente **simulado** (mockado) ou uma implementação de referência leve. O foco não será na otimização da rede em si, mas na viabilidade da tradução da intenção.

---

## 2. Definições e Decisões de Projeto

### Arquitetura do Software

* **Modelo:** **Microserviço** (simulando uma xApp ou rApp no RIC).
* **Padrão de Design:** Padrão de Agente com foco no processamento de entrada e decisão.
* **Tecnologias:** O agente será desenvolvido em [PREENCHER: *ex: Python*] devido à sua popularidade em microsserviços e bibliotecas de IA/NLP.

### Interfaces e Interação com o Usuário

* **Interface:** Principalmente via **API REST** ou **Linha de Comando (CLI)**. O "usuário" (simulando um sistema de orquestração superior) enviará a intenção como uma *string* de texto.
* **Interação:** A única interação crítica será a submissão da intenção; a saída será o log de confirmação da ação na RAN simulada.

### Tecnologias e Ferramentas

| Tecnologia/Ferramenta | Justificativa |
| :--- | :--- |
| **Linguagem de Programação:** [PREENCHER: *ex: Python*] | Escolhida pela compatibilidade com bibliotecas de NLP e Machine Learning (IA). |
| **Framework de API:** LangChain / SmolAgents| Para criar a interface API  microserviço do Agente. |
| **Simulação de LLM:** Executar um modelo  |
| **Ambiente:** Docker | Para encapsular o microserviço e simular a natureza *cloud native* da O-RAN. |

### Decisões de Projeto

* **Plataforma:** Uso de **Docker** para garantir um ambiente consistente e replicável, simulando o ambiente de contêineres do RIC.
* **Simulação de Inteligência:** A funcionalidade de LLM será simplificada, focando no **mapeamento determinístico** de um conjunto limitado de intenções para ações, para garantir a entrega dentro do prazo e gerenciar o desempenho.
* **Gerenciamento de Recursos:** Foco apenas na medição do uso de recursos do Agente (CPU/Memória); os aspectos de gerenciamento de energia na rede 6G serão tratados conceitualmente.

---

## 3. Metodologia de Avaliação de Desempenho

### Critérios de Desempenho

1.  **Precisão da Tradução (Funcional):** Percentual de intenções de teste que são traduzidas corretamente para a ação de rede esperada.
2.  **Tempo de Resposta (Latência):** O tempo entre o recebimento da intenção na API e o envio do comando de atuação simulado (crítico para xApps).
3.  **Uso de Recursos:** Medição do consumo de CPU/Memória do contêiner do Agente durante o processamento da intenção.
4.  **Experiência do Usuário (Usabilidade da API):** Simplicidade e clareza da interface de submissão de intenções.

### Ferramentas de Avaliação

* **Testes de Latência:** Utilização de bibliotecas de temporização (ex: `time` ou `datetime` em Python) integradas ao código para medir o processamento interno.
* **Profiling:** Uso de ferramentas nativas da linguagem (ex: *cProfile* em Python) para medir o uso de CPU e memória.
* **Testes em Ambiente Simulador:** Execução em ambiente Docker e testes de aceitação (Testes de Sistema) contra a interface simulada do RIC.

### Plano de Testes

| Tipo de Teste | Descrição | Objetivo da Análise |
| :--- | :--- | :--- |
| **Unitários** | Testar separadamente a função de **mapeamento de *string* (intenção) para a ação** (comando). | Garantir que a lógica de tradução (NLP/Regras) está precisa. |
| **De Integração** | Testar a comunicação ponta-a-ponta entre a API de entrada e o Módulo de Atuação. | Identificar falhas de comunicação entre os módulos do microserviço. |
| **De Sistema (Caixa Preta)** | Executar 10-15 cenários de intenção e medir a **Precisão** e a **Latência**. | Otimizar o tempo de resposta e refinar as regras de tradução. |
| **De Usabilidade** | Avaliação da clareza da API e dos logs de saída pelo "usuário" (sistema de orquestração). | Garantir que o Agente é fácil de integrar e monitorar. |

---

## 4. Cronograma de Desenvolvimento

| Fase do Projeto | Marcos e Entregas | Prazos Estimados |
| :--- | :--- | :--- |
| **1. Planejamento e Design** | Definição final da Intenção alvo e Ação de Rede. Estrutura do relatório e ambiente Docker pronto. | [PREENCHER: ex: 1 semana] |
| **2. Implementação da Infraestrutura** | Código base do Microserviço (API) pronto e integração simulada com o RIC. | [PREENCHER: ex: 1,5 semana] |
| **3. Implementação da Lógica Central** | Módulo de Tradução da Intenção (NLP/Regras) implementado e testado. **Entrega:** Protótipo funcional (Versão Beta). | [PREENCHER: ex: 2,5 semanas] |
| **4. Testes, Avaliação e Documentação** | Execução completa dos testes e coleta de dados de desempenho. Elaboração do Relatório Final. **Entrega:** Versão Final do código. | [PREENCHER: ex: 2 semanas] |

---

## 5. Considerações Finais

### Riscos e Mitigações

| Risco | Estratégia de Mitigação |
| :--- | :--- |
| **Escopo muito amplo** na tradução de intenção. | Reduzir o escopo para focar em **um único caso de uso** para garantir a funcionalidade mínima. |
| **Atraso** na configuração do ambiente O-RAN simulado. | Usar um ambiente **mockado** (simplesmente logar a saída) ao invés de tentar a integração com uma plataforma complexa. |
| **Desempenho insatisfatório** da lógica de tradução (latência alta). | Focar na otimização da função de tradução e simplificar a lógica de regras para ganhar velocidade. |

### Expectativas de Futuras Evoluções

* Evoluir a lógica de tradução de regras para um modelo de **LLM/NLP real** de *edge*.
* Expandir o Agente para utilizar a técnica **RAG (Generation Augmented Retrieval)** para consultar o estado real da rede O-RAN antes de tomar a decisão.
* Ampliar a capacidade do Agente para orquestrar múltiplas funções e diferentes fatias de rede.

---

## 6. Entrega

O projeto será entregue em formato de **relatório detalhado** e uma **apresentação oral**, cobrindo todos os pontos deste enunciado.
