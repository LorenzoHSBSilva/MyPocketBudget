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

A herança está na superclasse Gasto e nas classes filhas GastoExtra e GastoParcelado, a herança foi utilizada aqui pois evita repetições desnecessárias no codigo e como GastoExtra e GastoParcelada são tipos de gasto, logo, as subclasses poderia reutilizar o metodo e atributos da superclasse e serem sobrescritos com overriding.

---

###  Polimorfismo

```python
class GerenciadorDeGastos:
    def listar_gastos(self):
        for g in self.gastos:  # g pode ser Gasto, GastoParcelado ou GastoExtra
            print(g.descricao, "-", g.valor)
```

O método listar_gasto percorre todos os gasto e acessa g.descricao e g.valor e independetemente do tipo de gasto (objeto), o mesmmo código funciona de mesmo modo sem se importar com o tipo de gasto, ou seja os diferentes objetos são tratado da mesma forma.

---


## Funcionalidades:

## Criar Categorias Próprias — criação e exclusão de categorias personalizadas 100%
