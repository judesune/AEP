# 🌱 EcoLog Alimentos

### Sistema Logístico de Resgate e Redistribuição de Excedentes Alimentares

O **EcoLog Alimentos** é um projeto acadêmico desenvolvido no curso de **Análise e Desenvolvimento de Sistemas da UniCesumar**.

O sistema tem como objetivo auxiliar na organização do **resgate, transporte e redistribuição de alimentos excedentes**, conectando pontos comerciais que possuem alimentos disponíveis para doação a instituições receptoras.

A proposta busca reduzir o desperdício de alimentos e tornar o processo de distribuição mais eficiente, priorizando produtos próximos da validade e alimentos que necessitam de refrigeração.

---

## 🎯 Problema

Diariamente, alimentos ainda próprios para consumo são descartados por supermercados, feiras, cooperativas e outros estabelecimentos por estarem próximos do término de sua validade comercial.

Ao mesmo tempo, instituições assistenciais enfrentam dificuldades para localizar esses alimentos e organizar sua coleta antes que eles se deteriorem.

O **EcoLog Alimentos** busca solucionar esse problema através de uma plataforma responsável por organizar as informações e controlar o processo logístico de coleta e entrega.

---

## 🌎 ODS 2 — Fome Zero e Agricultura Sustentável

O projeto está alinhado ao **Objetivo de Desenvolvimento Sustentável 2 da ONU**, com foco na **Meta 2.1**:

> Erradicar a fome e garantir o acesso de todas as pessoas a alimentos seguros, nutritivos e suficientes durante todo o ano.

O EcoLog contribui para esse objetivo ao facilitar o reaproveitamento e a redistribuição de alimentos que poderiam ser desperdiçados.

---

## 💡 Solução Proposta

O sistema permite gerenciar toda a cadeia de resgate de alimentos.

O fluxo principal funciona da seguinte forma:

```text
Ponto de Coleta
      │
      ▼
Registro dos Lotes
      │
      ▼
Ordem de Transporte
      │
      ▼
Priorização dos Alimentos
      │
      ▼
Transporte
      │
      ▼
Instituição Receptora
```

Alimentos com **menor prazo de validade** ou que necessitam de **cadeia refrigerada** recebem maior prioridade no transporte.

---

## ⚙️ Funcionalidades

Entre as principais funcionalidades previstas estão:

* Cadastro de pontos de coleta parceiros.
* Cadastro de instituições receptoras.
* Cadastro de transportadores parceiros.
* Registro de lotes de alimentos disponíveis para resgate.
* Classificação entre alimentos perecíveis e não perecíveis.
* Criação de ordens de transporte.
* Associação de vários lotes a uma ordem.
* Definição do ponto de coleta de origem.
* Definição da instituição receptora de destino.
* Atualização do status da ordem.
* Cancelamento de ordens pendentes.
* Priorização automática dos lotes para transporte.
* Controle de produtos que necessitam de refrigeração.

---

## 🚚 Status das Ordens

Uma ordem de transporte poderá possuir os seguintes estados:

```text
Pendente
   │
   ▼
Em Trânsito
   │
   ▼
Concluída
```

Também será possível alterar uma ordem para:

```text
Cancelada
```

O cancelamento deverá possuir uma justificativa operacional.

---

## 🧠 Regras de Priorização

Cada lote possui uma prioridade de transporte.

A prioridade poderá considerar fatores como:

* Data de validade.
* Quantidade de dias restantes até o vencimento.
* Necessidade de refrigeração.
* Faixa de temperatura.
* Tipo de alimento.
* Tipo de embalagem.

Os lotes perecíveis possuem regras específicas e podem receber prioridade superior aos alimentos não perecíveis.

---

# 🏗️ Arquitetura

O sistema utiliza uma arquitetura em camadas:

```text
Controller
    │
    ▼
 Service
    │
    ▼
Repository
    │
    ▼
  Model
    │
    ▼
  MySQL
```

### Controller

Responsável por receber as solicitações e encaminhá-las para a camada de serviço.

### Service

Responsável pelas regras de negócio, validações e operações do sistema.

### Repository

Responsável pela comunicação e persistência dos dados no banco.

### Model

Contém as entidades e objetos que representam o domínio do sistema.

---

# 🧩 Programação Orientada a Objetos

O projeto utiliza conceitos importantes de **POO**.

## Encapsulamento

Os atributos das entidades são protegidos e manipulados através de métodos.

## Abstração

`LoteAlimento` funciona como uma classe abstrata contendo informações comuns a todos os tipos de alimento.

```java
public abstract int calcularPrioridadeTransporte();
```

## Herança

Existem dois tipos principais de lote:

```text
             LoteAlimento
             <<abstract>>
                /     \
               /       \
              ▼         ▼
    LotePerecivel   LoteNaoPerecivel
```

## Polimorfismo

Cada tipo de lote possui sua própria implementação do método:

```java
calcularPrioridadeTransporte();
```

Dessa forma, cada alimento pode determinar sua prioridade utilizando regras específicas.

## Composição

Uma `OrdemTransporte` possui **um ou mais lotes de alimentos**.

```text
OrdemTransporte
      ◆
      │
      │ 1:N
      ▼
 LoteAlimento
```

---

# 📦 Principais Entidades

### `PontoColeta`

Representa empresas ou locais que disponibilizam alimentos excedentes.

Principais dados:

* Razão social
* CNPJ
* Endereço
* Telefone
* E-mail

---

### `InstituicaoReceptora`

Representa organizações responsáveis por receber os alimentos.

Principais dados:

* Razão social
* CNPJ
* Endereço
* Capacidade de armazenamento
* Capacidade refrigerada
* Telefone
* E-mail
* Responsável institucional

---

### `Transportador`

Representa o parceiro responsável pelo transporte dos alimentos.

Principais dados:

* Razão social
* CNPJ ou CPF
* Tipo de veículo
* Capacidade de carga
* Motorista responsável
* Telefone
* E-mail

---

### `OrdemTransporte`

Entidade principal do sistema.

Responsável por relacionar:

```text
PontoColeta
     │
     ▼
OrdemTransporte
     │
     ▼
InstituicaoReceptora
```

Também contém os lotes transportados e informações como data e status da operação.

---

### `LoteAlimento`

Classe abstrata utilizada como base para os diferentes tipos de alimentos.

Contém:

* Descrição
* Categoria
* Quantidade
* Data de validade

---

### `LotePerecivel`

Representa alimentos que necessitam de maior controle de conservação.

Possui informações adicionais como:

* Temperatura mínima.
* Temperatura máxima.

---

### `LoteNaoPerecivel`

Representa alimentos que não necessitam de cadeia refrigerada.

Possui informações como:

* Tipo de embalagem.

---

# 🗂️ Diagrama de Classes

O diagrama UML representa as principais entidades e seus relacionamentos.

![Diagrama de Classes](https://github.com/judesune/EcoLog-Alimentos/blob/main/docs/Diagrama%20de%20Classes%20UML.pdf)

---

# 🗄️ Banco de Dados

O sistema utiliza **MySQL 8.0** para persistência dos dados.

Principais tabelas:

```text
tb_ponto_coleta
tb_instituicao_receptora
tb_ordem_transporte
tb_lote_alimento
```

As tabelas utilizam **Primary Keys** e **Foreign Keys** para garantir a integridade dos relacionamentos.

Os lotes estão vinculados às ordens de transporte através de uma chave estrangeira.

```text
tb_ordem_transporte
        │
        │ 1:N
        ▼
tb_lote_alimento
```

A exclusão de uma ordem poderá remover seus lotes relacionados através de:

```sql
ON DELETE CASCADE
```

---

# 🗃️ Diagrama Entidade-Relacionamento

![DER](https://github.com/judesune/EcoLog-Alimentos/blob/main/docs/DER.pdf)

---

# 🛠️ Tecnologias

### Backend

* Java
* JDK 17 ou 21
* JDBC

### Banco de Dados

* MySQL 8.0

### Modelagem

* UML
* Diagrama de Classes
* Diagrama Entidade-Relacionamento

### Controle de versão

* Git
* GitHub

---

# 📁 Estrutura do Projeto

```text
AEP/
│
├── src/
│   └── código-fonte Java
│
├── database/
│   └── schema.sql
│
├── docs/
│   ├── Diagrama de Classes UML.pdf
│   ├── DER.pdf
│   └── EcoLog Alimentos.pdf
│
├── README.md
│
└── .gitignore
```

---

# ▶️ Requisitos

Para executar o projeto será necessário possuir:

```text
Java JDK 17 ou 21
MySQL 8.0+
Git
```

O Java permite que o projeto seja executado em diferentes sistemas operacionais através da JVM:

* Windows
* Linux
* macOS

---

# 📅 Cronograma

| Data       | Atividade                                    | Etapa      | Responsável     |
| ---------- | -------------------------------------------- | ---------- | --------------- |
| 06/09/2026 | Definição da ODS 2.1, escopo e requisitos    | 1ª Entrega | Vitor           |
| 07/09/2026 | Modelagem do Diagrama de Classes UML e DER   | 1ª Entrega | João            |
| 10/09/2026 | Justificativa técnica, GitHub e documentação | 1ª Entrega | Juliano         |
| 15/10/2026 | Script MySQL e configuração JDBC             | 2ª Entrega | Juliano e João  |
| 25/10/2026 | Implementação das classes e conceitos de POO | 2ª Entrega | Vitor e João    |
| 25/10/2026 | Implementação do CRUD de OrdemTransporte     | 2ª Entrega | Vitor e Juliano |
| 30/10/2026 | Testes, homologação e revisão dos commits    | 2ª Entrega | Equipe          |

---

# 👨‍💻 Equipe

Projeto desenvolvido por:

* **João Gabriel Caraçato Pastorelli**
* **Juliano Rafael de Oliveira Hauari**
* **Vitor Gabriel da Silva Duarte**

Curso de **Análise e Desenvolvimento de Sistemas — UniCesumar**.

---

# 📚 Projeto Acadêmico

**Atividade de Estudo Programada (AEP)**
**4º Semestre — 2026.2**

O projeto possui finalidade acadêmica e busca aplicar conceitos de:

* Programação Orientada a Objetos.
* Modelagem UML.
* Banco de Dados Relacional.
* Arquitetura de Software.
* CRUD.
* Persistência de dados.
* Desenvolvimento colaborativo utilizando Git e GitHub.

---

## 🔗 Repositório

[GitHub — EcoLog Alimentos](https://github.com/judesune/EcoLog-Alimentos)
