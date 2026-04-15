# Projeto: Dashboard para visualização de dados de uma planta física em tempo real

### 1. Identificação do Grupo
* **Instituição:** Centro Educacional da Fundação Salvador Arena
* **Curso:** Engenharia de Controle e Automação
* **Grupo:** B
* **Integrantes:** 
    * [Jhuan Henrique Luz Dias] - RA: [062210023]
    * [Lucas Veiga Bezerra] - RA: [062210006]
    * [Yasmin Souza Correia] - RA: [062210008]
    * [Tifany Mariane Ferreira] - RA: [062210019]
    * [Vinicius Koiti Vila Nova Tsuchiya] - RA: [062210022]
      
---

### 2. Área Problema Selecionada
O grupo seleciona uma das áreas norteadoras abaixo para o desenvolvimento do projeto:
* [ ] Manutenção Preditiva de Zero-Downtime
* [ ] Eficiência Energética e Descarbonização via Smart Grids
* [ ] Controle de Qualidade Autônomo com Visão Computacional
* [X] Gêmeos Digitais (Digital Twins) e Analytics em Tempo Real

---

### 3. Diagnóstico e Definição do Problema
Esta seção apresenta a fundamentação do desafio. O grupo descreve o cenário de atuação e justifica a importância da solução proposta.
* **Contexto:** O projeto aborda o cenário de Indústria 4.0.
* **Problema:** A dificuldade central reside em integrar o sistema físico com a aplicação digital em tempo real.
* **Impacto:** A solução visa otimizar os custos com IHM's e integrar o sistema com geração de dados da planta.

---

### 4. Arquitetura de Dados (Fonte e Dataset)
O projeto utiliza dados estruturados para alimentar os modelos preditivos.
* **Origem dos Dados:** Os dados serão gerados pela planta física montada para o Projeto de Final de Curso.
* **Características:** O conjunto de dados apresenta variáveis como Peso medido pelas células de carga, PWM das bombas, setpoint, curva característica de dosagem e os ganhos do PID.
* **Volume:** Ainda não se tem o volume de dados.

---

### 5. Plano de Tratamento de Dados (ETL)
O pipeline de dados segue as seguintes etapas de processamento:
1. **Extração:** Os dados serão extraídos da planta através da conexão do dashboard com o ESP32 através do protocolo MQTT.
2. **Transformação:** O grupo aplica a limpeza de valores ausentes, a remoção de outliers e a normalização das escalas numéricas.
3. **Carga:** Os dados tratados são disponibilizados na pasta `/data/processed` para consumo dos modelos de Machine Learning.

---

### 6. Estrutura do Repositório
A organização das pastas facilita a manutenção e o versionamento do projeto:
* `/docs`: Contém os diagramas de fluxo de dados e a documentação técnica.
* `/data/raw`: Armazena os arquivos de dados originais (não modificados).
* `/data/processed`: Armazena os dados após a execução do script de ETL.
* `/scripts`: Contém os códigos Python responsáveis pelo tratamento dos dados.
* `requirements.txt`: Lista todas as bibliotecas necessárias para a execução do projeto.

---

### 7. Instruções para Execução
Para reproduzir o ambiente de dados e executar o pipeline de ETL:
1. Clona-se este repositório.
2. Instalam-se as dependências através do comando:
   ```bash
   pip install -r requirements.txt
   ```	

---

### 8. Etapa 02 (M2) - Análise Exploratória de Dados (EDA)

**Link do Notebook Executável:** https://colab.research.google.com/drive/1xx957LJXttDMHhqf9VWNMmgstkPNpeTy?usp=sharing

**Contexto e Gêmeo Digital:**
Como o hardware da planta (ESP32/Sensores) ainda está sendo ajustado, esta etapa de análise foi realizada sobre um Gêmeo Digital parametrizado. O dataset foi construído com dependência causal, onde a atuação do PWM dita a dinâmica do peso lido. O modelo assume comportamento linear na faixa de operação, sendo uma base sólida para etapas de modelagem preditivas industriais reais.

**Pipeline de ETL e Limpeza:**
Executamos o tratamento de perdas de pacote do sensor (Nulls) e aplicamos o cálculo de Amplitude Interquartil (IQR). Esse filtro estatístico é vital para definir os limites seguros de operação e remover ruídos de vibração mecânica, garantindo a qualidade do dataset para o treinamento da IA. Após o processo de limpeza, o dataset final resultou em 994 amostras válidas, evidenciando a remoção correta de falhas simuladas de sensor e garantindo integridade estatística para a análise.

**Hipóteses Validadas:**

1. **Hipótese 1:** O sinal PWM da bomba possui correlação direta com o peso.  
   *(Validado pela linha de tendência no Gráfico 3 de Regressão).*

2. **Hipótese 2:** O erro da malha de controle é inversamente proporcional ao peso lido.  
   *(Validado pelo Heatmap de correlação).*

---

### 9. Apêndice de Uso Ético de IA (Transparência)

Conforme as diretrizes da disciplina, a IA (Google Gemini) foi utilizada como ferramenta de apoio ao desenvolvimento, exclusivamente em atividades de pair-programming, organização de código e auxílio de sintaxe (Pandas, Seaborn e Matplotlib).

Nenhuma análise estatística foi aceita sem validação manual dos fundamentos por parte do grupo, conforme a Regra de Ouro.

**A. Exemplos de Interação com IA**
Durante o desenvolvimento, a ferramenta foi utilizada em interações como:

- Auxílio na estruturação de DataFrames em Pandas  
- Sugestões de funções para visualização de dados (heatmap, regplot)  
- Organização do pipeline de ETL  

Inicialmente, a IA sugeriu a geração de dados com distribuições aleatórias independentes, o que resultou em ausência de correlação entre variáveis.

**B. Intervenção e Correção Técnica do Grupo**
O grupo identificou que essa abordagem violava princípios fundamentais da Engenharia de Controle, pois não havia relação entre variável de atuação (PWM) e variável de processo (peso). A partir disso, foi definida manualmente a modelagem causal do sistema:

Relação linear entre PWM e peso.

Cálculo direto do erro PID (setpoint - variável de processo).

A IA foi então utilizada apenas para auxiliar na implementação dessa lógica matemática em código Python.

**C. Validação Técnica (Regra de Ouro)**
O grupo realizou as seguintes verificações:

Conferência da coerência física do modelo (atuador impactando o processo).

Validação manual do método IQR para remoção de outliers.

Análise crítica da matriz de correlação e da regressão linear.

Essas etapas garantiram que os resultados apresentados refletem um comportamento realista de um sistema de dosagem industrial de poliol, e não apenas uma geração automatizada de dados.
