# 🛡️ Detecção de Fraude em Cartões de Crédito

## 📌 O Problema e o Desafio do Desbalanceamento
Neste projeto, o objetivo é detectar transações fraudulentas em cartões de crédito. O maior desafio é o **extremo desbalanceamento dos dados**: cerca de 99,8% das transações são autênticas e apenas 0,17% são fraudes. 
Em cenários assim, a **acurácia é uma métrica enganosa** (um modelo que chuta "não é fraude" para tudo terá 99,8% de acurácia e será inútil). Por isso, a avaliação deste projeto foca em **Recall** (capturar o máximo de fraudes possível) e **F1-Score**.

## 🛠️ Preparação dos Dados
Seguindo as melhores práticas de Ciência de Dados, o pipeline de preparação incluiu:
- **Transformação Logarítmica:** Aplicada na variável `Amount` (valor da transação) para atenuar o impacto de *outliers*.
- **Padronização:** Uso do `StandardScaler` nas variáveis `Time` e `Amount_log`.
- **Split Estratificado:** Separação de treino e teste utilizando o parâmetro `stratify=y`, garantindo que a proporção de 0,17% de fraudes se mantivesse igual em ambos os conjuntos.

## 🤖 Comparação entre os Modelos
Foi estabelecida uma *baseline* com Regressão Logística e, em seguida, testamos modelos baseados em árvores (XGBoost).
Para lidar com o desbalanceamento no treinamento, utilizei o `class_weight='balanced'` na Regressão Logística e ajustei o `scale_pos_weight` no XGBoost.
* **Regressão Logística:** Apresentou um bom Recall para a classe de fraude, mas com baixa precisão (muitos falsos positivos).
* **XGBoost:** Mostrou o melhor equilíbrio entre Precisão e Recall (maior F1-Score), conseguindo detectar as fraudes sem bloquear tantos cartões legítimos.

## 🔍 Explicabilidade com SHAP e Limiar de Decisão
Através do ajuste da curva *Precision-Recall*, ajustamos o limiar de decisão para maximizar o Recall da classe 1 (fraudes). 
Para evitar o efeito "caixa preta", foi implementada a biblioteca **SHAP**. O gráfico *summary_plot* gerado no notebook revela que as variáveis transformadas em PCA (como V14, V4 e V17) foram as que tiveram maior peso para o modelo classificar uma transação como fraudulenta.

## 🚀 Minhas Evoluções (O que fiz diferente)
Em relação ao que foi construído em aula, implementei as seguintes evoluções para demonstrar domínio técnico:
1. Adicionei o modelo **XGBoost**, otimizando o hiperparâmetro `scale_pos_weight` via cálculo matemático da proporção das classes de treino.
2. Criei a etapa de explicabilidade avançada importando a biblioteca `shap`, gerando uma visualização profissional que o mercado exige para justificar bloqueios de cartões de clientes reais.
