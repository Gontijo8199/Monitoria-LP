Todas as funções deverão conter docstrings (no padrão Numpy/SciPy ou Google), typehints e testes unitários (unittest, pytest ou doctest).

# Exercício 1 - Filtro de Candidatos por Competências

Você foi encarregado de criar um pequeno sistema de triagem de currículos para o RH. O sistema deve comparar as habilidades exigidas por uma vaga com as habilidades que cada candidato afirma ter.

Crie uma função chamada `filtrar_candidatos` que receba os seguintes parâmetros:

- Primeiro argumento posicional obrigatório: um conjunto (`set`) contendo as competências exigidas para a vaga.
- Segundo argumento posicional obrigatório: um dicionário (`dict`) onde a chave é o nome do candidato e o valor é uma tupla (`tuple`) contendo as competências que aquele candidato possui. As competências devem ser `strings` não vazias.

A função deve retornar um dicionário. A chave será o nome do candidato e o valor será uma tupla de dois elementos:

- Um booleano (`bool`) com valor True se o candidato possui todas as competências exigidas.
- Um conjunto contendo as competências exigidas que faltam para o candidato (se ele tiver todas, será um set vazio).

> **Nota:** Candidatos que possuam competências adicionais (que não foram exigidas pela vaga) não devem ser penalizados.

Se o argumento das competências exigidas não for do tipo conjunto, a função deve levantar um `TypeError` informando o erro. Se as competências de algum candidato do dicionário não forem do tipo tupla, a função também deve levantar um `TypeError`.

Se alguma das competências presentes no conjunto ou tuplas não for uma string, a função também deve levantar um `TypeError`. E se algum dos candidatos não possuir competências cadastradas (tupla vazia) ou alguma das strings for vazia (`""`), a função deve levantar um `ValueError`. 

Exemplo de uso:

```python
vaga = {"Python", "SQL", "Git"}
    
banco_talentos = {
    "ana": ("Python", "SQL", "Git", "Docker"),
    "bruno": ("Python", "Git"),
    "carla": ("Java", "SQL")
}

resultado = filtrar_candidatos(vaga, banco_talentos)
print(resultado)
```

Saída esperada:

```python
{
    'ana': (True, set()),
    'bruno': (False, {'SQL'}),
    'carla': (False, {'Git', 'Python'})
}
```

# Exercício 2 - Compressão de Sequências de DNA 
Em bioinformática, sequências de DNA frequentemente possuem repetições consecutivas das mesmas bases nitrogenadas (A, C, G e T). Uma forma simples de compactar esses dados é o algoritmo Run-Length Encoding (RLE), que substitui blocos de repetições pela base e sua respectiva contagem.

Crie duas funções:

### 1. comprimir_dna(sequencia)

Recebe uma string contendo a sequência de DNA e retorna uma lista de tuplas representando os blocos consecutivos.

Cada tupla é formada pelos componentes (base, quantidade), indicando qual a base e quantas vezes ela se repete. Caso a entrada seja uma string vazia, a função deve retornar uma lista vazia.

Caso a string de entrada não seja do tipo str, a função deve levantar um `TypeError`. Se a sequência contiver qualquer caractere que não seja uma das quatro bases válidas ('A', 'C', 'G', 'T') (trate letras minúsculas como inválidas), a função deve levantar um `ValueError`.

### 2. descomprimir_dna(dados_comprimidos)

Recebe uma lista de tuplas e reconstrói a sequência original de DNA. As tuplas da entrada seguem o mesmo formato das tuplas da saída da função anterior. Caso a entrada seja uma lista vazia, o retorno deve ser uma string vazia.

Se a entrada não for do tipo list ou os elementos não forem tuplas de tamanho 2, levante um `TypeError`. Se a base for inválida ou a quantidade não for um inteiro positivo (>0), levante um `ValueError`.

Ao final, espera-se que as funções sejam compatíveis entre si: 

```python
sequencia == descomprimir_dna(comprimir_dna(sequencia))

sequencia_comprimida == comprimir_dna(descomprimir_dna(sequencia_comprimida))
```

Exemplo de uso:
```python
dna = "AAACCGGGGTTTAA"

comprimido = comprimir_dna(dna)
print(comprimido)
# Saída: [('A', 3), ('C', 2), ('G', 4), ('T', 3), ('A', 2)]

original = descomprimir_dna(comprimido)
print(original)
# Saída: 'AAACCGGGGTTTAA'
```

# Exercício 3 - Delimitação de retângulos

Crie uma função chamada `calcular_caixa_delimitadora` que receba uma lista de pontos.

Cada ponto fornecido será uma tupla de dois números reais ou inteiros: (x, y). O objetivo é encontrar o menor e o maior valor de x, bem como o menor e o maior valor de y presentes entre todos os pontos fornecidos. Com esses valores, é possível encontrar um retângulo que contenha todos os pontos.

Retorno: A função deve retornar uma tupla contendo exatamente duas tuplas: `((min_x, min_y), (max_x, max_y))`.

Se a função for chamada com menos de dois pontos, ela deve levantar um `ValueError` com uma mensagem explicativa. Caso qualquer um dos argumentos não estejam de acordo com a especificação de ponto, a função deve levantar um `TypeError` com uma mensagem explicativa.

# Exercício 4 - Conversor de Temperaturas com Doctest

Crie uma função chamada `converter_temperatura` que receba três parâmetros:

- `valor` (`float` ou `int`): o valor numérico a ser convertido.
- `origem` (`str`): a unidade de origem, podendo ser `"C"` (Celsius), `"F"` (Fahrenheit) ou `"K"` (Kelvin).
- `destino` (`str`): a unidade de destino, com as mesmas opções acima.

A função deve retornar a temperatura convertida como `float`, arredondada para 2 casas decimais.

Fórmulas de conversão a serem utilizadas:

| De → Para | Fórmula |
|---|---|
| Celsius → Fahrenheit | `F = C * 9/5 + 32` |
| Fahrenheit → Celsius | `C = (F - 32) * 5/9` |
| Celsius → Kelvin | `K = C + 273.15` |
| Kelvin → Celsius | `C = K - 273.15` |
| Fahrenheit → Kelvin | `K = (F - 32) * 5/9 + 273.15` |
| Kelvin → Fahrenheit | `F = (K - 273.15) * 9/5 + 32` |

_Dica de implementação: em vez de tratar as 6 combinações separadamente, considere converter sempre para uma unidade intermediária (por exemplo, Celsius) e depois para o destino, isso reduz a duplicação de código._

**Requisitos desta questão:** a docstring da função deve seguir o padrão NumPy/SciPy (com seções `Parameters`, `Returns` e `Raises`) e conter exemplos executáveis via `doctest`, incluindo:
- Pelo menos duas conversões corretas, usando as fórmulas acima (ex: C→F e K→C).
- Pelo menos um exemplo que demonstre o levantamento de uma exceção (usando o formato de `Traceback` que o `doctest` reconhece).

Regras de validação:
- Se `origem` ou `destino` não estiverem entre as unidades válidas (`"C"`, `"F"`, `"K"`), levante `ValueError`.
- Se `valor` não for `int` nem `float` (nem `bool`, que deve ser rejeitado mesmo sendo subtipo de `int`), levante `TypeError`.
- Se `origem` e `destino` forem iguais, a função deve simplesmente retornar `valor` sem conversão (sem levantar erro).
- Observação sobre zero absoluto: Kelvin não admite valores negativos. Se `valor < 0` e `origem == "K"`, a função deve levantar `ValueError`, pois essa temperatura é fisicamente impossível.

Exemplo de uso:
```python
converter_temperatura(0, "C", "F")   # 32.0
converter_temperatura(212, "F", "C") # 100.0
converter_temperatura(0, "C", "K")   # 273.15
converter_temperatura(-10, "K", "C") # levanta ValueError
```

Ao final, seu arquivo deve poder ser validado rodando:

```python
import doctest
doctest.testmod(verbose=True)
```

e todos os exemplos da docstring devem passar.

# Exercício 5 - Suíte de Testes para Validador de Senhas (com `unittest`)

Desta vez, o foco é construir uma suíte de testes completa com o módulo `unittest` da biblioteca padrão.

Implemente a função `validar_senha(senha: str) -> dict`, que recebe uma senha e retorna um dicionário com o seguinte formato:

```python
{
    "valida": bool,
    "criterios_atendidos": set,   # ex: {"tamanho_minimo", "letra_maiuscula", "numero"}
    "criterios_faltantes": set
}
```

Critérios avaliados (cada um vira uma string no set correspondente):
- `"tamanho_minimo"`: pelo menos 8 caracteres.
- `"letra_maiuscula"`: pelo menos uma letra maiúscula.
- `"letra_minuscula"`: pelo menos uma letra minúscula.
- `"numero"`: pelo menos um dígito.
- `"caractere_especial"`: pelo menos um caractere fora de `[a-zA-Z0-9]`.

`"valida"` é `True` somente se todos os 5 critérios forem atendidos. Se `senha` não for `str`, levante `TypeError`. Se `senha` for uma string vazia, levante `ValueError`.

**Formatos e estruturas sugeridas:** um arquivo `test_validador_senha.py` contendo uma classe de testes que herde de `unittest.TestCase`, com no mínimo:

1. `setUp`: um método que prepare uma lista de senhas "fracas" reutilizável, atribuída a `self.senhas_fracas`, usada em pelo menos dois métodos de teste diferentes.

2. Testes parametrizados manualmente: crie um método de teste que itere sobre uma lista de pelo menos 5 tuplas `(senha, criterios_atendidos_esperado)` definida dentro do próprio teste, usando `self.subTest(senha=senha)` a cada iteração para que falhas sejam reportadas individualmente. Misture casos válidos e inválidos, e verifique o conteúdo de `criterios_atendidos`/`criterios_faltantes`, não apenas o valor de `"valida"`.

3. Testes de exceção: um teste isolado para cada tipo de exceção, usando `self.assertRaises(TipoDoErro)` como *context manager*, e verificando a mensagem de erro com `assertRaisesRegex` (ou o argumento `msg`/atributo `.exception` capturado do context manager).

4. Casos de borda: um teste que verifique explicitamente uma senha com exatamente 8 caracteres (limite mínimo) e outra com 7 caracteres, usando `assertEqual`/`assertIn`/`assertNotIn` conforme apropriado.

5. Bloco de execução: o arquivo deve terminar com:
```python
if __name__ == "__main__":
    unittest.main()
```

Ao final, o comando `python -m unittest test_validador_senha.py -v` (ou simplesmente `python test_validador_senha.py`) deve rodar todos os testes sem falhas.
