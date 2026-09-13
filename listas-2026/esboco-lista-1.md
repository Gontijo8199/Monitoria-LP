Todas as funções deverão conter docstrings (no padrão Numpy/SciPy ou Google), typehints e testes unitários. Podem escolher entre unittest, pytest ou doctest como framework, a menos que o enunciado da questão exija um específico, mas usem o mesmo framework em todas as funções de um mesmo exercício, para manter a coesão.

Façam, idealmente, os exercícios no papel e lápis, alinhando a prática com como será a avaliação. 

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

Exemplo de uso:
```python
calcular_caixa_delimitadora((1, 5), (3, 2), (0, 8))
# Saída: ((0, 2), (3, 8))
```

# Exercício 4 - Conversor de Temperaturas (Doctest ou Pytest)

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

Podem escolher uma das duas opções abaixo para completar a questão.

Opção A, com doctest: a docstring da função deve seguir o padrão NumPy/SciPy (com seções Parameters, Returns e Raises) e conter exemplos executáveis via doctest, incluindo pelo menos duas conversões corretas usando as fórmulas acima (por exemplo C para F e K para C) e pelo menos um exemplo que demonstre o levantamento de uma exceção, usando o formato de Traceback que o doctest reconhece.

Ao final, deve ser possível validar rodando:
```python
import doctest
doctest.testmod(verbose=True)
```

Opção B, com pytest: a docstring pode ser descritiva, sem exemplos executáveis, mas vocês devem entregar um arquivo `test_converter_temperatura.py` contendo um teste com `@pytest.mark.parametrize` cobrindo pelo menos as 6 combinações de conversão da tabela acima, comparando o resultado com `pytest.approx` (cuidado com arredondamento); um teste para o caso `origem == destino`, sem conversão; e testes isolados com `pytest.raises` para cada tipo de exceção (`TypeError` e `ValueError`), incluindo o caso do Kelvin negativo.

Ao final, o comando `pytest -v test_converter_temperatura.py` deve rodar sem falhas.

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

Exemplo de uso:
```python
validar_senha("abc123")
# Saída: {'valida': False, 'criterios_atendidos': {'letra_minuscula', 'numero'}, 'criterios_faltantes': {'tamanho_minimo', 'letra_maiuscula', 'caractere_especial'}}
```

**Formatos e estruturas sugeridas:** um arquivo `test_validador_senha.py` contendo uma classe de testes que herde de `unittest.TestCase`, com no mínimo:

1. `setUp`: prepara uma lista de senhas "fracas" reutilizável, atribuída a `self.senhas_fracas`, usada em pelo menos dois métodos de teste.

2. Um teste que itere sobre uma lista de pelo menos 5 tuplas `(senha, criterios_atendidos_esperado)`, usando `self.subTest(senha=senha)` a cada iteração. Misture casos válidos e inválidos, e verifique o conteúdo de `criterios_atendidos`/`criterios_faltantes`, não apenas `"valida"`.

3. Um teste isolado para cada tipo de exceção, usando `self.assertRaises(TipoDoErro)` como context manager, verificando a mensagem de erro com `assertRaisesRegex`.

4. Um teste de caso de borda: senha com exatamente 8 caracteres e outra com 7.

5. Bloco de execução ao final do arquivo:
```python
if __name__ == "__main__":
    unittest.main()
```

Ao final, o comando `python -m unittest test_validador_senha.py -v` (ou simplesmente `python test_validador_senha.py`) deve rodar todos os testes sem falhas.

# Exercício 6 - Processamento de Pedidos

Uma loja registra seus pedidos em um arquivo de texto, um item por linha, no seguinte formato:

```
cliente;preco;quantidade
```

Exemplo de conteúdo (`pedidos.txt`):
```
Ana ;10.0;2
ana;5.5;1
Bruno;20.0;abc
Carla;;3
Bruno;15.0;1
```

Repare que "Ana " e "ana" devem ser tratados como o mesmo cliente, ignorando espaços nas bordas e diferenças de maiúsculas e minúsculas. Algumas linhas são malformadas: "Bruno;20.0;abc" tem uma quantidade que não é um número inteiro válido, e "Carla;;3" está sem preço.

a) Defina uma exceção `LinhaInvalidaError(Exception)`. Ela deve ser levantada por uma função auxiliar sempre que uma linha não puder virar um pedido válido, seja por não ter 3 campos separados por ponto e vírgula, seja por preco ou quantidade não poderem ser convertidos para float ou int.

b) Implemente `parsear_linha(linha: str) -> tuple[str, float, int]`. A função faz strip, split por ponto e vírgula e normalização do nome do cliente (strip e lower), converte preco e quantidade para os tipos corretos, e retorna a tupla (cliente, preco, quantidade). Se a linha for inválida, levante `LinhaInvalidaError` com uma mensagem que inclua a linha original.

c) Implemente `processar_pedidos(caminho_arquivo: str) -> dict[str, float]`. A função abre o arquivo (use `with open`), tenta converter cada linha usando `parsear_linha`, e acumula preco vezes quantidade no total do cliente correspondente. Linhas que levantarem `LinhaInvalidaError` devem ser ignoradas, sem interromper o processamento das demais, mas imprimindo um aviso com o número da linha e o motivo. Retorna um dicionário `{cliente: total_gasto}`.

Não utilizem `except` genérico nem `except Exception`. Capturem especificamente `LinhaInvalidaError`.

d) Implemente `ler_pedidos_interativo() -> dict[str, float]`, com o mesmo comportamento de `processar_pedidos`, mas lendo as linhas via `input` em um laço, até que o usuário digite a palavra `fim`. Reutilizem `parsear_linha`.

e) Escrevam testes criando um arquivo temporário antes de cada teste (por exemplo com `tempfile.NamedTemporaryFile` ou a fixture `tmp_path` do pytest), cobrindo: um arquivo só com linhas válidas, incluindo o mesmo cliente em duas linhas diferentes; um arquivo com linhas válidas e inválidas misturadas; e um teste isolado de `parsear_linha` para cada tipo de linha malformada.

Escolham `unittest` ou `pytest` para a suíte de testes desta questão.

# Exercício 7 - Validador de ISBN-13

Todo livro publicado possui um código ISBN-13, um identificador único de 13 dígitos, no formato XXX-X-XXXX-XXXX-Y, onde os 12 primeiros dígitos formam a base e o último (Y) é o dígito verificador, calculado a partir dos 12 anteriores.

Crie uma função chamada `validar_isbn13` que receba uma string e retorne True se o ISBN for válido e False caso contrário.

Pré-processamento: remova quaisquer hífens da entrada, considerando apenas os dígitos numéricos. Verifique se o código possui exatamente 13 dígitos, caso contrário ele deve ser considerado inválido.

Cálculo do dígito verificador: os 12 primeiros dígitos recebem pesos alternados, começando em 1 e alternando entre 1 e 3. Multiplique cada dígito pelo seu peso, some os resultados e aplique módulo 10 à soma. Se o resto for 0, o dígito verificador é 0, caso contrário é 10 menos o resto. Compare o dígito calculado com o último dígito do código original.

Exemplo:
```
ISBN: 978-3-16-148410-0
Base: 978316148410
Dígito calculado = 0
Resultado: ISBN válido (retorna True)

ISBN: 978-3-16-148410-7
Resultado: ISBN inválido (retorna False)
```

Se a entrada não for do tipo str, a função deve levantar um TypeError. A função deve conter docstring, typehints, e testes unitários cobrindo pelo menos um ISBN válido, um ISBN com dígito verificador incorreto, um ISBN com quantidade errada de dígitos e um ISBN contendo algum caractere não numérico além dos hífens.

# Exercício 8 - Conversão de Datas

Implemente uma função `converter_data` que receba uma string no formato "dd/mm/aaaa" e retorne uma tupla (dia, mes, ano), com os três valores como inteiros.

Caso a string não esteja no formato esperado (por exemplo, faltando alguma barra, ou algum dos campos não sendo numérico), a função deve levantar um ValueError.

Defina uma nova exceção chamada `DataInvalidaError`, que deve ser utilizada quando a string estiver no formato correto mas representar uma data inválida. Considerem inválido: mês fora do intervalo de 1 a 12; dia fora do intervalo válido para o mês informado (considerando anos bissextos para fevereiro); e ano fora de um intervalo razoável, por exemplo menor que 1900 ou maior que 2100.

Exemplo de uso:
```python
converter_data("29/02/2024")  # (29, 2, 2024), 2024 é bissexto
converter_data("29-02-2024")  # levanta ValueError, formato incorreto
converter_data("29/02/2023")  # levanta DataInvalidaError, 2023 não é bissexto
```

Escrevam testes cobrindo um caso válido, um caso que gere ValueError e um caso que gere DataInvalidaError, incluindo pelo menos um teste de ano bissexto.

# Exercício 9 - O Mistério da Mochila

Você está desenvolvendo o sistema de inventário de um RPG. Cada personagem deveria ter sua própria mochila, mas os jogadores estão reclamando que os itens de um personagem estão aparecendo na mochila de outro, e às vezes duas mochilas parecem ser exatamente a mesma. Abaixo está o trecho de código responsável por isso. Sem executá-lo, analise-o com atenção e responda às perguntas a seguir.

```python
def equipar_item(item, mochila=[]):
    mochila.append(item)
    return mochila


mochila_arqueiro = equipar_item("arco")
mochila_mago = equipar_item("grimorio")
mochila_arqueiro = equipar_item("flechas", mochila_arqueiro)
mochila_guerreiro = equipar_item("espada", [])
```

a) Qual é o conteúdo final de `mochila_arqueiro`, `mochila_mago` e `mochila_guerreiro` depois dessas quatro chamadas? Justifiquem a ordem dos itens em cada mochila, considerando a ordem em que as chamadas acontecem.

b) A expressão `mochila_arqueiro is mochila_mago` resulta em True ou False? Expliquem o que essa comparação revela sobre os dois nomes, e por que `mochila_guerreiro` não sofre do mesmo problema.

c) Em que momento o valor padrão `[]` de `equipar_item` é criado, e por que ele persiste entre chamadas diferentes da função? Por que passar `[]` explicitamente na chamada de `mochila_guerreiro` evita o problema?

d) Reescrevam `equipar_item` para que cada chamada sem o segundo argumento comece sempre com uma mochila vazia nova, mantendo a possibilidade de passar uma mochila existente para a função.

e) Um colega propõe a seguinte correção, alegando que resolve o problema:

```python
def equipar_item(item, mochila=None):
    if not mochila:
        mochila = []
    mochila.append(item)
    return mochila
```

Essa correção resolve o bug original? Existe algum caso de uso em que ela se comporta de forma diferente do esperado, mesmo sem levantar nenhum erro? Expliquem, considerando o que acontece quando alguém chama a função passando uma mochila que já existe, mas está vazia no momento da chamada.