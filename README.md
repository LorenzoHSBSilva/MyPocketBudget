# MyPocketBudget

O Projeto foi desenvolvido para a disciplina de **Projeto de Software**, baseado no aplicativo **Monefy**, com melhorias e funcionalidades adicionais.

---

## 🧭 Informativo do projeto

O **MyPocketBudget** é um aplicativo de gerenciamento de gastos pessoais estruturado em uma arquitetura web moderna e desacoplada:
* **Backend:** Construído em Python utilizando o framework **FastAPI**, fornecendo uma API REST estável, performática e totalmente auto-documentada.
* **Frontend:** Uma interface single-page (SPA) limpa e responsiva construída com **HTML5, CSS3 e JavaScript nativo**, integrada perfeitamente ao servidor por meio de requisições assíncronas (`fetch`).

---

## 🛠️ Padrões de Projeto (Design Patterns)

A arquitetura do sistema utiliza três padrões clássicos do GoF (*Gang of Four*) para garantir que o código seja modular, extensível e de fácil manutenção.

### 1. Prototype (Padrão Criacional)
* **Onde está no código:** `core/prototype.py` (na classe `GastoPrototype`).
* **Por que foi utilizado:** Em sistemas financeiros, os usuários repetem despesas constantemente (ex: faturas mensais, assinaturas). Em vez de forçar o usuário a preencher um formulário idêntico do zero, o padrão permite duplicar um gasto existente com apenas um clique na rota `/gastos/{id}/clonar`. 
* **Como funciona:** Ele utiliza a biblioteca interna `copy.deepcopy` do Python para clonar o objeto original na memória de forma segura, gerando um novo ID único (`uuid4`) e permitindo a aplicação de sobrescritas dinâmicas (`**overrides`), caso o usuário queira clonar alterando apenas o valor ou a data.

### 2. Bridge (Padrão Estrutural)
* **Onde está no código:** `core/bridge.py` (nas classes `GerenciadorBase`, `RepositorioGastosBase` e `RepositorioCategoriasBase`).
* **Por que foi utilizado:** Para separar completamente a **Abstração** (as regras de negócio de como gerenciar gastos e categorias) da sua **Implementação** (onde e como esses dados são salvos).
* **Como funciona:** O `GerenciadorBase` não sabe se os dados estão sendo gravados em arquivos, na memória ou em um banco de dados real. Ele apenas conversa com as interfaces abstratas. Atualmente, o sistema usa persistência em memória (`RepositorioGastosMemoria`). Graças ao Bridge, se no futuro o projeto migrar para um banco de dados real (como SQLite ou PostgreSQL), **nenhuma linha de código** da lógica de negócio ou das rotas do FastAPI precisará ser modificada; bastará plugar a nova implementação do repositório.

### 3. Strategy (Padrão Comportamental)
* **Onde está no código:** `core/strategy.py` (nas hierarquias de `OrdenacaoStrategy` e `FiltroStrategy`).
* **Por que foi utilizado:** Telas de histórico financeiro exigem múltiplos critérios de pesquisa (filtrar por data, por tipo, por categoria, buscar por texto, ordenar por valor crescente ou decrescente). Se usássemos estruturas tradicionais, o código seria infestado de blocos condicionais `if/else` gigantescos e difíceis de manter.
* **Como funciona:** Cada algoritmo de filtro e ordenação foi isolado em sua própria classe independente (ex: `FiltroPorCategoria`, `OrdenarPorValorDesc`). Elas compartilham uma interface comum e são resolvidas dinamicamente em um dicionário de registro (`ORDENACAO_MAP`), permitindo adicionar novas regras de ordenação ou filtragem a qualquer momento sem modificar o motor principal do sistema (respeitando o princípio Open/Closed do SOLID).

---

## 🧩 Programação Orientada a Objetos (OO)

### Herança

| Classe | Herda de |
|---|---|
| `GastoNormal` | `Gasto` |
| `GastoExtra` | `Gasto` |
| `GastoParcelado` | `Gasto` |

A superclasse abstrata `Gasto` (localizada em `models/gasto.py`) concentra os atributos comuns de qualquer despesa (como descrição, valor, data e categoria). A herança foi utilizada para reaproveitar esses atributos e métodos comuns, evitando a repetição desnecessária de código e estabelecendo uma relação clara de subtipo (onde um `GastoParcelado` **é um** `Gasto`).

### Polimorfismo

O polimorfismo de subtipo é amplamente explorado no sistema através de métodos abstratos definidos na classe pai `Gasto`:

```python
# Definidos na classe abstrata Gasto e implementados de forma diferente nas subclasses:
@abstractmethod
def tipo(self) -> str: ...

@abstractmethod
def valor_mensal(self) -> float: ...

@abstractmethod
def descricao_completa(self) -> str: ...

 #  MyPocketBudget


O Projeto foi desenvolvido para a disciplina de **Projeto de Software**, baseado no aplicativo **Monefy**, com melhorias e funcionalidades adicionais.


---


## Informativo do projeto


O **MyPocketBudget** é um aplicativo de gerenciamento de gastos pessoais desenvolvido em Python, com interface gráfica construída com **Tkinter** — biblioteca nativa do Python, sem necessidade de instalação externa.


---


## Programação Orientada a Objeto


---


### Herança


| Classe | Herda de |

|---|---|

| `GastoParcelado` | `Gasto` |

| `GastoExtra` | `Gasto` |


```python

class Gasto:  # superclasse

    def __init__(self, valor, descricao, categoria, data):

        self.valor = valor

        self.descricao = descricao

        self.categoria = categoria

        self.data = data


class GastoParcelado(Gasto):  # herda de Gasto

    def __init__(self, valor, descricao, parcelas, categoria, data):

        super().__init__(valor, descricao, categoria, data)

        self.parcelas = parcelas

        self.valor_parcela = valor / parcelas if parcelas > 0 else 0


class GastoExtra(Gasto):  # herda de Gasto

    def __init__(self, valor, descricao, categoria, data):

        super().__init__(valor, descricao, categoria, data)

```


**O Uso da Herança**


A herança está na superclasse Gasto e nas classes filhas GastoExtra e GastoParcelado, a herança foi utilizada aqui pois evita repetições desnecessárias no codigo e como GastoExtra e GastoParcelada são tipos de gasto, logo, as classes filhas podem reutilizar o metodo e atributos da classe pai.


---


###  Polimorfismo


```python

class GerenciadorDeGastos:

    def listar_gastos(self):

        for g in self.gastos:  # g pode ser Gasto, GastoParcelado ou GastoExtra

            print(g.descricao, "-", g.valor)

```


O método listar_gasto percorre todos os gasto e acessa g.descricao e g.valor e independetemente do tipo de gasto (objeto), o mesmo código funciona de igual maneira sem se importar com o tipo de gasto, ou seja os diferentes objetos são tratados da mesma forma. (Polimofismo de subtipo)


---




## Funcionalidades:


1- Criar Categorias Próprias — criação e exclusão de categorias personalizadas (100%)


2 - Gasto Extra — registro de gastos inesperados com destaque visual em laranja (100%)


3 - Especificação de Gastos — registro detalhado com valor, descrição e categoria (100%)


4 - Registro de Despesa Parcelada — parcelamento de 1x a 12x com acompanhamento (100%)


5 - Filtro de Gastos por Data — exibe gastos de uma data específica (100%)


6 - Alerta de Gastos Excedentes — popup automático ao ultrapassar o limite da categoria (100%)


7 - Busca de Gastos — localiza despesas pelo nome (100%)


8 - Editar Gastos — edição e remoção de gastos já registrados (100%)


9 - Ordenar Gastos — ordenação por valor ou por data (100%)


10 - Limite de Orçamento por Categoria — definição e edição de limite por categoria (100%)



---


## Onde rodar:


O projeto pode ser executado em qualquer IDE que suporte Python, como:


* VS Code

* PyCharm

* IDLE


Não é necessária a instalação de bibliotecas externas, pois o sistema utiliza apenas o Tkinter, que já vem com próprio Python. 
