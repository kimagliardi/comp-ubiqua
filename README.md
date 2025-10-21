**Disciplina:** Desenvolvimento de Software Orientado à Computação Móvel e Ubíqua

---

## 1. Descrição do Projeto

### Visão Geral

O projeto visa desenvolver um **Agente Inteligente** em formato de rApp para a arquitetura **O-RAN (Open Radio Access Network)**, enquadrando-se no contexto de **Computação Móvel e Ubíqua** ao gerenciar e otimizar recursos de rede de acesso dinamicamente. O software endereçará a necessidade de automação "cognitiva" no core da rede de acesso, resolvendo a dificuldade de traduzir **intenções de alto nível** (como "Reduzir o consumo de energia sem afetar a latência") em políticas de gerenciamento de energia.

### Objetivos

* **Principal:** Implementar uma **Prova de Conceito (PoC)** de um Agente Inteligente que simula o uso de **Modelos de Linguagem de Grande Escala (LLMs)** para traduzir uma intenção de alto nível de **Eficiência Energética** (ex: "Entrar em modo de economia de energia") em parâmetros de configuração da RAN simulada.
* **Funcionalidades-Chave:**
    * **Módulo de Intenção:** Inicialmente deve receber a intenção como *string* de texto via API.
    * **Módulo de Tradução/Decisão:** Mapear a intenção para uma ação específica de controle de energia na O-RAN simulada (ex: ajustar a potência da célula ou desligar um setor).
    * **Módulo de Atuação:** Integrar-se com uma interface simulada do Controlador Inteligente de RAN (RIC) para enviar o comando de configuração de energia.
* **Requisitos:** O software deve ser modular e aderir, conceitualmente, aos princípios de uma xApp/rApp O-RAN.

### Escopo

* **Principais Características:** O projeto se concentrará no desenvolvimento da lógica do Agente Inteligente (o núcleo do rApp) e na simulação da sua interface de entrada (intenção) e saída (comando para o RIC). O Agente considerará a carga de tráfego simulada (do free5gc ) como o principal fator de decisão.
* **Limitações:** A integração com o RIC será via ambiente **simulado com o free5gc** para o envio de comandos de "ajuste de energia". O foco não será na otimização da rede em si, mas na viabilidade da tradução da intenção.

---

## 2. Definições e Decisões de Projeto

### Arquitetura do Software

* **Modelo:** **Microserviço** (simulando uma rApp no RIC, devido ao aspecto Non-Real-Time das decisões de energia).
* **Padrão de Design:** Padrão de Agente com foco no processamento de entrada, consulta de estado (simulado) e decisão.
* **Tecnologias:** O agente será desenvolvido em Python.

### Interfaces e Interação com o Usuário

* **Interface:** Principalmente via **API REST** ou **Linha de Comando (CLI)**. O "usuário" (simulando um sistema de orquestração superior) enviará a intenção como uma *string* de texto.
* **Interação:** A saída será o log de confirmação da ação de controle de energia (ex: "Potência da Célula X reduzida para 50%") na RAN simulada.

### Tecnologias e Ferramentas

| Tecnologia/Ferramenta | Justificativa |
| :--- | :--- |
| **Linguagem de Programação:** Python | Escolhida pela compatibilidade com de agentes de AI tais como SmolAgent e Langchain. |
| **Framework de Agentes:** LangChain / SmolAgents| Para construir o fluxo de raciocínio (observar dados -> raciocinar/planejar -> agir) e usar as *Tools* do Agente. |
| **Simulação de LLM:** Modelo LLM simples ou funções determinísticas | Focar no **mapeamento determinístico** de intenções-chave para ações de controle de energia, priorizando a entrega. |
| **Ambiente:** Docker/ Ollama | Para encapsular o microserviço e simular a natureza *cloud native* da O-RAN. |

### Decisões de Projeto

* **Plataforma:** Uso de **Docker** para garantir um ambiente consistente. A lógica do Agente utilizará *Mocks de API* para simular a leitura de métricas do free5gc e a escrita de comandos no RIC.
* **Simulação de Inteligência:** A funcionalidade de LLM será simplificada, focando no **mapeamento determinístico** de um conjunto limitado de intenções de eficiência energética (ex: "Economizar energia agora", "Voltar ao normal") para comandos simulados de RAN.

---

## 3. Metodologia de Avaliação de Desempenho

### Critérios de Desempenho

1.  **Precisão da Tradução (Funcional):** Percentual de intenções de teste relacionadas à energia que são traduzidas corretamente para a ação de controle de energia esperada.
2.  **Tempo de Resposta (Latência):** O tempo entre o recebimento da intenção na API e o envio do comando de atuação simulado.
3.  **Uso de Recursos:** Medição do consumo de CPU/Memória do contêiner do Agente durante o processamento da intenção.
4.  **Experiência do Usuário (Usabilidade da API):** Simplicidade e clareza da interface de submissão de intenções e dos logs de resultado.

### Ferramentas de Avaliação

* **Testes de Latência:** Utilização de bibliotecas de temporização (ex: `time` ou `datetime` em Python) integradas ao código para medir o processamento interno. (Prometheus)
* **Profiling:** Uso de ferramentas nativas da linguagem (ex: *cProfile* em Python) para medir o uso de CPU e memória do Agente. (Prometheus)
* **Testes em Ambiente Simulador:** Execução em ambiente Docker e testes de aceitação (Testes de Sistema) contra a interface simulada do RIC.

### Plano de Testes

| Tipo de Teste | Descrição | Objetivo da Análise |
| :--- | :--- | :--- |
| **Unitários** | Testar separadamente a função de **mapeamento de *string* (intenção) para a ação de controle de energia**. | Garantir que a lógica de tradução do agente está precisa para a Eficiência Energética. |
| **Integração** | Testar a comunicação ponta-a-ponta entre a API de entrada, o Módulo de Decisão e o Módulo de Atuação. | Identificar falhas de comunicação entre os módulos do microserviço. |
| **Performance** | Executar 10-15 cenários de intenção e cenários simulados de carga de tráfego, medindo a **Precisão** e a **Latência**. | Otimizar o tempo de resposta e refinar as regras de tradução. |

---

## 4. Cronograma de Desenvolvimento

| Fase do Projeto | Marcos e Entregas | Prazos Estimados | Data Prevista (Início 20/Outubro) |
| :--- | :--- | :--- | :--- |
| **1. Planejamento e Design** | Definição final das Intenções de Energia e Ações de Comando de RAN. Estrutura do relatório e ambiente Docker inicial pronto. | 1 semana | 27 de Outubro |
| **2. Implementação da Infraestrutura** | Código base do Microserviço (API) pronto e integração simulada (Mocks) com o RIC. | 1,5 semana | 07 de Novembro |
| **3. Implementação da Lógica Central** | Módulo de Tradução da Intenção (NLP/Regras) e *Tools* do Agente implementados e testados. **Entrega:** Protótipo funcional (Versão Beta). | 2,5 semanas | 28 de Novembro |
| **4. Testes, Avaliação e Documentação** | Execução completa dos testes e coleta de dados de desempenho. Elaboração do Relatório Final e Apresentação. **Entrega:** Versão Final do código. | 2 semanas | 12 de Dezembro |

---

## 5. Considerações Finais

### Riscos e Mitigações

| Risco | Estratégia de Mitigação |
| :--- | :--- |
| **Escopo muito amplo** na tradução de intenção de energia. | Reduzir o escopo para focar em **um único tipo de controle** (ex: ajuste de potência) para garantir a funcionalidade mínima. |
| **Atraso** na configuração do ambiente O-RAN simulado. | Usar um ambiente **mockado** (simplesmente logar a saída) ao invés de tentar a integração com uma plataforma complexa. |
| **Desempenho insatisfatório** da lógica de tradução (latência alta). | Focar na otimização da função de tradução e simplificar a lógica de regras para ganhar velocidade. |

### Expectativas de Futuras Evoluções

* Expandir o Agente para utilizar a técnica **RAG (Generation Augmented Retrieval)** para consultar o estado real da rede O-RAN (ex: carga de tráfego) antes de tomar a decisão de controle de energia.
* Ampliar a capacidade do Agente para gerenciar múltiplas células e diferentes políticas de economia de energia.

* Utilizar métricas de rede ao invés de prompts / requisições.