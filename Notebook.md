<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

## Semana 01: [Apresentar o projeto e contextualizar o Orientador]
**Período:** 14/09/2026 – 18/09/2026

### Trabalho realizado:
* Organizar e efetuar o levantamento do estado atual do projeto
* Reunião de contextualização com o orientador (Prof. Mario Antunes)

### Próximos Passos:
* Estruturar e investigar as ideias partilhadas com o Orientador (UA) (*)
* Delinear artigos de leitura sobre o caso de estudo do projeto

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

## Semana 02: [Estudar observações do Orientador (UA)]
**Período:** 21/09/2026 – 25/09/2026

### Trabalho realizado:
- Análise dos objetivos das Frameworks e das Métricas partilhadas pelo Orientador (UA)
- Objetivos e diferenças do que já está utilizado relativamente às novas Frameworks e Métricas (*)

> ---

#### (*) *Ideias e notas partilhadas pelo Orientador (UA) na reunião de dia 15/09:*
- ### Frameworks:
    - **Prophet**
        - O Prophet modela a série temporal \(y(t)\) como a soma de três componentes principais:
            $$
            y(t) = g(t) + s(t) + h(t) + \epsilon(t)
            $$

            - Onde:
                - \(g(t)\): **Tendência (Trend)**
                    - Captura as mudanças não-periódicas ao longo do tempo. Pode ser um modelo linear por partes (piecewise linear) ou logístico saturante;
                - \(s(t)\): **Sazonalidade (Seasonality)**
                    - Representa os padrões periódicos recorrentes (ex: diário, semanal, anual), modelados através de séries de Fourier;
                - \(h(t)\): **Feriados e Eventos (Holidays)**
                    - Modela o efeito de feriados ou eventos especiais, com parâmetros dedicados para cada um;
                - \(\epsilon(t)\): **Termo de Erro (Error Term)**
                    - Representa o ruído ou a variabilidade não explicada pelo modelo.

    - **Neural Prophet:**
        - O NeuralProphet modela a série temporal \(x_t\) (ou \(y(t)\)) como uma soma modular de componentes:
            $$
            x_t = T(t) + S(t) + E(t) + F(t) + A(t) + L(t)
            $$

        - Onde:
            - \(T(t)\): **Tendência (Trend)**. Similar ao Prophet, pode ser uma tendência linear ou linear por partes, com pontos de mudança (changepoints);
            - \(S(t)\): **Sazonalidade (Seasonality)**. Modelada com termos de Fourier, suportando múltiplas sazonalidades;
            - \(E(t)\): **Eventos e Feriados (Events/Holidays)**. Efeitos de eventos especiais ou feriados, tratados como covariáveis com coeficientes dedicados;
            - \(F(t)\): **Regressores Futuros (Future Regressors)**. Efeitos de variáveis exógenas cujos valores futuros são conhecidos no momento da previsão;
            - \(A(t)\): **Autorregressão (Autoregression)**. Captura a dependência dos valores passados da própria série, implementada através de uma rede neuronal feed-forward (AR-Net);
            - \(L(t)\): **Regressores Desfasados (Lagged Regressors)**. Efeitos de variáveis exógenas cujos valores futuros **não** são conhecidos, sendo usado apenas o seu histórico (valores desfasados).

    - **Anova:**
        - Ferramenta estatística para entender o passado e filtrar o que realmente importa antes de treinar qualquer modelo;

        - **NOTA** (diferença entre *Anova* e o *Prophet*):
            - O *Prophet* e o *Neural Prophet* são importantes, pois são os motores para prever o futuro;
            - *Anova* já é uma ferramenta de filtragem.

        - Exemplo:
            - Imaginemos que queremos saber se a escolha da transportadora tem impacto real no tempo de atrasos das entregas:
                - *Anova* divida a variâncai total dos dados em dois blocos:
                    - **Variância entre grupos** - Quão distantes estãos as médias de atrasos de cada transportadora em relação à média global;
                    - **Variância dentro dos grupos** - O ruído que cada transportadora tem no seu dia a dia.
                
                - **Fórmula:**
                    $$
                    \frac{\text{Variância entre os transportes (sinal)}}{\text{Variância dentro de cada transportadora (ruído)}}
                    $$

                - Se *$F \approx 1$ e $p > 0.05$*, os transportes têm comportamentos praticamente idênticos. Esta variável é ruído inútil para o modelo.

                - Se *$F \gg 1$ e $p < 0.05$*, a diferença de pontualidade entre transportes é expressiva, então esta compõe uma feature importante para o modelo.

        - **Fluxo**
            - Dados - Feature Engineering - Feature Selection (ANOVA) - Treino dos modelos - Performance

---

- ### Métricas:
    - **MCC** - *Matthews Correlation Coefficient*
        - Métrica de validação para **modelos de classificação**;
        - **Fórmula:**
            $$
            \text{MCC} = \frac{TP \cdot TN - FP \cdot FN}
            {\sqrt{(TP+FP)(TP+FN)(TN+FP)(TN+FN)}}
            $$

        - **Interpretação**:
            - Varia entre: 
                - '-1' -- discordância total;
                - '0' -- predict aleatório;
                - '1' -- previsão perfeita.


    - **HUbber loss** - *Smooth Mean Absolute Error*:
        - Função de perda usada para treinar e avaliar **modelos de regressão**;
        - **NOTA:**
            - Se usarmos **MSE** (Mean Square Error): Os outliers dominam o gradiente e puxam a previsão geral para cima;
            - Se usarmos **MAE** (Mean Absolute Error): Não é contínua em zero o que pode dificultar a convergência.

        - **Fórmula:**

            $$
            L_\delta(y, \hat{y}) =
            \begin{cases}
            \dfrac{1}{2}(y - \hat{y})^2 & \text{se } |y - \hat{y}| \le \delta \\[6pt]
            \delta \cdot \left(|y - \hat{y}| - \dfrac{1}{2}\delta\right) & \text{caso contrário}
            \end{cases}
            $$

            - Onde:
                - $y$: valor real
                - $\hat{y}$: valor previsto
                - $\delta > 0$: parâmetro que define o ponto de transição entre a zona quadrática e a zona linear

        - **NOTA:**
            - Para pequenos desvios, comporta-se quadraticamente como o **MSE**:
            $$
            \text{se } |y - \hat{y}| \le \delta
            $$
            - Para grandes erros, torna-se linear como o **MAE** (não explode com outliers):
            $$
            \text{se } |y - \hat{y}| > \delta
            $$

        
        - **Detalhe:**
            - O hiperparâmetro $\delta$ define a fronteira exata entre o que consideramos "ruído operacional tolerável".

    - **Diagrama da Huber Loss**:
        ![Diagrama da Huber Loss](Huber-loss-vs-MSE-loss-and-MAE-loss.jpg)
        

---

## Próximos Passos:
- Perceber a necessidade/possibilidade de inserção do estudado acima
- Identificar artigos de estudo para leitura e análise

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 03: [Levantamento de Artigos relevantes com tema do projeto]
**Período:** 28/09/2026 – 02/10/2026

## Trabalho realizado:
- Escolha dos Artigos que se enquadram com o tema do projeto
- Avalição dos resultados e dos estudos já existentes

## Próximos Passos:
- Recolha das análises e notas aos artigos lidos.

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 04: [Recolha dos resultados dos artigos lidos & Estado atual do projeto]
**Período:** 05/10/2026 – 09/10/2026

## Trabalho realizado:
- Retirar notas importantes das leituras e análises efetuadas aos artigos
- Recolha dos resultados dos artigos lidos

## Próximos Passos:
- Começar o estágio
- Perceber o estado atual do projeto e o que já foi desenvolvido


</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 05: [Início do estágio]
**Período:** 12/10/2026 – 16/10/2026

## Trabalho realizado:
- Inicio do estágio
- Apresentação da equipa
- Introdução à dinâmica do estágio e ao projeto
- Contextualização do ambiente logistico e da cadeia de abastecimento
- Estado atual do projeto


</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 06: [Recolha dos resultados dos artigos lidos & Estado atual do projeto]
**Período:** 19/10/2026 – 23/10/2026

## Trabalho realizado:
- Estudo de variáveis importantes para inserir no modelo
- Algoritmos testados -> Random Forest
- Tratamento e normalização de dados

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 07: [Preparação Sprint 2; Continuação do desenvolvimento e testes; Migrações e tratamento de dados]
**Período:** 26/10/2026 – 30/10/2026

## Trabalho realizado:
- Levantamento do estado de arte - preparação do Sprint2
- Outros algoritmos testados - XGB e LGBM
- Continuação do tratamento de dados e feature engineering
- Migração de BO's - continuação do processo à medida que vão existindo updates

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 08: [Testar novos tipos de variáveis; Clustering suppliers; Migrações de BO's]
**Período:** 02/11/2026 – 06/11/2026

## Trabalho realizado:
- Novos tipos de dados sondados - Custom data, meio de transporte, client demand volatility
- Clustering de supplier (Relação supplier x plant)
- Migração dos BO's (reuniões para alinhar detalhes das nossas tabelas do DALI comparativamente com o objetivo dos BO's existentes)

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 09: [Migrações de BO's; Clustering suppliers; Clustering supplier per plant]
**Período:** 09/11/2026 – 13/11/2026

## Trabalho realizado:
- Clustering de supplier e Clustering de supplier per plant (testados novamente vários algoritmos de clustering - DBSCAN/K Means/Gaussian Mixture/Hierarchical (Ward))
- Migração dos BO's

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 10: [Material Clustering; Migrações de BO's]
**Período:** 16/11/2026 – 20/11/2026

## Trabalho realizado:
- Identificar materiais criticos
- Avaliar que tipo de dados é que temos nos BO's e/ou nas tabelas do DALI que nos permitam filtrar por tipos/famílias/grupos de materiais
    - Confirmar a percentagem de coverage para os materiais usados e presentes no nosso dataset, para perceber efetivamente se ganhamos a adicionar esse tipo de variáveis 
- Clustering de materiais
- Migração dos BO's
- Preparação do Sprint III (Arquitetura)

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---

<div style="border: 1px solid #444; border-radius: 8px; padding: 18px; margin-bottom: 24px; background-color: #1e1e1e;">

# Semana 11: [Comparação do Material Clustering e do Supplier Clustering; Retreinar o melhor modelo com todos os dados no final; Migrações de BO's]
**Período:** 23/11/2026 – 27/11/2026

## Trabalho a realizar:
- Comparar os resultados do material clustering com os resultados do supplier clustering
    - Para poder perceber se estão relacionados e se trabalhar com dois clusterings diferentes é vantajoso ou nao
- Retreinar o modelo com melhor performance (com melhor Diabo_Score, na regressão, e com boa relação com o F1 Score na classificação) com todos os dados disponíveis (append val_df with train_df)
- Migração dos BO's
- Sprint III (Arquitetura)

</div>

> ---

<div align="center">❖ ── ✦ ── ❖</div>

> ---