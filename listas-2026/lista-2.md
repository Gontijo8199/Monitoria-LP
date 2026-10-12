# Lista de Revisão de NumPy - Linguagens de Programação

# Exercício 1 - Avaliação de Modelos de Previsão

Você é cientista de dados e quer avaliar as predições de 4 modelos para o mesmo problema. Os valores reais estão em um array NumPy de shape `(1000,)`. As predições estão em um array de shape `(1000, 4)`, em que cada coluna corresponde a um modelo. Descubra qual modelo tem o menor MSE (Mean Squared Error), sem usar laços de repetição.

```python
import numpy as np

rng = np.random.default_rng(seed=42)
y_real = rng.random(1000)
previsoes = rng.random((1000, 4))
```

a) Execute `previsoes - y_real` e explique, em um comentário, por que dá erro (lembre que um vetor só se alinha a uma matriz com o mesmo número de colunas). Em seguida, transforme `y_real` em um vetor coluna com `.reshape` e calcule a diferença ao quadrado entre previsões e valores reais. Salve em `erros_quadrados`.

b) A partir de `erros_quadrados`, calcule o MSE de cada modelo separadamente. O resultado deve ser um array unidimensional com 4 elementos. Salve em `mse_modelos`. Dica: o parâmetro `axis`.

c) Encontre o índice (de 0 a 3) do modelo com o menor valor em `mse_modelos`. Dica: crie uma máscara com `np.min` e use-a para selecionar elementos de `np.arange(4)`.

# Exercício 2 - Cálculo de Scores

O código abaixo calcula um score ponderado para cada um de 100 vetores de 5 elementos, usando dois laços aninhados.

```python
import numpy as np

rng = np.random.default_rng(seed=42)
X = rng.random((100, 5))
pesos = np.array([0.1, 0.3, 0.2, 0.15, 0.25])

scores = np.zeros(100)

for i in range(X.shape[0]):
    soma = 0
    for j in range(X.shape[1]):
        soma += X[i, j] * pesos[j]
    scores[i] = soma

print("Cálculo finalizado.")
```

a) Reescreva apenas o bloco do laço `for`, eliminando o laço interno `for j` e substituindo-o por operações vetorizadas do NumPy. Mantenha o laço externo `for i`.

b) **Desafio:** substitua todos os laços por operações vetorizadas, usando broadcasting entre `X` e `pesos` e uma função estatística com `axis`.

c) Qual é o shape do resultado do item (b) e por que ele tem esse shape? Confira que as versões dos itens (a) e (b) produzem os mesmos scores: calcule o maior erro quadrático entre elas (`np.max`) e confirme que é praticamente zero.

# Exercício 3 - Normalização de Notas

A matriz `X` tem uma linha por aluno e uma coluna por prova (prova 1, prova 2 e prova 3).

```python
import numpy as np

rng = np.random.default_rng(seed=42)
X = rng.normal(loc=70, scale=10, size=(200, 3))
```

a) Normalize cada **coluna**: subtraia a média da coluna e divida pelo desvio padrão da coluna. Salve em `X_col`. Qual é o shape de `X.mean(axis=0)` e por que a subtração `X - X.mean(axis=0)` funciona?

b) Normalize cada **linha** (a média e o desvio de cada aluno). Salve em `X_lin`. A mesma expressão do item (a), trocando apenas o `axis`, dá erro. Explique, em um comentário, por que isso acontece (compare o shape de `X.mean(axis=1)` com o número de colunas de `X`) e corrija transformando a média e o desvio em vetores coluna com `.reshape`.

c) Imprima `X_col.mean(axis=0)` e `X_lin.std(axis=1)`. Os valores devem ser aproximadamente 0 e 1, respectivamente.

d) Se `X` fosse `(3, 3)`, a expressão que falhou no item (b) daria erro? Prediga o que aconteceria e por que isso seria um problema.

# Exercício 4 - Análise de Notas

A matriz `notas` guarda as notas de 6 alunos (linhas) em 4 provas (colunas).

```python
import numpy as np

rng = np.random.default_rng(seed=42)
notas = rng.integers(0, 101, size=(6, 4))
```

a) Crie `aprovados`, uma máscara booleana com `True` onde a nota é maior ou igual a 60. Quantos aprovados há no total? Quantos há em cada prova (coluna)?

b) Crie `notas_corrigidas`, uma cópia de `notas` (use `.copy()`) em que todo valor abaixo de 60 foi substituído por 60. Confira que `notas` não foi alterada.

c) Considere `sub = notas[:2, :2]` seguido de `sub[0, 0] = -1`. Prediga, antes de executar, o valor de `notas[0, 0]`. Depois repita o experimento com `sub = notas[:2, :2].copy()`. Explique a diferença entre os dois casos.

d) Para cada aluno, conte em quantas provas ele foi aprovado (some `aprovados` com o `axis` adequado). Depois, com uma comparação, obtenha uma máscara dos alunos aprovados nas 4 provas.

e) Use `np.any` para descobrir se alguém tirou 100 em alguma prova e `np.all` para confirmar que todas as notas são maiores ou iguais a 0.