# Lista 2 - Linguagens de programação

**Objetivo Geral:** Revisar Funções (funções como objetos, funções anônimas, closures e decoradores) e tratamento de erros.

## Organização dos arquivos e questões

Todas as funções pedidas devem ser implementadas em um único arquivo `respostas.py`.

> ⚠️ Aviso: Siga exatamente os nomes dos arquivos e funções apresentados neste README, incluindo letras maiúsculas e minúsculas. O não cumprimento dessa regra poderá resultar em penalidade na nota.

## Questão 1
**a)** Implemente uma função geradora de polinômios `gerar_polinomio`. Sua função deve receber um número arbitrário de argumentos posicionais ($n_1,n_2,n_3,...$) e o retorno dela deverá ser outra função, que recebe qualquer número real e retorna o valor do polinômio $n_1+n_2x+n_3x^2+n_4x^3+...$. 

Exemplo:

```py
f = gerar_polinomio(5, 6, 2)
f(2) #retorna 25 (5 + 6*2 + 2*4)
f(3) #retorna 41 (5 + 6*3 + 2*9)
g = gerar_polinomio(2, 4, 3, 3)
g(2) #retorna 46 (2 + 4*2 + 3*4 + 3*8)
```

Faça o nome da função retornada (você pode inspecioná-lo com `func.__name__`) ser `polinomio_grau_n`, sendo que `n` deverá corresponder ao grau do polinômio.

Exemplo:

```py 
f = gerar_polinomio(5, 6, 7)
print(f.__name__) #printa polinomio_grau_2
g = gerar_polinomio(5, 3, 2, 5, 4, 3)
print(g.__name__) #printa 5
```

A função `gerar_polinomio` deverá lançar um `TypeError` caso algum dos argumentos não seja um número inteiro ou caso nenhum argumento seja passado.

A função retornada pela função `gerar_polinomio` deverá lançar um `TypeError` caso o argumento fornecido não seja um valor numérico (ou seja, se não for do tipo `int`, `float` ou `complex`).

## Questão 2
Implemente um **sistema de registro e validação de funções matemáticas**.  

Antes de começar, lembre-se que é possível criar **exceções personalizadas** em Python herdando da classe `Exception`. Por exemplo:  

```py
class MinhaExcecao(Exception):
    pass

raise MinhaExcecao("Ocorreu um erro específico!")
```

No exercício, você deve criar exceções personalizadas para diferentes situações de erro, como entradas inválidas, nomes repetidos ou funções inexistentes.

> Não se preocupe em entender o que significa a keyword `class` ou o que é herdar de uma classe. Você só precisa usar a sintaxe fornecida para criar uma exceção personalizada.


### a) O decorador `registrar_funcao(func)`

O decorador deve:

* Adicionar a função decorada ao dicionário global `funcoes_registradas`, usando seu nome como chave.
* Garantir que funções com o mesmo nome **não sejam sobrescritas** → Caso já haja uma função com o nome registrada, lançar `FuncaoJaRegistradaError`.
* Garantir que todos os argumentos posicionais passados para a função são todos numéricos (floats ou ints), de forma que a função não precise verificar isso internamente. Caso uma entrada passada não seja numérica, lançar `ArgumentoInvalidoError`

Exemplo de uso:

```py
@registrar_funcao
def quadrado(x):
    return x * x

quadrado(5) #retorna 25
quadrado(1.41) #retorna 1.988
print(quadrado("oi")) #lança a exceção ArgumentoInvalidoError

@registrar_funcao
def quadrado(x):
    return x ** 0.5
#lança a exceção FuncaoJaRegistradaError

@registrar_funcao
def dividir(x, y):
    return x / y

dividir(9, 3) #retorna 3.0
dividir(5, "olá") #lança a exceção ArgumentoInvalidoError
```

---

### b) A função `aplicar(nome: str, valores: list[tuple])`

A função deve:

* Receber um **nome** (`str`) e uma lista de tuplas (`list[tuple]`).
* Procurar a função registrada correspondente ao nome. Se não existir, lançar `FuncaoNaoEncontradaError`.
* Aplicar a função em todos os elementos da lista. Cada tupla corresponde aos argumentos posicionais de uma invocação da função.
* Se alguma execução gerar erro (ex.: divisão por zero), o valor deve ser substituído por `None`, mas o programa deve continuar normalmente.

Exemplo de uso:

```py
aplicar("quadrado", [1, 2, 3, 4]) #retorna [1, 4, 9, 16]

aplicar("dividir", [(10, 2), (5, 1), (9, 0)]) #retorna [5.0, 2.5, None]

aplicar("multiplicar", [(2, 5), (4, 9)]) #lança a exceção FuncaoNaoEncontradaError
```

## Questão 3

Implemente um decorador `cooldown(tempo_milisegundos)` que impede que uma função seja executada várias vezes dentro de um curto intervalo de tempo.

A lógica deve ser a seguinte:

- A primeira vez que a função é chamada, ela deve ser executada normalmente.
- Se a mesma função for chamada novamente antes que `tempo_milisegundos` milisegundos tenham se passado desde a última execução bem-sucedida, a função não deve ser executada. Em vez disso, ela deve lançar uma exceção `CooldownError`. 
> Essa exceção deverá ter uma mensagem informando o tempo restante de cooldown.

```py
@cooldown(4)
def disparar_acao():
    print("Ação disparada com sucesso!")

disparar_acao() #executa corretamente
time.sleep(2)
disparar_acao() #lança a exceção CooldownEmAndamento. Ela deve ter uma mensagem contendo o tempo de cooldown restante

#-----------------
disparar_acao() #executa corretamente
time.sleep(6)
disparar_acao() #executa corretamente

```
