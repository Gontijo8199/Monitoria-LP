Todas as funções deverão conter docstrings (no padrão NumPy/SciPy ou Google), type hints e testes unitários. 

Façam, idealmente, os exercícios no papel e lápis, alinhando a prática com como será a avaliação.

# Exercício 1 - Sistema de Empréstimos

Uma biblioteca deseja controlar seus exemplares e empréstimos. Implemente as classes `Exemplar` e `Biblioteca`.

**a)** Crie `Exemplar(codigo, titulo, autor)`, com os atributos correspondentes e um atributo `emprestado`, inicialmente `False`. Os campos textuais devem ser strings não vazias. Levante `TypeError` para tipos inadequados e `ValueError` para strings vazias.

**b)** Crie `Biblioteca(nome)`, com os métodos:

- `cadastrar(exemplar)`: adiciona um exemplar. Não permita códigos repetidos.
- `buscar(codigo)`: retorna o exemplar correspondente ou levanta `KeyError`.
- `disponiveis()`: retorna os exemplares que não estão emprestados.

**c)** Implemente `emprestar(codigo)` e `devolver(codigo)`. Não permita emprestar um exemplar já emprestado nem devolver um exemplar que já esteja disponível. Reutilize `buscar` nesses métodos.

**d)** Implemente `relatorio()`, que retorna um dicionário com as quantidades de exemplares cadastrados, emprestados e disponíveis. Teste também as situações de erro.

Exemplo:

```python
biblioteca = Biblioteca("Biblioteca EMAp")
biblioteca.cadastrar(Exemplar("L01", "Cálculo", "Autor A"))
biblioteca.emprestar("L01")
print(biblioteca.relatorio())
# {'total': 1, 'emprestados': 1, 'disponiveis': 0}
```

# Exercício 2 - Estação de Monitoramento

Uma estação registra temperatura e umidade. Implemente a classe `Estacao`, utilizando `@property` para validar as medições e consultar estatísticas.

**a)** O construtor recebe `identificacao`, `temperatura` e `umidade`. A temperatura deve estar entre -10 e 50 °C, e a umidade entre 0 e 100%. Crie propriedades com setters para validar toda alteração. Rejeite tipos inadequados com `TypeError` e valores fora dos intervalos com `ValueError` (não aceite `bool` como número).

**b)** Mantenha históricos separados para temperatura e umidade. As propriedades `historico_temperatura` e `historico_umidade` devem retornar tuplas, sem permitir alterações externas.

**c)** Implemente as propriedades somente leitura `temperatura_media`, `umidade_media` e `amplitude_termica` (maior temperatura menos menor temperatura). Se não houver medições, retorne `None`.

**d)** Implemente `comparar(outra_estacao)`, que retorna a diferença entre as temperaturas médias. Levante `TypeError` se o argumento não for uma `Estacao` e `ValueError` se alguma média ainda não puder ser calculada.

Exemplo:

```python
estacao = Estacao("A", 24, 60)
estacao.temperatura = 28
print(estacao.temperatura_media)  # 26.0
print(estacao.historico_temperatura)  # (24, 28)
```

# Exercício 3 - Folha de Pagamento

Uma empresa possui funcionários efetivos e horistas. Todos têm nome e matrícula, mas o cálculo do pagamento depende do tipo de vínculo. Implemente as classes abaixo utilizando herança.

**a)** Crie a classe `Funcionario(nome, matricula)`, com o método `calcular_pagamento()`. A classe deve servir como base para as categorias de funcionários.

**b)** Crie `Efetivo`, que recebe também `salario_base` e `bonus`. Seu pagamento é a soma desses valores. Utilize `super()` para inicializar os atributos comuns.

**c)** Crie `Horista`, que recebe `valor_hora` e `horas_trabalhadas`. Seu pagamento é o produto desses valores. Implemente também `adicionar_horas(quantidade)`. Valores monetários e horas devem ser não negativos; a quantidade adicionada deve ser positiva.

**d)** Implemente `gerar_folha(funcionarios)`, que retorna um dicionário associando cada matrícula ao pagamento. Não use condicionais para identificar a categoria: utilize o método `calcular_pagamento()`. Crie ainda uma terceira categoria de funcionário e teste-a sem alterar `gerar_folha`.

Exemplo:

```python
funcionarios = [
    Efetivo("Ana", "F01", 5000, 500),
    Horista("Bruno", "H01", 50, 120)
]
print(gerar_folha(funcionarios))
# {'F01': 5500, 'H01': 6000}
```

# Exercício 4 - Modalidades de Entrega

Uma loja oferece diferentes modalidades de entrega. Cada modalidade calcula seu próprio custo e prazo. Implemente o sistema utilizando classes abstratas e polimorfismo.

**a)** Crie a classe abstrata `Entrega(distancia, peso)`, com os métodos abstratos `calcular_custo()` e `prazo_estimado()`. Distância e peso devem ser positivos. Utilize `ABC` e `abstractmethod`.

**b)** Implemente as subclasses:

- `EntregaPadrao`: R\$ 5,00 por quilômetro + R\$ 2,00 por quilograma; prazo de 5 dias.
- `EntregaExpressa`: R\$ 8,00 por quilômetro + R\$ 3,00 por quilograma; prazo de 2 dias.
- `Retirada`: custo zero e prazo de 1 dia.

**c)** Implemente `resumir_entregas(entregas)`, que retorna uma lista de dicionários com custo e prazo de cada entrega. A função deve utilizar os métodos da interface, sem identificar a classe concreta.

**d)** Crie `EntregaAgendada`, com o mesmo custo por quilômetro da modalidade padrão, mais uma taxa fixa de R$ 12,00, e prazo de 3 dias. Demonstre que ela funciona com `resumir_entregas` sem modificar essa função. Explique o que ocorre ao tentar instanciar uma classe que não implementa todos os métodos abstratos.

Exemplo:

```python
entregas = [EntregaPadrao(10, 2), EntregaExpressa(10, 2), Retirada(10, 2)]
print(resumir_entregas(entregas))
# [{'custo': 54, 'prazo': 5}, {'custo': 86, 'prazo': 2}, {'custo': 0, 'prazo': 1}]
```

# Exercício 5 - Contas Bancárias

Implemente a classe `ContaBancaria`, utilizando atributos de classe, encapsulamento, métodos de classe e métodos estáticos.

**a)** Cada conta deve ter `numero`, `titular` e `saldo`. O saldo deve ser protegido contra alterações diretas e consultável por uma propriedade somente leitura. Mantenha também um atributo de classe que registre quantas contas foram criadas.

**b)** Implemente os métodos `depositar(valor)` e `sacar(valor)`. Não permita operações com valores não positivos nem saques superiores ao saldo disponível.

**c)** Implemente um método de classe `quantidade_contas()` para consultar o total de contas criadas e um método estático `validar_numero(numero)` para verificar se o número da conta é válido.

**d)** Implemente `transferir(destino, valor)`, que transfere dinheiro entre duas contas. Valide o destino e garanta que uma transferência inválida não altere o saldo de nenhuma das contas.

Exemplo:

```python
conta_a = ContaBancaria("001", "Ana", 100)
conta_b = ContaBancaria("002", "Bruno", 50)

conta_a.transferir(conta_b, 30)print(conta_a.saldo, conta_b.saldo)  # 70, 80
print(ContaBancaria.quantidade_contas())  # 2
```

# Exercício 6 - Decorador de Cronômetro

Implemente o decorador `@cronometrar` para calcular o tempo de execução de qualquer função utilizando o módulo `time`. Todas as vezes que a função decorada for chamada, o decorador deve fazer com que a mensagem `"Função '{nome_da_funcao}' executada em {tempo} ms"` seja impressa no terminal.

Ele precisa suportar funções com qualquer quantidade de argumentos posicionais (`*args`) e nomeados (`**kwargs`), retornando corretamente o valor gerado pela função original. Além disso, garantar que a função decorada não perca seus metadados originais (como `__name__` e `__doc__`).

Exemplo:

```python
@cronometrar
def processar_dados(n: int) -> list:
    return [i * 2 for i in range(n)]

resultado = processar_dados(100000)

# Saída no terminal: Função 'processar_dados' executada em 4.12 ms
```


# Exercício 7 - Logger de Execução com Controle de Colisão

Sistemas complexos precisam registrar (logar) o que acontece em tempo de execução, mas é necessário cuidado para que diferentes processos não sobrescrevam os logs uns dos outros. Implemente um decorador `@registrar_log` que, a cada chamada da função decorada, adicione uma linha no arquivo `log_{nome_da_funcao}.log`.

A linha deve seguir estritamente o formato: `[YYYY-MM-DD HH:MM:SS] função {nome_da_funcao} chamada com args {args} e kwargs {kwargs}, retornando {resultado}.` (utilize o módulo `datetime` para auxiliar na impressão do momento da execução).

Para evitar colisões, o decorador deve manter um registro interno dos nomes das funções já decoradas durante a execução do programa. Se o decorador for aplicado a uma segunda função que possua o mesmo `__name__` de uma função já registrada, ele deve impedir a sua criação e parar o programa com uma `Exception` com a mensagem: `"Uma função com o nome '{nome}' já possui um logger ativo."`

Exemplo:

```python
@registrar_log
def calcular_imposto(valor: float) -> float:
    return valor * 0.15

@registrar_log
def calcular_imposto(salario: float) -> float: # Deve levantar Exception aqui!
    return salario * 0.27

# Exemplo de chamada da função válida:
calcular_imposto(100.0)
calcular_imposto(valor=200.0)

# Conteúdo que deve ser gerado no arquivo "log_calcular_imposto.log":
# [2026-11-18 10:15:30] função calcular_imposto chamada com args (100.0,) e kwargs {}, retornando 15.0.
# [2026-11-18 10:15:32] função calcular_imposto chamada com args () e kwargs {"valor": 200.0}, retornando 30.0.
```

# Exercício 8 - Pipeline de Operações e Fábricas de Funções

Em ciência de dados, é comum aplicarmos uma sequência de transformações a um dado e manter o histórico dessas modificações. Também é comum reutilizar essas transformações em outros dados obtidos posteriormente ou em outras análises.

a) Implemente a função `criar_pipeline(operacoes: list[callable]) -> callable`, que recebe uma lista de funções matemáticas simples (**que aceitam e retornam um único número**) e retorna uma nova função. A função retornada receberá um `valor_inicial` e, quando chamada, aplicará cada operação da lista em sequência, passando o resultado de uma como entrada para a próxima. A saída deve ser uma tupla contendo o resultado final e uma memória de cálculo (uma lista de dicionários indicando o nome da operação aplicada e o valor resultante daquela etapa).

b) Implemente closures para gerar operações matemáticas genéricas fixando o segundo operando. Crie as funções `criar_somador(y: float)`, `criar_subtrator(y: float)`, `criar_multiplicador(y: float)` e `criar_divisor(y: float)`. Cada uma delas deve retornar uma nova função que recebe um único argumento `x` e realiza a respectiva operação matemática (x + y, x - y, x * y, x / y). Para que a memória de cálculo do item anterior fique clara, o atributo `__name__` de cada função retornada deve ser alterado internamente para refletir a operação e o valor (por exemplo, "somar_2", "multiplicar_3", "dividir_4").

Exemplo:

```python
somar_2 = criar_somador(2)
multiplicar_3 = criar_multiplicador(3)
subtrair_1 = criar_subtrator(1)

meu_pipeline = criar_pipeline([somar_2, multiplicar_3, subtrair_1])
resultado, memoria = meu_pipeline(5)

print(resultado) # 20
print(memoria) 
# [{"operacao": "somar_2", "resultado_parcial": 7}, 
#  {"operacao": "multiplicar_3", "resultado_parcial": 21}, 
#  {"operacao": "subtrair_1", "resultado_parcial": 20}]
```

# Exercício 9 - Pipeline de Operações com Objetos

Implemente a classe `Pipeline` para gerenciar uma sequência de operações matemáticas, permitindo que novas funções sejam adicionadas dinamicamente através de um decorador.

**a)** Crie um construtor que inicializa uma lista vazia para armazenar as operações.

**b)** Implemente o método registrar, que atuará como o decorador. Quando utilizado acima da definição de uma função, ele deve adicionar essa função à lista interna de operações da instância e retornar a própria função, inalterada.

**c)** Implemente o método `aplicar(valor_inicial)`, que passa o `valor_inicial` pela primeira função registrada, repassa o resultado para a segunda, e assim por diante. Assim como no exercício anterior, este método deve retornar uma tupla contendo o resultado final e a memória de cálculo (uma lista de dicionários indicando o nome da operação aplicada e o valor resultante daquela etapa).

Exemplo:
```python
processador = Pipeline()

@processador.registrar
def dobrar(x: float) -> float:
    return x * 2

@processador.registrar
def subtrair_tres(x: float) -> float:
    return x - 3

resultado, memoria = processador.aplicar(5)

print(resultado) # 7
print(memoria) 
# [{"operacao": "dobrar", "resultado_parcial": 10}, 
#  {"operacao": "subtrair_tres", "resultado_parcial": 7}]
```
