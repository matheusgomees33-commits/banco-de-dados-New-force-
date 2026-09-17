# Sistema de Cadastro da Academia

Projeto de modelagem de banco de dados para um sistema de gestão de academia (cadastro de alunos, matrículas, planos, pagamentos, produtos, avaliações físicas, fichas de treino, exercícios, máquinas, modalidades e turmas), inspirado no sistema **NextFit**.

> ✏️ **Banco de Dados:** Modelagem de Banco de Dados, Analise E Desenvolvimento de Sistemas, UNICID e 2°Semestre.

## Integrantes do grupo

| Nome | RGM |
|------|-----------|
| ✏️ Cauan Santos Patti       | 44889046
| ✏️ Nathan Martins Custodio  | 46998519
| ✏️ Matheus Gomes Lauretino  | 47250895
| ✏️ Vitor Ribeiro de Morais  | 46865900
| ✏️ Pedro Macario dos Santos Souza | 47291931

---

## Sumário

1. [Contexto e Justificativa](#1-contexto-e-justificativa)
2. [Objetivo do Sistema](#2-objetivo-do-sistema)
3. [Levantamento de Requisitos](#3-levantamento-de-requisitos)
4. [Regras de Negócio](#4-regras-de-negócio)
5. [Modelagem de Dados (DER)](#5-modelagem-de-dados-der)
6. [Dicionário de Dados](#6-dicionário-de-dados)
7. [Fluxogramas de Processos](#7-fluxogramas-de-processos)
8. [Justificativa Técnica das Decisões de Modelagem](#8-justificativa-técnica-das-decisões-de-modelagem)
9. [Uso de Inteligência Artificial](#9-uso-de-inteligência-artificial)
10. [Estrutura do Repositório](#10-estrutura-do-repositório)

---

## 1. Contexto e Justificativa

O projeto tem como tema uma **academia de ginástica e musculação** de médio porte, que oferece planos de matrícula, modalidades variadas (musculação, funcional, dança, entre outras) e turmas com horários fixos ao longo da semana.

Assim como a maioria das academias desse porte, a gestão administrativa envolve o controle de alunos e matrículas, a cobrança de mensalidades, o acompanhamento da evolução física dos alunos por meio de avaliações e fichas de treino, o controle de check-ins (entrada livre ou em turma) e a venda de produtos complementares na recepção (suplementos, acessórios, vestuário).

A escolha do tema se justifica por reunir, em um único domínio, processos transacionais (matrículas, pagamentos, vendas) e processos de acompanhamento (avaliações físicas, fichas de treino), o que exige uma modelagem de dados rica em relacionamentos — incluindo chaves estrangeiras opcionais e tabelas associativas — tornando-o um bom exercício de modelagem relacional. A estrutura conceitual foi baseada no funcionamento de sistemas de gestão de academias comerciais, como o **NextFit**.

## 2. Objetivo do Sistema

Desenvolver a modelagem de um banco de dados relacional capaz de suportar a gestão administrativa e operacional de uma academia, contemplando:

- Cadastro e controle de alunos, funcionários, planos e matrículas;
- Registro de pagamentos vinculados a matrículas e a vendas de produtos;
- Controle de avaliações físicas e fichas de treino personalizadas;
- Gestão de modalidades, turmas, horários e check-ins;
- Controle de estoque e vendas de produtos (suplementos, acessórios, vestuário).

## 3. Levantamento de Requisitos

Os requisitos abaixo foram levantados a partir da análise do funcionamento típico de uma academia (rotina de matrícula, treino, avaliação física e vendas na recepção) e do modelo de dados de referência (NextFit).

### 3.1 Requisitos Funcionais

- RF01 — O sistema deve permitir o cadastro de alunos, com CPF único como identificador de busca.
- RF02 — O sistema deve permitir o cadastro de funcionários com diferentes funções (Administrador, Recepção, Instrutor, Professor) e níveis de acesso.
- RF03 — O sistema deve permitir a criação de planos e o registro de matrículas de alunos vinculadas a um plano e a um funcionário responsável.
- RF04 — O sistema deve registrar pagamentos vinculados a uma matrícula (mensalidade) ou a uma venda de produto.
- RF05 — O sistema deve permitir o registro de avaliações físicas periódicas por aluno.
- RF06 — O sistema deve permitir a criação de fichas de treino compostas por exercícios do catálogo, associados a máquinas.
- RF07 — O sistema deve permitir o cadastro de modalidades e turmas, com professor responsável e horários semanais.
- RF08 — O sistema deve permitir a inscrição de alunos em turmas e o registro de check-ins (entrada livre ou em turma).
- RF09 — O sistema deve permitir o controle de estoque de produtos e o registro de vendas com múltiplos itens.

### 3.2 Requisitos Não Funcionais

- RNF01 — O sistema deve garantir a integridade referencial entre todas as entidades relacionadas (chaves estrangeiras).
- RNF02 — O CPF do aluno e o código de patrimônio da máquina devem ser únicos no sistema.
- RNF03 — Datas e horários devem seguir formatos padronizados (AAAA-MM-DD e HH:MM:SS).
- RNF04 — Campos de status (aluno, matrícula, pagamento, máquina) devem possuir valores padrão definidos.

## 4. Regras de Negócio

- Um aluno pode possuir várias matrículas ao longo do tempo (histórico), mas apenas uma pode estar ativa por período.
- Um pagamento pode estar vinculado a uma matrícula **ou** a uma venda de produto (vínculo opcional e mutuamente relacionado ao contexto da cobrança).
- Um check-in pode ocorrer de forma livre ou vinculado a uma turma específica.
- Uma ficha de treino é elaborada por um instrutor para um único aluno e pode ser revisada (campo `validade`).
- Cada exercício do catálogo está associado a uma máquina/equipamento.
- O estoque de um produto é reduzido conforme os itens de venda registrados (regra de aplicação, a ser implementada via trigger/aplicação).
- Um aluno com matrícula inativa ou cancelada não deve ser inscrito em novas turmas nem gerar novos check-ins.
- O nível de acesso do funcionário determina quais operações ele pode realizar no sistema (ex.: apenas Administrador e Recepção registram pagamentos e vendas).

## 5. Modelagem de Dados (DER)

O Diagrama Entidade-Relacionamento (DER) representa as 18 entidades do sistema e seus relacionamentos:

`Aluno`, `Funcionario`, `Modalidade`, `Turma`, `Aluno_Turma`, `Avaliacao_Fisica`, `Checkin`, `Maquina`, `Exercicio`, `Ficha_Treino`, `Ficha_Exercicio`, `Horario_Turma`, `Produto`, `Venda_Produto`, `Item_Venda`, `Plano`, `Matricula`, `Pagamento`.

📎 O arquivo do DER está disponível em [`/docs/DER_Academia.png`](./docs/Foto_DER.jpeg) 

### Relacionamentos entre entidades

| Entidade (1) | Entidade (N) | Cardinalidade | Descrição |
|---|---|---|---|
| Modalidade | Turma | 1:N | Uma modalidade pode ter várias turmas. |
| Funcionario | Turma | 1:N | Um funcionário (professor) pode lecionar várias turmas. |
| Aluno | Aluno_Turma | 1:N | Um aluno pode se inscrever em várias turmas. |
| Turma | Aluno_Turma | 1:N | Uma turma pode ter vários alunos inscritos. |
| Turma | Horario_Turma | 1:N | Uma turma pode ter vários horários ao longo da semana. |
| Aluno | Avaliacao_Fisica | 1:N | Um aluno pode ter várias avaliações físicas ao longo do tempo. |
| Funcionario | Avaliacao_Fisica | 1:N | Um funcionário pode realizar várias avaliações. |
| Aluno | Checkin | 1:N | Um aluno pode realizar vários check-ins. |
| Turma | Checkin | 1:N | Uma turma pode estar associada a vários check-ins . |
| Maquina | Exercicio | 1:N | Uma máquina pode ser utilizada em vários exercícios do catálogo. |
| Aluno | Ficha_Treino | 1:N | Um aluno pode ter várias fichas de treino (histórico). |
| Funcionario | Ficha_Treino | 1:N | Um instrutor pode montar várias fichas de treino. |
| Ficha_Treino | Ficha_Exercicio | 1:N | Uma ficha de treino contém vários exercícios (itens). |
| Exercicio | Ficha_Exercicio | 1:N | Um exercício pode aparecer em várias fichas de treino. |
| Aluno | Matricula | 1:N | Um aluno pode ter várias matrículas ao longo do tempo. |
| Plano | Matricula | 1:N | Um plano pode estar associado a várias matrículas. |
| Funcionario | Matricula | 1:N | Um funcionário pode registrar várias matrículas. |
| Aluno | Venda_Produto | 1:N | Um aluno pode realizar várias compras de produtos. |
| Funcionario | Venda_Produto | 1:N | Um funcionário pode registrar várias vendas. |
| Venda_Produto | Item_Venda | 1:N | Uma venda pode conter vários itens . |
| Produto | Item_Venda | 1:N | Um produto pode constar em vários itens de venda. |
| Aluno | Pagamento | 1:N | Um aluno pode ter vários pagamentos registrados. |
| Matricula | Pagamento | 1:N | Uma matrícula pode gerar vários pagamentos . |
| Venda_Produto | Pagamento | 1:N | Uma venda pode gerar vários pagamentos . |

## 6. Dicionário de Dados

O dicionário de dados completo, com todos os atributos de cada entidade (tipo de variável, descrição, valores permitidos, se admite nulo e anotações), foi elaborado no molde de **Costa (2021)** e está disponível em:

📎 [`/docs/Dicionario_de_Dados_Academia.html`](./docs/Dicionario_de_Dados_Academia.pdf)

Entidades documentadas: `Aluno`, `Funcionario`, `Modalidade`, `Turma`, `Aluno_Turma`, `Avaliacao_Fisica`, `Checkin`, `Maquina`, `Exercicio`, `Ficha_Treino`, `Ficha_Exercicio`, `Horario_Turma`, `Produto`, `Venda_Produto`, `Item_Venda`, `Plano`, `Matricula`, `Pagamento`.

## 7. Fluxogramas de Processos

Os fluxogramas dos principais processos do sistema (matrícula de aluno, check-in, venda de produto e elaboração de ficha de treino) estão disponíveis em:

📎 [`/docs/Fluxograma_Matricula.jpeg`](./docs/Foto_2.jpeg)
📎 [`/docs/Fluxograma_Checkin.jpeg`](./docs/Foto_1.jpeg)
📎 [`/docs/Fluxograma_Venda.jpeg`](./docs/Foto_3.jpeg)
📎 [`/docs/Fluxograma_Ficha_Treino.jpeg`](./docs/Foto_4.jpeg)


## 8. Justificativa Técnica das Decisões de Modelagem

- **Chaves primárias substitutas (surrogate keys):** todas as entidades utilizam identificadores numéricos autoincrementados (`id_*`) como chave primária, garantindo estabilidade mesmo diante de mudanças em atributos naturais (ex.: CPF).
- **Relacionamento N:N tratado com tabela associativa:** `Aluno_Turma` resolve a relação muitos-para-muitos entre `Aluno` e `Turma`, usando chave primária composta.
- **Chaves estrangeiras opcionais:** `Pagamento.id_matricula` e `Pagamento.id_venda`, assim como `Checkin.id_turma`, são nuláveis para representar relacionamentos opcionais (um pagamento pode vir de matrícula OU de venda; um check-in pode ou não estar ligado a uma turma).
- **Separação Venda_Produto / Item_Venda:** segue o padrão de modelagem de pedidos (cabeçalho + itens), permitindo múltiplos produtos por venda e preservando o preço praticado no momento da transação (`preco_unitario`).
- **Separação Ficha_Treino / Ficha_Exercicio:** segue o mesmo padrão, permitindo que uma ficha tenha vários exercícios com séries, repetições, carga e ordem próprios.
- **Valores padrão:** campos como `status` (Aluno), `status_matricula`, `status_pagamento` e `tipo` (Checkin) possuem valores padrão para reduzir inconsistência em cadastros incompletos.
- **Normalização:** o modelo segue a Terceira Forma Normal (3FN) — não há atributos derivados armazenados de forma redundante (com exceção de `preco_unitario` em `Item_Venda`, mantido intencionalmente para preservar o histórico de preço no momento da venda) e todos os atributos não-chave dependem exclusivamente da chave primária de sua entidade.

## 9. Uso de Inteligência Artificial

O grupo utilizou uma ferramenta de IA (Claude, da Anthropic) na etapa de **organização e redação deste README**, conforme registrado abaixo.

| Item | Registro |
|------|----------|
| **Ferramenta e etapa** | Claude (Anthropic) — utilizado na etapa de redação e organização do README.md do repositório, a partir do DER e do Dicionário de Dados já elaborados pelo grupo usando tambem para fazer os fluxosgramas. Gemini (Google) — utilizado para estruturar a ideia inicial do DER (entidades, atributos e relacionamentos) a partir de uma entrevista realizada pelo grupo com uma atendente da academia.|
| **Motivação** | O grupo já havia produzido o DER (imagem) e o Dicionário de Dados (PDF) e buscou apoio para estruturar essas informações no formato de README.md exigido para o GitHub, seguindo os critérios de avaliação (conceitual, procedimental e a seção obrigatória de uso de IA). |
| **Prompt(s) utilizados** | "me ajude a fazer o readme com esses dados para o github", acompanhado do envio da imagem do DER e do PDF do Dicionário de Dados, além do trecho do enunciado da atividade contendo os critérios de avaliação e a exigência da seção 9 (Uso de IA). |
| **Resposta recebida** | A IA gerou uma estrutura de README com seções de contexto, objetivo, requisitos, regras de negócio, modelagem/DER, dicionário de dados, fluxogramas, justificativa técnica e uso de IA, extraindo automaticamente a lista de entidades e a tabela de relacionamentos a partir do PDF fornecido, e sugerindo requisitos funcionais/não funcionais e justificativas técnicas com base no padrão de modelagem observado no DER (ex.: chaves substitutas, tabelas associativas, FKs opcionais). |
| **Fontes consultadas e verificadas** | Não houve consulta a fontes externas nesta etapa; o conteúdo foi extraído diretamente dos dois arquivos fornecidos pelo grupo (imagem do DER e PDF do dicionário de dados), que foram conferidos pelo grupo antes da entrega. |
| **Trechos rejeitados ou corrigidos** | Os dados de identificação do grupo (integrantes, matrículas) e os nomes/caminhos reais dos arquivos de fluxograma foram mantidos como "✏️ PREENCHER", pois são informações específicas do grupo que a IA não possui. O restante do conteúdo (contexto, requisitos, regras de negócio e justificativa técnica) foi escrito de forma genérica, coerente com um projeto de academia fictícia/hipotética, e revisado pelo grupo antes da entrega. |
| **Justificativa da escolha final** | Optou-se por manter a estrutura e o texto sugeridos pela IA por estarem alinhados aos critérios de avaliação (pesos conceitual/procedimental/atitudinal) descritos no enunciado e por descreverem corretamente um cenário de academia coerente com o DER e o dicionário de dados elaborados pelo grupo. |
| **Reflexão crítica** | Como o trabalho trata de uma academia genérica/hipotética (não uma organização real visitada), a IA foi usada para redigir um contexto plausível e coerente com o modelo de dados, e não para descrever uma organização específica — reduzindo o risco de "alucinação" sobre fatos reais. Ainda assim, regras de negócio e requisitos generalizados a partir do sistema de referência (NextFit) podem não cobrir particularidades que o grupo queira acrescentar, e devem ser revisados por ele antes da entrega final. |

## 10. Estrutura do Repositório

```
├── README.md
├── docs/
│   ├── DER_Academia.png
│   ├── Dicionario_de_Dados_Academia.html
│   └── Fluxograma_*.png
└── sql/
    └── academia_db.sql        
```
