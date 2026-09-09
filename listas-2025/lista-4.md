# Lista 4 - Linguagens de Programação: PortfolioBuilder OO (Python)

**Objetivo Geral:** Aplicar conceitos fundamentais de Programação Orientada a Objetos (Herança, Polimorfismo, Encapsulamento e Composição) para construir o motor de um sistema de portfólio profissional em Python.

## Organização dos arquivos

  * `src/`: Pasta (módulo) contendo todo o código-fonte da implementação (suas classes, interfaces/classes abstratas).
  * `modelagem.md`: Arquivo (na raiz) com as justificativas de design (detalhes ao final).
  * `main.py`: Um arquivo Python na raiz do projeto que importa de `src/` e demonstra o sistema funcionando.

## Enunciado da Atividade

Você foi contratado para modelar e implementar o motor de um sistema de gerenciamento de portfólio. A ideia é que cada profissional (o "Dono") tenha **um portfólio**, que é composto por diversas seções (educação, experiência, etc.).

Nesse sistema, existem dois "perfis" de acesso:

1.  **O Dono:** Deve ter controle total para criar, editar, reordenar e remover qualquer parte do seu portfólio.
2.  **O Recrutador (Visitante):** Deve poder apenas visualizar as informações do portfólio, sem qualquer possibilidade de alterá-las.

Além disso, o mesmo portfólio precisa ser "exportado" de formas diferentes, a depender da vontade de quem está exportando. Uma das opções é como um arquivo Markdown (`.md`) formal ou uma página HTML (`.html`).

Sua tarefa é usar POO para modelar e construir esse motor, com foco na flexibilidade (para adicionar novas seções e novos formatos de exportação) e na segurança (para garantir a diferença de acesso entre Dono e Recrutador).

Nosso objetivo aqui é que vocês usem o que aprenderam para modelar o sistema, pensar quando utilizar heranças, agregações, polimorfismo... Não existe solução única, sejam criativos.

> Não usem nenhum LLM para isso. Sei que da vontade, mas é importante saber pensar e criar coisas novas.

-----

## Componentes e Regras do Sistema

### 1\. O Portfólio 

  * A classe `Portfolio` é a entidade central. Ela deve ter um `nome_dono`. É assim que vamos identificar de quem é aquele portifolio. Opicionalmente, você pode também adicionar outras informações relevantes para um currículo (ex: idade)
  * Um `Portfolio` gerencia uma **coleção** de Seções, que têm caracteristicas comuns entre si. No mínimo, deve haver uma seção de educação, uma de experiências profissionais, uma de projetos e mais uma a sua escolha. 

### 2\. O Acesso 

Para controlar se quem está acessando a classe `Portfolio` é o dono ou o recrutador, vamos usar duas classes:

  * **`PortfolioManager` (Dono):** Deve poder alterar e visualizar a classe Portifolio
  * **`PortfolioView` (Recrutador):** Deve porder apeans visualizar a classe Portifolio

Ambas as classes devem recebe um objeto `Portfolio` em seu construtor.

### 3\. As Seções

O portfólio é composto por seções. O design deve facilitar a adição de novos tipos de seção no futuro sem quebrar o código existente.

  * **Características Comuns:** Toda seção, independentemente do seu tipo (Educação, Projetos, etc.), deve ser capaz de fornecer uma representação JSON de seu conteúdo (que será usada pelos formatadores). Como você garantirá esse "contrato" comum usando POO? Não deve ser Hard Coded.

  * **Seções Mínimas Obrigatórias (com atributos):**
    1.  **`SecaoEducacao`**: Deve conter `nome_curso` (str), `nome_instituicao` (str), `data` (str, ex: "2020-2024") e `informacoes_adicionais` (str, opcional)
    2.  **`SecaoExperiencia`**: Deve conter `nome_cargo` (str), `nome_empresa` (str), `data` (str, ex: "Jan 2022 - Presente") e `responsabilidades` (str ou lista de str).
    3.  **`SecaoProjetos`**: Deve conter `nome_projeto` (str), `descricao` (str), `data_inicio` (str), `data_fim` (str, pode ser "Atual") e `link` (str, opcional).
  * **Seção de Criatividade (Mínimo 1):**
      * O aluno **deve** criar e implementar pelo menos **uma** seção nova à sua escolha, com seus próprios atributos.
      * *Sugestões:* `SecaoHabilidades` (lista de skills), `SecaoCertificacoes` (curso, entidade), `SecaoIdiomas` (idioma, nível).

### 4\. Os Formatadores

O sistema deve ser capaz de "renderizar" um portfólio em diferentes formatos de saída.

  * **Características Comuns:** Ambos os formatadores realizam a mesma *ação* (formatar um portfólio), mas produzem *resultados* diferentes. Como você usará POO para modelar essa intercambialidade e permitir a fácil adição de novos formatos (ex: `FormatadorLATEX`) no futuro?
  * **Formatadores Mínimos:**
    1.  `FormatadorMarkdown`: Deve gerar um arquivo Markdown.
    2.  `FormatadorHTML`: Deve gerar um arquivo HTML.

-----

## 📦 Entrega Esperada

### 1\. Código-Fonte (`src/` e `main.py`)

  * **`src/`**: Todas as classes e (se usar) módulos de classes abstratas.
  * **`main.py` (Driver Code):** Este arquivo `main` (na raiz) deve demonstrar o fluxo completo do sistema. Nele, vocês devem **simular a criação do currículo de um dos integrantes do grupo**:
    1.  Criar um `Portfolio` com o nome do integrante.
    2.  Criar um `PortfolioManager` para ele.
    3.  Criar um `PortfolioView` para o *mesmo* portfólio.
    4.  Usar o *Manager* para adicionar as 3 seções mínimas e a 1 seção criativa, **com dados reais** (ou realistas) do integrante (ex: sua formação na faculdade, seus projetos da disciplina, etc.).
    5.  Usar o `FormatadorMarkdown` para salvar o portfólio em `portfolio.md` por meio da classe `PortfolioManager`. Lembre-se que o Dono também deve poder visualizar.
    6.  Usar o `FormatadorHTML` para salvar o portfólio em `portfolio.html` por meio da classe `PortfolioView`.

#### Exemplo de Fluxo para o `main.py`

Seu `main.py` deve seguir uma lógica similar a este pseudo-código, preenchido com os dados reais de um integrante:

```python
meu_portfolio = Portfolio(nome_dono="Nome do Aluno")
manager = PortfolioManager(meu_portfolio)
view = PortfolioView(meu_portfolio)

# Manipulação
# (Aqui, adicione as 3 seções mínimas e 1 criativa usando o manager)
#
# Aqui vocẽ terá 2 opções. Escolha entre elas (para justificar no modelagem.md):
#
# (Abordagem 1) Criar o objeto e adicionar ao Portifolio
# edu = SecaoEducacao(curso="...", ...)
# manager.adicionar_secao(edu)
#
# (Abordagem 2) delegar a criação do objeto para o Portifolio
# manager.adicionar_educacao(curso="...", ...)


print("--- Portfólio atualizado pelo Manager ---")

# 4. Exportação (Dono)
md_format = FormatadorMarkdown()
manager.exportar("portfolio_dono.md") #aqui vocẽ define se quer um metodo para html e outro para markdown, ou se quer passar como argumento, passar uma classe...
print("--- Manager exportou em Markdown por meio do Dono ---")

# 5. Exportação (Recrutador)
view.exportar("portfolio_recrutador.html") #aqui vocẽ define se quer um metodo para html e outro para markdown, ou se quer passar como argumento, passar uma classe...
print("--- View exportou em HTML pelo Recrutador ---")

# 6. Leitura e Demonstração de Encapsulamento
print(f"Nome do Dono (via View): {view.get_nome_dono()}")
```

### 2\. Justificativas (`modelagem.md`)

Este é o arquivo mais importante para a avaliação. Explique suas decisões de design. O foco não é o quê seu código faz, mas por que você o estruturou dessa forma. Explique suas decisões de design respondendo, no mínimo, às seguintes perguntas:

- Como você garantiu a "Característica Comum" de que toda seção (atual ou futura) pudesse fornecer uma representação JSON de seu conteúdo?
- Explique como seu design permite que o PortfolioManager e o PortfolioView usem qualquer formatador (HTML, Markdown, ou um futuro FormatadorLATEX). Como você modelou os formatadores e por que.
- Descreva a relação entre a classe Portfolio e as classes de Secao. Como elas interagem?
- Detalhe aonde foi usado Encapsulamento, Abstração, Herança, Agregação, COmposição, Polimorfismo... e detalhe o por que isso melhora o código. Caso tenha escolhido não usar, documente também.
-----

## 💡 Exemplos de Saída

Para um portfólio simples, as saídas esperadas seriam semelhantes a estas:

### Exemplo de Saída (`portfolio.md`)

```markdown
# Ana Silva
# (Outras informações que queiram colocar)

## 🎓 Educação
* Bacharelado em Ciência da Computação - Universidade Brasileira (2018 - 2022)

## 💼 Experiência Profissional
* Engenheira de Software Pleno - Tech Solutions (2022 - Presente)
  * Desenvolvimento de sistemas de larga escala.
  * Manutenção de sistemas de larga escala.

## 🚀 Projetos
* PortfolioBuilder (2025 - Atual)
  * Um construtor de portfólios feito em Python usando POO.
  * Link: github.com/exemplo
```

### HTML Customizavel (`portfolio.html`)

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portfólio - {{ nome_dono }}</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>{{ nome_dono }}</h1>

    <h2>Educação</h2>
    <div class="item">
        <div class="item-title">{{ edu.curso }}</div>
        <div class="item-subtitle">{{ edu.instituicao }}</div>
        <div class="item-date">{{ edu.periodo }}</div>
        <div class="item-description">{{ edu.descricao }}</div>
    </div>

    <h2>Experiência Profissional</h2>
    <div class="item">
        <div class="item-title">{{ exp.cargo }}</div>
        <div class="item-subtitle">{{ exp.empresa }}</div>
        <div class="item-date">{{ exp.periodo }}</div>
        <div class="item-description">{{ exp.responsabilidades }}</div>
    </div>

    <h2>Projetos</h2>
    <div class="projects-grid">
        {% for proj in projetos %}
        <div class="project">
            <div class="project-title">{{ proj.nome }}</div>
            <div class="project-date">{{ proj.data_inicio }}{% if proj.data_fim %} - {{ proj.data_fim }}{% endif %}</div>
            <div class="project-description">{{ proj.descricao }}</div>
            <a href="{{ proj.link }}" target="_blank">{{ proj.link_text or 'Ver mais' }}</a>
        </div>
    </div>
</body>
</html>
```

> Criamos um style.css e um template de HTMl para facilitar. Para editar, podem separar em blocos, preencher inteiro ou qualquer outra forma que preferirem. Altere apenas o que está como variável (ex: {{ edu.descricao }}).
> No final, o resultado deve ser algo como em `example.html`.
