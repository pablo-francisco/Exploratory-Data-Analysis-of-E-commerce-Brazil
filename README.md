# Análise dos dados de uma empresa de e-commerce, a OLIST.

## Coleta de dados.

Os dados foram coletados em [Kaggle - Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

## Definição de problemas

Suponha que você é um cientista/analista de dados trabalhando na empresa Olist, e foram requisitadas algumas análises e conclusões obtidas a partir do banco de dados fornecido. As análises principais devem responder os seguintes questionamentos:

* **Qual a frequência em atrasos superiores a 14 dias?** e qual a faixa de atrasos mais recorrentes?

* **O peso do produto possui relação com o custo de frete?**

* **Qual é a taxa de cancelamento de pedidos?**

* **Quais os fatores que podem intensificar o cancelamento dos pedidos?**

* **É possível realizar um sistema no qual identifica os produtos foram comprados em conjunto com uma maior frequência?** Apresente uma versão inicial do projeto.

## Objetivos do projeto

* Realizar uma análise exploratória dos dados para solucionar os objetivos mais simples.

* Aplicar um modelo de **Machine Learning** e **técnicas estátisticas** para encontrar as características que podem **influenciar na taxa de cancelamento** de pedidos 

* Realizar um algoritmo que apresente a relação entre os **produtos mais comprados em conjunto** com determinado produto.

## Metodologia.

### Coleta e filtragem de dados
Foram realizadas a mesclagem dos variados conjuntos de dados em conjunto com técnicas de data cleaning para remover valores irregulares, como os valores nulos.

### Criação de features

Foram criadas novas características a partir dos dados que já estão presentes, com o objetivo de explicar e entender melhor o comportamento do conjunto de dados, as novas features criadas foram:

* **Dias_para_entrega:** Tempo estimado para a entrega do produto.

* **Dias_atraso:** Valores negativos indicam que foi entregue com X dias de antecedência, e positivos com atraso de X dias.

* **qntd_produtos_cesta:** Quantidade de itens que o consumidor comprará naquela transação.

* **qntd_compras:** Quantas compras já foram feitas para aquele produto, indicando sua popularidade entre os consumidores.

* **Class_Atraso:** Atrasos nos prazos de entregas divididos em categorias, sendo elas:

    * Atraso Leve (Atrasos de até 7 dias);
    * Atraso Médio (Atrasos de 7 a 14 dias);
    * Atraso Grave (Atrasos de 14 a 30 dias);
    * Atraso Gravíssimo (Atrasos maiores que 30 dias).

## Feature scaling/encoding

Foram transformados todos os dados categóricos de texto em dados numéricos, utilizando o Label Encoding, que transforma os dados em números ordinais.

|Label Encoding | Método de Pagamento |
|----|---------------------|
| 1  | credit_card         |
| 2  | boleto              |
| 3  | voucher             |
| 4  | debit_card          |
| 5  | not_defined         |

Já os dados numéricos foram escalonados utilizando a técnica de padronização a partir do desvio padrão.

z = (x - μ) / σ

Onde:
- z é o valor escalonado (standardizado/padronizado).
- x é o valor original.
- μ é a média dos valores no conjunto de dados.
- σ é o desvio padrão dos valores no conjunto de dados.

## Random Forest e ANOVA
 
Nessa etapa são separados os dados alvo (y) dos demais (X) e é feita a aplicação nos métodos ANOVA e Random Forest entre X e y cujo o objetivo a partir da aplicação é determinar os dados que mais influenciam da variável alvo.

### Random Forest

Random Forest é um algoritmo de aprendizado de máquina que utiliza uma técnica de ensemble de árvores de decisão, sendo eficaz para determinar a importância das features em um conjunto de dados. O método de feature importance do Random Forest mede a diminuição na precisão do modelo quando uma feature específica é removida ou embaralhada, podendo esclarecer quais features (X) são mais relevantes.

### ANOVA (Análise de Variância)

ANOVA é uma técnica estatística que compara a média entre dois ou mais grupos para determinar se há diferenças estatisticamente significativas entre eles. No contexto de feature importance, ANOVA pode ser usada para avaliar a relação entre uma feature e uma variável alvo (y). O teste ANOVA calcula a variância entre os grupos e dentro dos grupos, e determina se a variância entre os grupos é significativamente maior do que a variância dentro dos grupos. Se a diferença for significativa, isso sugere que a feature tem influência na variável alvo.

Para mais informações sobre Random Forest, consulte a documentação oficial do [scikit-learn](https://scikit-learn.org/stable/modules/ensemble.html#forest) e sobre ANOVA, consulte a documentação do [SciPy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.f_oneway.html).

## Matriz de compras

Por fim foi realizada uma matriz onde em suas linhas e colunas são representadas por cada produto presente, e os seus valores são equivalentes à quantidade de vezes em que produto *M* foi comprado em conjunto com produto *N*, segue exemplo:

|  | Produto 1 | Produto 2 | Produto 3 |
|---------|----------|----------|----------|
| Produto 1 | 0 | 3 | 0 |
| Produto 2 | 3 | 1 | 0 |
| Produto 3 | 0 | 0 | 2 |

Em que no exemplo acima o produto 2 foi comprado em duas unidades, e em conjunto com 3 unidades do produto 1.

## Resultados

* A quantidade de atrasos superiores a 14 dias é de **50,89%** e a classe de atrasos mais frequente está dentro de um intervalo de até 7 dias e ocupa um total de **49,11%** dos atrasos.

* Correlação entre peso do produto e o frete, verifica se há influência entre essas variáveis numéricas, sendo constatada uma relação diretamente proporcional de **61,13%** entre si.

* A taxa de cancelamento é de **0.45%** do total de compras realizadas.

* A análise dos fatores que podem contribuir para o cancelamento, sendo os principais a avaliação dos clientes, a popularidade do produto o preço e frete além dos dias que levaram até completar a entrega, uma amostra dos 4 fatores mais relevantes para cada método utilizado (ANOVA e Random Forest) estão abaixo:

|               | Random Forest   | ANOVA            |
|---------------|-----------------|------------------|
| 1             | review_score    | review_score     |
| 2             | qntd_compras    | qntd_compras     |
| 3             | freight_value   | price            |
| 4             | price           | Dias_para_entrega|
| 5             | product_id      | order_id         |

* Após a implementação do modelo simples de identificação de frequência de compras em conjunto, foram feitas as análises:

|   Produto alvo / Produtos relacionados       |  19640   |
|----------|----------|
| 6939     | 29       |
| 31129    | 6        |
| 8063     | 2        |
| 30588    | 2        |
| 654      | 1        |

Em que para o produto codificado (19640) , pelo Label Encoder, é comprado com uma maior frequência (29 vezes) em conjunto com o produto 6939, e indica que os produtos caso estejam presentes em promoções/recomendações possuem uma chance maior de serem comprados em conjunto.
