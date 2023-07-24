# Titanic - Machine Learning from Disaster Part1

* Utilizando os [dados disponíveis no Kaggle](https://www.kaggle.com/competitions/titanic) para tentar prever a porcentagem de passageiros que sobreviveram ao desastre. A intenção é obter a maior acurácia possível.
<hr>

### Estrutura do projeto

**O projeto é composto por 2 arquivos csv, sendo eles:**
1. `train.csv` que contém os dados de treino dos modelos;
2. `teste.csv` que contém os dados de teste dos modelos;
<hr>

**Primeiramente, foi realizado a visualização e limpeza dos dados fornecidos.**
Para isso, utilizei as bibliotecas [**Pandas**](https://pandas.pydata.org/) e [**Ydata-profiling**](https://github.com/ydataai/ydata-profiling).
#

### Exploração
O arquivo _treino_ é composto pelas colunas **PassengerId - Survived - Pclass - Name - Sex - Age - SibSp -Parch	- Ticket - Fare	- Cabin -	Embarked**.

O arquivo *teste* é composto basicamente pelas **mesmas colunas**, com exceção da coluna **Survived**, pois é a variável alvo.

*  **OBS**: Nesse momento, **não utilizarei as colunas com valores textuais (objects), apenas numéricas**. As colunas textuais serão utilizadas numa exploração posterior, uma **parte 2**.
#
### Tratamento

As tabelas (ambas) possuem valores ausentes, sendo as colunas:
1) **Treino**: _Age, Cabin e Embarked_.
2) **Teste**: _Age, Cabin e Fare_.

#
Por isso, Os valores nulos de _Age_ e _Fare_ foram substituidos por sua **média** e os valores de _Embarked_ pela sua **moda**.

Colunas de **alta cardinalidade**, como _Name_, _Ticket_ e _Cabin_ foram removidos pois poderiam tornar o modelo menos generalizável. Logo, numa primeira análise eles foram removidos.

Como dito anteriormente, somente valores numéricos foram utilizados nessa análise, sendo assim as colunas com **valores textuais** foram **removidas**.

Com isso, os modelos serão utilizados a partir dos DataFrames com as seguintes colunas:
1) **Treino**: _PassengerId - Pclass - Age - SibSp - Parch - Fare_
2) **Teste**:  _PassengerId - Pclass - Age - SibSp - Parch - Fare_

#
### Seleção de modelos e Treinamento

* Para começar, vamos testar entre:

  * **Árvore de classificação**
    * https://scikit-learn.org/stable/modules/tree.html#classification
 *  **Classificação dos vizinhos mais próximos**
    * https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html#sklearn.neighbors.KNeighborsClassifier
 *  **Regressão Logística**
    * https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression

Dividi a base de treino em treino e validação com o [**train_test_split**](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html).

#

### Avaliação dos modelos

Depois de treinados e rodados os modelos, avaliamos seu desempenho através da [**acurácia**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) e [**matriz de confusão**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html).

* Com relação à **acurácia**, os resultados foram:
  
  * Árvore de classificação: 0.6169491525423729
  * KNeighborsClassifier: 0.6542372881355932
  * **Regressão Logística: 0.7254237288135593**

* Com relação à **matriz de confusão**, os resultados foram:
  
  * Árvore de classificação:````array([[125,  50],
       [ 63,  57]], dtype=int64)````
  * KNeighborsClassifier: ````array([[133,  42],
       [ 60,  60]], dtype=int64)````
  * **Regressão Logística: ````array([[156,  19],
       [ 62,  58]], dtype=int64)````**

<br />
Em ambos os métodos de avaliação, o modelo de **Regressão Logística** obteve o melhor rendimento. Por isso, foi escolhido para a previsão com a _base de teste_.

#

O modelo então foi rodado e uma nova tabela contendo o _PassengerId_ e a predição (que ficou armazenada em uma coluna chamada de _Survived_, cujos valores assumiram 0 ou 1) foi criada.

A **acurácia** da predição com a base de teste foi de **0.66746**.

#
Isso mostra que o modelo ainda está longe de ser considerado bom, o que possivelmente é consequência da exclusão de muitas colunas dos dados, tornando-os pouco preditivos.
Posteriormente, uma nova análise será feita para que possa utilziar as colunas que foram removidas a fim de testar se a acurácia será maior.
