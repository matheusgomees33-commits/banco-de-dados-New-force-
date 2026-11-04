# Sistema de Cadastro da Academia

# Entrega 2 — Modelo Lógico, Implementação SQL e Apresentação Final
### Sistema de gestão de informações da Academia New Force

| Nome completo do aluno | RGM | Usuário do GitHub |
| :---- | :---- | :---- |
| Cauan Santos Patti  | 44889046 | Caua Santos Patti
| Matheus Gomes Laurentino | 47250895 | matheusgomees33
| Nathan Martins Custodio | 46998519 | martinscustodionathan-ai
| Pedro Macario | 47291931 | pedromacario567-design |
| Vitor Ribeiro de Morais | 46865900 | Vitor-Ribeiro-15 |

**Artefatos:** [levantamento](levantamento/levantamento.md) • [dicionário de dados](dicionario_de_dados.md) • [script SQL completo](docs/script.sql) • [DER conceitual](docs/der_conceitual.png) • [modelo lógico](docs/modelo_logico.png) • [roteiro da apresentação](roteiro_pitch.md) • [evidências](evidencias/)

---

## 1. Relatório Técnico Final

### 1.1 Revisão do Modelo Conceitual

Esta entrega segue o **Caso B**: a organização da Entrega 1 era uma academia genérica, sem existência comprovada. O grupo passou a trabalhar com uma **organização real**, a Academia New Force, e refez para ela toda a base da Entrega 1, inclusive o DER.

#### Caracterização da organização

- **Nome e natureza:** Academia New Force, academia de ginástica e musculação (empresa privada com fins lucrativos), com venda de planos e de suplementos e aulas coletivas de STEP e Pilates.
- **Contexto e porte (números informados pela gerente):**
  - cerca de **850 alunos** e média de **7 novos cadastros por dia**;
  - **19 funcionários** e usuários do sistema: 1 gerente, 4 recepcionistas (um por turno, manhã, tarde e noite, mais um reforço; geralmente duas pela manhã) e 14 professores, 7 deles de musculação, com jornada de 5 horas por dia;
  - cerca de **70 aparelhos** (máquinas, pesos livres, esteiras, escadas e bicicletas);
  - aulas coletivas de STEP e Pilates, com **no máximo 30 pessoas** e agendamento até 20 minutos antes;
  - funcionamento de segunda a sexta, das 5h30 às 22h, e aos sábados, das 8h às 15h; fechada aos domingos;
  - cinco planos (mensal, trimestral, semestral, anual e diária), pagamento em crédito, débito, Pix e dinheiro;
  - revisão dos aparelhos todo mês (musculação) e a cada 15 dias (aeróbicos), com atendimento a defeitos em cerca de 48 horas.
- **Problemas e necessidades identificados** (cada um é resolvido pelo modelo e tem consulta no script):

| Código | Problema | Onde aparece no DER | Consulta |
|---|---|---|---|
| P1 | A catraca libera o acesso mesmo de quem não pagou: o status "bloqueado" não chega a ela de forma confiável | ALUNO, CONTRATO, COBRANCA, REGISTRO_ACESSO | G01, A01 |
| P2 | A cobrança de inadimplentes é manual: a gerente recebe os dados e envia as mensagens | ALUNO, CONTRATO, COBRANCA | G02 |
| P3 | O controle financeiro é feito em planilhas Excel | COBRANCA, PAGAMENTO, NOTA_FISCAL | G03, G11 |
| P4 | A validade dos suplementos é controlada manualmente | PRODUTO, LOTE_PRODUTO, COMPRA | G04, G16 |
| P5 | Não há cadastro individual das máquinas nem histórico de defeitos e reparos, e a gerência não vê quais estão disponíveis, em manutenção ou interditadas | EQUIPAMENTO, CHAMADO_TECNICO, PECA_TROCADA, INSPECAO_EQUIPAMENTO, INCIDENTE_SEGURANCA | G05, G06, G15 |
| P6 | Garantias (1 ano) e revisões não têm controle de vencimento nem alerta | EQUIPAMENTO, ROTINA_MANUTENCAO, EXECUCAO_MANUTENCAO | G07, G08 |
| P7 | Os custos de manutenção ficam espalhados entre o aplicativo da empresa contratada e o financeiro | CHAMADO_TECNICO, PECA_TROCADA | G09 |

- **Escopo do sistema:** o banco reúne **todos os dados da academia**, e não só os dos problemas: alunos, planos, contratos, cobrança, acesso, avaliações, treinos, aulas, funcionários, escalas, vendas e compras de suplementos, equipamentos e manutenção.
- **Justificativa da escolha:** o grupo teve acesso direto à gerente, que concedeu três entrevistas no local. A academia reúne processos variados em porte adequado para esta etapa. **Vínculo:** [PREENCHER: "nenhum integrante trabalha na academia nem tem vínculo familiar com os responsáveis" ou o vínculo existente].
- **Evidências** (arquivos JPG ou PNG na pasta [`evidencias/`](evidencias/)): perfil da academia no Google (Google Maps), fachada, interior da academia e foto com a gerente, além do site (https://www.academianewforce.com.br/) e do Instagram (@academianewforce). Endereço: R. João de Siqueira Afonso, 608 - Lajeado, São Paulo - SP, 08452-105.
- **Responsável pela organização:** Renata Alves Pinheiro, gerente. Telefone: [PREENCHER: telefone da Renata].

#### Relato da entrevista

| Entrevista | Data | Local | Entrevistada | Integrantes presentes | Assunto |
|---|---|---|---|---|---|
| 1 | 25/09/2026 | Academia New Force | Renata Alves Pinheiro, gerente | [PREENCHER] | Operação geral, planos, acesso, cobrança, treinos e aulas |
| 2 | 16/10/2026 | Academia New Force | Renata Alves Pinheiro, gerente | [PREENCHER] | Complemento e validação dos requisitos |
| 3 | 27/10/2026 | Academia New Force | Renata Alves Pinheiro, gerente | [PREENCHER] | Equipamentos: cadastro, manutenção, garantia, segurança e relatórios |

- **Comprovação:** fotos das entrevistas em `evidencias/` (JPG ou PNG): [PREENCHER nomes dos arquivos].
- **Observação presencial** (achados do grupo, separados das respostas da gerente): [PREENCHER: o que o grupo viu, por exemplo a catraca e o sistema da recepção, planilhas e papéis em uso, a placa de equipamento quebrado, o local dos aparelhos].
- **O que foi levantado:** o levantamento completo, em texto organizado, está em [`levantamento/levantamento.md`](levantamento/levantamento.md). Em resumo: uma primeira versão da Entrega 1 usou uma academia genérica e foi descartada; as entrevistas no local trouxeram os números de porte, os processos de acesso, cobrança, treino, aulas, escalas e vendas, e a entrevista de equipamentos acrescentou cadastro de máquinas, manutenção preventiva e corretiva, garantia, inspeção diária, incidentes e relatórios de custo. A primeira entrevista da Entrega 1 teve problemas de informação e foi substituída.

#### Processos de negócio (com os dados de cada um)

| Processo | O que acontece | Dados envolvidos | Entidades |
|---|---|---|---|
| Cadastro e contrato | O atendimento cadastra o aluno, registra a biometria, escolhe o plano e gera o contrato | Nome completo, CPF, RG, nascimento, celular, CEP, e-mail, restrição pessoal, telefone do responsável (menor), senha da catraca; plano, início, fim, promoção, taxa de matrícula | ALUNO, PLANO, PROMOCAO, CONTRATO |
| Controle de acesso | Entrada por reconhecimento facial ou senha; bloqueio quando o plano termina ou o aluno está bloqueado; contingência pela gerente | Data e hora, método, resultado, motivo, funcionário que liberou | REGISTRO_ACESSO, NOTIFICACAO |
| Trancamento e cancelamento | Suspensão por até 3 meses ou cancelamento com multa e meses utilizados, feitos apenas dentro do sistema | Datas de início e fim, data do cancelamento, multa, meses utilizados | TRANCAMENTO, CANCELAMENTO |
| Cobrança e pagamento | Cobrança recorrente, por contrato ou avulsa; pagamento; nota fiscal enviada por WhatsApp | Valor, vencimento, tipo, situação; data e hora, valor pago, forma; número e data da nota | COBRANCA, PAGAMENTO, NOTA_FISCAL |
| Avaliação física | Agendada com o professor; bioimpedância ou adipômetro | Data e hora, método, gordura, massa magra, água, dobras cutâneas, situação | AVALIACAO_FISICA |
| Treinos | O professor conversa com o aluno, monta treinos A, B e C e os mostra pelo aplicativo; troca a cada 3 meses; cargas registradas | Dificuldades, data de troca, letra, exercício, ordem, séries, repetições, carga | FICHA_TREINO, TREINO, EXERCICIO, TREINO_EXERCICIO, REGISTRO_CARGA |
| Aulas coletivas | Consulta por turno e agendamento de STEP e Pilates | Modalidade, dia, hora, professor, capacidade, data da aula, data do agendamento, presença | MODALIDADE, AULA_COLETIVA, AGENDAMENTO_AULA |
| Funcionários | Perfis, permissões, escala por turno e ponto facial | Matrícula, nome, senha, perfil, turno, reforço, horas por dia, entrada e saída; experiência, entrevista e conhecimento do professor | FUNCIONARIO, PROFESSOR, PERFIL, PERMISSAO, PERFIL_PERMISSAO, ESCALA_TRABALHO, PONTO |
| Suplementos | Reposição duas vezes por semana com fornecedores; venda na recepção; controle de validade por lote | Fornecedor, data, produto, quantidade, custo, validade; venda, itens, preço unitário | FORNECEDOR, COMPRA, LOTE_PRODUTO, PRODUTO, VENDA, ITEM_VENDA |
| Manutenção preventiva | Inspeção diária e revisão técnica pela empresa terceirizada (mensal na musculação, quinzenal nos aeróbicos) | Componente, condição, data, rotina, periodicidade, executor, resultado | EQUIPAMENTO, INSPECAO_EQUIPAMENTO, ROTINA_MANUTENCAO, EXECUCAO_MANUTENCAO, ASSISTENCIA_TECNICA |
| Manutenção corretiva e segurança | Interdição, chamado, visita da assistência (cerca de 48 horas), troca de peças, teste de carga e liberação pelo coordenador; acidentes registrados | Abertura, diagnóstico, visita, custo, peças (nome, quantidade, custo, original), teste, liberação; data, descrição, gravidade | CHAMADO_TECNICO, PECA_TROCADA, INCIDENTE_SEGURANCA |

**Processos prováveis não incluídos:**

| Processo comum em academias | Por que não está no modelo |
|---|---|
| Contas a pagar, caixa geral e folha de pagamento | O setor financeiro registra as despesas diariamente fora do sistema (planilhas); o custo de manutenção e de compras fica nas próprias tabelas |
| Captação de interessados e aula experimental | Não foi levantado nas entrevistas |
| Controle de armários | Não foi levantado nas entrevistas |
| Escala de equipe de limpeza e serviços gerais | Não foi levantado nas entrevistas |

#### Requisitos

**Requisitos funcionais**

- RF01 — Cadastrar aluno com nome completo, CPF, RG, data de nascimento, celular, CEP, e-mail, restrição pessoal, senha de acesso à catraca e situação (ativo, inativo ou bloqueado).
- RF02 — Exigir o telefone do responsável quando o aluno for menor de 18 anos.
- RF03 — Manter a situação do aluno e alterá-la para bloqueado quando houver cobrança atrasada.
- RF04 — Registrar a biometria facial do aluno no momento da matrícula.
- RF05 — Registrar cada passagem na catraca com aluno, data e hora, método (facial, senha ou manual), resultado (permitido ou negado) e motivo da negação.
- RF06 — Negar o acesso quando o contrato estiver sem vigência, trancado ou cancelado, ou quando o aluno estiver bloqueado.
- RF07 — Avisar o aluno 7 dias antes do término do plano, registrando data de envio, tipo e canal.
- RF08 — Em falha da catraca, aceitar a senha; se não funcionar, permitir que a gerente libere o acesso manualmente, registrando quem liberou.
- RF09 — Exibir no aplicativo a frequência do aluno a partir dos acessos permitidos.
- RF10 — Cadastrar os planos mensal, trimestral, semestral (6 meses), anual e diária, com valor e forma de cobrança.
- RF11 — Na modalidade de pagamento na hora, cobrar apenas os dias ou meses efetivamente treinados.
- RF12 — Registrar o contrato com aluno, plano, data de início, data de fim e situação, e a taxa de matrícula conforme RN05.
- RF13 — Registrar trancamento do contrato com data de início e fim, de até 3 meses.
- RF14 — Registrar cancelamento do contrato com data, multa do plano e meses efetivamente utilizados.
- RF15 — Vincular ao contrato, opcionalmente, uma promoção.
- RF16 — Cadastrar promoções mensais com descrição, benefício e mês de vigência, priorizando o plano anual.
- RF17 — Registrar cobrança com valor, vencimento, tipo (recorrente, por contrato ou não recorrente) e situação, ligada a um contrato ou a uma venda.
- RF18 — Registrar pagamento com data e hora, valor e forma (crédito, débito, Pix ou dinheiro).
- RF19 — Registrar a nota fiscal de cada pagamento com número, data de emissão e envio por WhatsApp.
- RF20 — Listar os inadimplentes com nome, celular, valor em atraso e dias de atraso, para a gerente enviar a cobrança.
- RF21 — Totalizar o recebimento por mês e forma de pagamento, substituindo as planilhas.
- RF22 — Agendar a avaliação física com o professor, com data e hora.
- RF23 — Registrar o resultado da avaliação: método, gordura acumulada, massa magra e água (bioimpedância) ou dobras cutâneas (adipômetro).
- RF24 — Registrar a ficha de treino com data de criação, dificuldades relatadas pelo aluno, treinos A, B e C, e para cada exercício a ordem, as séries e as repetições.
- RF25 — Disponibilizar o vídeo de execução de cada exercício no aplicativo.
- RF26 — Registrar as cargas do aluno por exercício a cada 3 meses.
- RF27 — Prever a troca do treino a cada 3 meses e notificar o aluno.
- RF28 — Cadastrar as aulas de STEP e Pilates com modalidade, dia, hora de início, professor e capacidade máxima de 30.
- RF29 — Agendar o aluno em uma aula, registrando a data e hora da aula e do agendamento, até 20 minutos antes do início e respeitando a capacidade.
- RF30 — Registrar a presença na aula, sem penalidade por atraso.
- RF31 — Controlar o acesso ao sistema por senha individual e por perfil (recepcionista, professor, gerente), com permissões por módulo e ação; a recepção tem mais controle que a gerente e os professores.
- RF32 — Cadastrar funcionários com matrícula, nome, senha, perfil e situação, e os professores com tempo de experiência, resultado da entrevista e conhecimento na área.
- RF33 — Registrar entrada e saída dos funcionários por reconhecimento facial.
- RF34 — Registrar vendas de suplementos com data e hora, aluno (opcional), vendedor e itens (produto, quantidade e preço unitário).
- RF35 — Controlar a validade dos suplementos por lote.
- RF36 — Sincronizar o status "bloqueado" com a catraca, para que aluno inadimplente não entre.
- RF37 — Registrar as compras de reposição, com fornecedor, data e lotes (produto, quantidade, custo unitário e validade).
- RF38 — Registrar a escala de funcionários com turno (manhã, tarde ou noite), indicação de reforço e horas por dia.
- RF39 — Cadastrar equipamentos com nome, categoria, localização, data de aquisição, fim da garantia, fornecedor e situação, e as rotinas de manutenção com periodicidade e executor.
- RF40 — Registrar cada execução de manutenção preventiva com equipamento, rotina, data, resultado, empresa e custo.
- RF41 — Interditar o equipamento quebrado e abrir chamado técnico com diagnóstico inicial, assistência acionada, data da visita, custo, teste de carga, liberação e o coordenador que a autorizou.
- RF42 — Registrar a inspeção diária de cabos, polias, estofados e estruturas, com condição, observação e responsável.
- RF43 — Registrar as peças trocadas em cada chamado, com nome, quantidade, custo unitário, origem (original ou não) e fornecedor.
- RF44 — Registrar acidentes e problemas de segurança com data, equipamento, aluno envolvido (opcional), descrição e gravidade.
- RF45 — Consultar a situação dos equipamentos (operacional ou interditado) e as revisões e garantias próximas do vencimento.
- RF46 — Gerar o histórico de defeitos e reparos por equipamento e os gastos de manutenção por período.

**Requisitos não funcionais**

- RNF01 — Capacidade: suportar cerca de 850 alunos, média de 7 cadastros por dia e 19 usuários ativos.
- RNF02 — Segurança: autenticar cada usuário por senha, com acesso restrito por perfil.
- RNF03 — Integração: integrar-se à catraca, ao reconhecimento facial e ao aplicativo (Nexfit), à emissão de notas por WhatsApp e às planilhas Excel.
- RNF04 — Disponibilidade e contingência: manter o acesso à academia mesmo com falha da catraca, por senha ou liberação manual.
- RNF05 — Consistência: a situação financeira do aluno deve refletir de forma confiável no controle de acesso.
- RNF06 — Desempenho: o reconhecimento facial e a liberação da catraca devem ser rápidos o bastante para uso em tempo real.
- RNF07 — Usabilidade: o aplicativo concentra treinos, vídeos, agendamentos e frequência, com uso simples por alunos, professores e recepção.
- RNF08 — Rastreabilidade: manter o histórico de acessos, ponto, cargas, avaliações, manutenções e chamados.
- RNF09 — Pagamentos: aceitar crédito, débito, Pix e dinheiro, com suporte a cobrança recorrente.
- RNF10 — Privacidade (LGPD): proteger biometria facial, restrições pessoais, dados de avaliação e de incidentes, com atenção especial aos dados de menores.

**Requisitos prováveis não incluídos:** emissão de relatórios financeiros completos (contas a pagar, fluxo de caixa) e módulo de captação de interessados, pelos mesmos motivos dos processos não incluídos.

#### Regras de negócio

- RN01 — Aluno menor de 18 anos só é cadastrado com o telefone do responsável.
- RN02 — A situação do aluno é ativo, inativo ou bloqueado; cobrança atrasada há mais de 5 dias bloqueia o aluno.
- RN03 — A catraca bloqueia quando o plano terminou, o contrato está trancado ou cancelado, ou o aluno está bloqueado.
- RN04 — O aluno é avisado 7 dias antes do término do plano.
- RN05 — A taxa de matrícula é cobrada na primeira adesão e em renovações após mais de 1 ano; renovação antes de 1 ano é isenta.
- RN06 — O trancamento dura até 3 meses.
- RN07 — No cancelamento, cobram-se a multa do plano e os meses efetivamente utilizados.
- RN08 — Na modalidade de pagamento na hora, o aluno paga apenas os dias ou meses treinados.
- RN09 — As formas de pagamento aceitas são crédito, débito, Pix e dinheiro.
- RN10 — Cada contrato tem no máximo uma promoção por vez; as promoções são mensais e priorizam o plano anual.
- RN11 — Cada cobrança pertence a um contrato ou a uma venda, nunca aos dois.
- RN12 — A avaliação física é agendada com o professor e feita por bioimpedância ou adipômetro.
- RN13 — O professor monta os treinos A, B e C depois de conversar com o aluno; o treino é trocado e as cargas são registradas a cada 3 meses.
- RN14 — Cada aula coletiva (STEP ou Pilates) comporta no máximo 30 pessoas e só pode ser agendada até 20 minutos antes do início; atraso não gera penalidade.
- RN15 — Os professores são funcionários da própria academia, contratados para essa função.
- RN16 — O acesso ao sistema é por senha individual e perfil; a recepção tem mais controle que a gerente.
- RN17 — O ponto dos funcionários é por reconhecimento facial.
- RN18 — A validade dos suplementos é controlada por lote; a reposição ocorre duas vezes por semana, com fornecedores.
- RN19 — Se a catraca falhar, o aluno entra por senha; se não funcionar, a gerente libera manualmente.
- RN20 — A recepção tem um turno de manhã, um à tarde e um à noite, e uma recepcionista de reforço; geralmente há duas pela manhã.
- RN21 — Os professores de musculação trabalham 5 horas por dia e mostram os treinos pelo aplicativo.
- RN22 — A manutenção preventiva tem limpeza diária e lubrificação semanal pela equipe, e revisão técnica pela empresa terceirizada, mensal na musculação e quinzenal nos aeróbicos.
- RN23 — Equipamento quebrado é interditado de imediato e sinalizado; só volta a operar depois do reparo e de teste de carga com o peso máximo.
- RN24 — O cancelamento de matrícula é feito apenas dentro do sistema.
- RN25 — Os equipamentos têm garantia do fabricante de 1 ano.
- RN26 — A preferência é por peças originais do fabricante; se faltar peça de equipamento antigo, a academia costuma substituir o aparelho.
- RN27 — A substituição de um equipamento considera o tempo de uso e a frequência de consertos.
- RN28 — A liberação do equipamento após o conserto é autorizada pelo coordenador.
- RN29 — Cabos, polias, estofados e estruturas são inspecionados diariamente.
- RN30 — Acidentes e problemas de segurança com equipamentos são registrados.
- RN31 — O valor total da venda é a soma dos itens (quantidade × preço unitário).
- RN32 — O valor total da compra é a soma dos lotes (quantidade × custo unitário).
- RN33 — Uma cobrança paga tem pagamentos que somam o valor devido.

**Restrições legais e organizacionais:** LGPD (biometria, saúde e dados de menores), emissão de nota fiscal, horário de funcionamento (seg-sex 5h30-22h, sáb 8h-15h) e dependência da plataforma Nexfit (catraca, facial e aplicativo).

**Regras prováveis não incluídas:** exigência de atestado médico ou PAR-Q e registro no CREF dos professores não foram levantadas nas entrevistas e por isso não entram no modelo; podem ser acrescentadas se a gerente confirmar.

#### Notação e software do DER

O DER segue a notação do brModelo e está em [`docs/der_conceitual.png`](docs/der_conceitual.png). **Software em que foi criado:** [PREENCHER: informe o software realmente usado, por exemplo "brModelo"; se o desenho foi gerado por outra ferramenta, diga qual].

#### Correções em relação à Entrega 1

| Apontamento da correção da Entrega 1 | O que mudou no trabalho | Por quê |
|---|---|---|
| Organização genérica, sem existência comprovada | Troca pela Academia New Force, com evidências, entrevistas no local e responsável identificado | O esqueleto exige organização real |
| Porte sem números e caracterização genérica | Números de alunos, funcionários, aparelhos, aulas e horários | Os números justificam a massa de dados |
| DER físico, com chaves estrangeiras | DER conceitual sem FKs, na notação do brModelo | FK é decisão do modelo lógico |
| PAGAMENTO ligado a aluno, matrícula e venda ao mesmo tempo | COBRANCA separada de PAGAMENTO, com arco exclusivo contrato ou venda (RN11) | Elimina caminho redundante |
| Processos e regras cobrindo só parte da academia (54% e 58%) | Inclusão de cancelamento, compras e fornecedores, manutenção, escalas, LGPD e regras de segurança | O modelo deve cobrir toda a organização |
| RNF eram restrições de banco | RNF de capacidade, segurança, integração, disponibilidade, consistência, desempenho, usabilidade, rastreabilidade, pagamentos e LGPD | RNF descrevem qualidade |
| Dicionário sem fluxo, convenções, índices, log e LGPD | Dicionário no modelo `02-03g`, com restrições por coluna | Exigência do esqueleto |
| Problemas sem consulta | Cada problema P1 a P7 tem consulta gerencial (seção 1.6) | Rastreabilidade |

### 1.2 Conversão do Modelo Conceitual para o Modelo Lógico

- **Entidades → tabelas:** cada uma das 41 entidades virou uma tabela com o mesmo nome, e cada tabela ganhou chave primária substituta `ID_<TABELA>` (INT AUTO_INCREMENT). A chave natural (CPF, número da nota, nome do exercício etc.) virou coluna `UNIQUE`.
- **Atributos → colunas:** cada atributo virou uma coluna com tipo físico (VARCHAR, DATE, DATETIME, DECIMAL, BOOLEAN, TEXT, BLOB, TIME). Atributo opcional `(0,1)` virou coluna `NULL`; os demais, `NOT NULL`. Valores permitidos viraram `CHECK … IN (…)`.
- **Relacionamentos → chaves:** relacionamento 1:N levou a chave da entidade do lado "1" para a tabela do lado "N" como chave estrangeira. Relacionamento 1:1 (PROFESSOR–FUNCIONARIO, CANCELAMENTO–CONTRATO, NOTA_FISCAL–PAGAMENTO) virou FK com `UNIQUE`, colocada na tabela dependente.
- **Cardinalidades:** cardinalidade mínima 0 do lado "1" virou FK `NULL` (por exemplo, ID_PROMOCAO em CONTRATO); mínima 1 virou `NOT NULL`. A cardinalidade N:N entre PERFIL e PERMISSAO virou a tabela associativa PERFIL_PERMISSAO, com `UNIQUE (ID_PERFIL, ID_PERMISSAO)`. As entidades associativas do DER (CONTRATO, AGENDAMENTO_AULA, ITEM_VENDA, TREINO_EXERCICIO, EXECUCAO_MANUTENCAO) viraram tabelas com duas ou mais FKs e atributos próprios.
- **Generalização/especialização:** FUNCIONARIO é a entidade genérica e PROFESSOR a especializada. Usamos **uma tabela por entidade**, com PROFESSOR ligada a FUNCIONARIO por FK única. Tabela única deixaria colunas vazias para recepcionistas e gerente; só as especializadas perderiam a identidade comum (matrícula, senha, perfil, ponto e escala valem para todos).
- **Arco exclusivo:** COBRANCA tem ID_CONTRATO e ID_VENDA anuláveis, com `CHECK` garantindo que exatamente um deles é preenchido (RN11).

### 1.3 Chaves primárias e estrangeiras

| Tabela | Chave primária | Chaves estrangeiras | Chave natural (UNIQUE) |
|---|---|---|---|
| ALUNO | ID_ALUNO | — | ID_CPF |
| PLANO | ID_PLANO | — | — |
| PROMOCAO | ID_PROMOCAO | — | — |
| CONTRATO | ID_CONTRATO | ID_ALUNO → ALUNO, ID_PLANO → PLANO, ID_PROMOCAO → PROMOCAO | — |
| TRANCAMENTO | ID_TRANCAMENTO | ID_CONTRATO → CONTRATO | — |
| CANCELAMENTO | ID_CANCELAMENTO | ID_CONTRATO → CONTRATO | — |
| NOTIFICACAO | ID_NOTIFICACAO | ID_ALUNO → ALUNO | — |
| PERFIL | ID_PERFIL | — | NM_PERFIL |
| PERMISSAO | ID_PERMISSAO | — | (NM_MODULO, TP_ACAO) |
| PERFIL_PERMISSAO | ID_PERFIL_PERMISSAO | ID_PERFIL → PERFIL, ID_PERMISSAO → PERMISSAO | (ID_PERFIL, ID_PERMISSAO) |
| FUNCIONARIO | ID_FUNCIONARIO | ID_PERFIL → PERFIL | ID_MATRICULA |
| PROFESSOR | ID_PROFESSOR | ID_FUNCIONARIO → FUNCIONARIO | — |
| ESCALA_TRABALHO | ID_ESCALA_TRABALHO | ID_FUNCIONARIO → FUNCIONARIO | — |
| PONTO | ID_PONTO | ID_FUNCIONARIO → FUNCIONARIO | (ID_FUNCIONARIO, DT_ENTRADA) |
| AVALIACAO_FISICA | ID_AVALIACAO_FISICA | ID_ALUNO → ALUNO, ID_PROFESSOR → PROFESSOR | — |
| FICHA_TREINO | ID_FICHA_TREINO | ID_ALUNO → ALUNO, ID_PROFESSOR → PROFESSOR | — |
| TREINO | ID_TREINO | ID_FICHA_TREINO → FICHA_TREINO | (ID_FICHA_TREINO, TP_LETRA) |
| EXERCICIO | ID_EXERCICIO | — | NM_EXERCICIO |
| TREINO_EXERCICIO | ID_TREINO_EXERCICIO | ID_TREINO → TREINO, ID_EXERCICIO → EXERCICIO | (ID_TREINO, QT_ORDEM) |
| REGISTRO_CARGA | ID_REGISTRO_CARGA | ID_TREINO_EXERCICIO → TREINO_EXERCICIO | — |
| MODALIDADE | ID_MODALIDADE | — | NM_MODALIDADE |
| AULA_COLETIVA | ID_AULA_COLETIVA | ID_MODALIDADE → MODALIDADE, ID_PROFESSOR → PROFESSOR | (ID_MODALIDADE, TP_DIA_SEMANA, HR_INICIO) |
| AGENDAMENTO_AULA | ID_AGENDAMENTO_AULA | ID_AULA_COLETIVA → AULA_COLETIVA, ID_ALUNO → ALUNO | (ID_ALUNO, ID_AULA_COLETIVA, DT_AULA) |
| PRODUTO | ID_PRODUTO | — | NM_PRODUTO |
| FORNECEDOR | ID_FORNECEDOR | — | NM_FORNECEDOR |
| VENDA | ID_VENDA | ID_ALUNO → ALUNO, ID_FUNCIONARIO → FUNCIONARIO | — |
| ITEM_VENDA | ID_ITEM_VENDA | ID_VENDA → VENDA, ID_PRODUTO → PRODUTO | (ID_VENDA, ID_PRODUTO) |
| EQUIPAMENTO | ID_EQUIPAMENTO | ID_FORNECEDOR → FORNECEDOR | — |
| ROTINA_MANUTENCAO | ID_ROTINA_MANUTENCAO | — | — |
| INSPECAO_EQUIPAMENTO | ID_INSPECAO_EQUIPAMENTO | ID_EQUIPAMENTO → EQUIPAMENTO, ID_FUNCIONARIO → FUNCIONARIO | — |
| INCIDENTE_SEGURANCA | ID_INCIDENTE_SEGURANCA | ID_EQUIPAMENTO → EQUIPAMENTO, ID_ALUNO → ALUNO | — |
| ASSISTENCIA_TECNICA | ID_ASSISTENCIA_TECNICA | — | NM_EMPRESA |
| COBRANCA | ID_COBRANCA | ID_CONTRATO → CONTRATO, ID_VENDA → VENDA | — |
| PAGAMENTO | ID_PAGAMENTO | ID_COBRANCA → COBRANCA | — |
| NOTA_FISCAL | ID_NOTA_FISCAL | ID_PAGAMENTO → PAGAMENTO | NR_NOTA |
| REGISTRO_ACESSO | ID_REGISTRO_ACESSO | ID_ALUNO → ALUNO, ID_FUNCIONARIO → FUNCIONARIO | (ID_ALUNO, DT_ACESSO) |
| COMPRA | ID_COMPRA | ID_FORNECEDOR → FORNECEDOR | — |
| EXECUCAO_MANUTENCAO | ID_EXECUCAO_MANUTENCAO | ID_EQUIPAMENTO → EQUIPAMENTO, ID_ROTINA_MANUTENCAO → ROTINA_MANUTENCAO, ID_ASSISTENCIA_TECNICA → ASSISTENCIA_TECNICA | — |
| CHAMADO_TECNICO | ID_CHAMADO_TECNICO | ID_EQUIPAMENTO → EQUIPAMENTO, ID_ASSISTENCIA_TECNICA → ASSISTENCIA_TECNICA, ID_FUNCIONARIO → FUNCIONARIO | — |
| PECA_TROCADA | ID_PECA_TROCADA | ID_CHAMADO_TECNICO → CHAMADO_TECNICO, ID_FORNECEDOR → FORNECEDOR | — |
| LOTE_PRODUTO | ID_LOTE_PRODUTO | ID_PRODUTO → PRODUTO, ID_COMPRA → COMPRA | — |

### 1.4 Processo de Normalização (1FN, 2FN e 3FN)

Mostramos o processo em cinco situações reais da academia.

**Caso 1 — Contrato e cobrança (planilha de matrícula).** Registro inicial: `(CPF, nome, celular, plano, valor do plano, duração, início, promoção, datas e valores de cada pagamento…)`.
- **1FN:** os pagamentos repetidos (um por mês) formam um grupo repetitivo; passam a linhas de COBRANCA e PAGAMENTO, uma por ocorrência.
- **2FN:** com chave `(CPF, plano, início)`, nome e celular dependem só do CPF, e valor e duração dependem só do plano (dependências parciais). Viram ALUNO e PLANO; CONTRATO fica com os dados da relação.
- **3FN:** a forma de cobrança do plano e o benefício da promoção dependem do plano e da promoção, não do contrato (dependência transitiva). Ficam em PLANO e PROMOCAO.

**Caso 2 — Ficha de treino.** Registro inicial: `(aluno, professor, treino A: exercício 1, séries, repetições, grupo muscular, vídeo, exercício 2…)`.
- **1FN:** exercícios repetidos viram linhas de TREINO_EXERCICIO; as letras A, B e C viram TREINO.
- **2FN:** grupo muscular e vídeo dependem só do exercício; vão para EXERCICIO.
- **3FN:** nenhuma dependência transitiva restante; o professor depende da ficha e fica em FICHA_TREINO.

**Caso 3 — Perfil e permissões.** Registro inicial: `perfil` com uma lista de permissões em um campo de texto (multivalorado).
- **1FN:** cada permissão passa a uma linha de PERMISSAO.
- **2FN:** a relação N:N é resolvida em PERFIL_PERMISSAO, e módulo e ação ficam só em PERMISSAO.
- **3FN:** sem dependências transitivas.

**Caso 4 — Compra e lote de suplementos.** Registro inicial: `(fornecedor, contato, data, produto 1, qtd, custo, validade, produto 2…)`.
- **1FN:** produtos repetidos viram linhas de LOTE_PRODUTO.
- **2FN:** preço de venda e categoria dependem só do produto (PRODUTO); contato depende só do fornecedor (FORNECEDOR).
- **3FN:** o tipo do fornecedor depende do fornecedor, não da compra.

**Caso 5 — Chamado técnico.** Registro inicial: `(equipamento, localização, categoria, assistência, contato, defeito, peças trocadas e custos…)`.
- **1FN:** as peças trocadas (várias por chamado) viram PECA_TROCADA.
- **2FN:** localização, categoria e garantia dependem só do equipamento; contato, só da assistência.
- **3FN:** o fornecedor da peça depende da peça, não do chamado, e fica em FORNECEDOR.

**Decisões que merecem justificativa:**
- `TP_TURNO` foi removido de AULA_COLETIVA: o turno é derivado de `HR_INICIO` (dependência transitiva); a consulta por turno usa `CASE` sobre a hora.
- A categoria "musculação" ou "aeróbico" não é coluna de EQUIPAMENTO: ela depende de `TP_CATEGORIA`; ROTINA_MANUTENCAO se liga à categoria (`TP_CATEGORIA_ALVO`).
- `VL_TOTAL` de VENDA e COMPRA e `DT_FIM` de CONTRATO são derivados e foram mantidos de propósito: o total congela o valor cobrado no momento e a data de fim congela o prazo contratado mesmo que o plano mude depois. A coerência dos totais é conferida pela auditoria A12 (RN31 e RN32).
- `VL_PRECO_UNITARIO` em ITEM_VENDA repete o preço do produto por escolha: preserva o histórico de preços.

**Justificativa do processo:** a 1FN foi necessária onde havia listas (exercícios, pagamentos, peças, permissões); a 2FN, onde a chave composta misturava dados do aluno, do plano e do exercício; a 3FN, onde atributos dependiam de outro atributo não chave (categoria, tipo de fornecedor, forma de cobrança).

### 1.5 Justificativas Técnicas das Decisões

**Chaves primárias.** Todas as tabelas usam `ID_<TABELA> INT AUTO_INCREMENT`: curta, imutável e sem significado de negócio, o que evita propagar CPF ou número de nota nas FKs e permite corrigir um CPF digitado errado sem alterar as tabelas filhas. As chaves naturais ficam como `UNIQUE`.

**Chaves estrangeiras.** Cada FK implementa um relacionamento do DER (seção 1.3). Todas usam `ON DELETE RESTRICT`: nada é apagado em cascata, porque contratos, pagamentos, acessos e chamados são histórico (RN24 e RNF08). FKs `NULL` representam participação opcional (promoção do contrato, aluno da venda, funcionário que liberou o acesso, assistência de uma execução).

**Restrições:**

| Restrição | Onde | Regra |
|---|---|---|
| `UNIQUE (ID_CPF)` | ALUNO | RN01; um aluno não pode ser cadastrado duas vezes |
| `CHECK (LENGTH(ID_CPF) = 11 AND LENGTH(CD_CEP) = 8)` | ALUNO | RN01; formato mínimo do cadastro |
| `CHECK TP_SITUACAO IN ('ativo','inativo','bloqueado')` | ALUNO | RN02 |
| `CHECK (DT_FIM >= DT_INICIO)` | CONTRATO e TRANCAMENTO | RN06; datas coerentes |
| `CHECK TP_COBRANCA IN (...)`, `TP_FORMA IN ('credito','debito','pix','dinheiro')` | COBRANCA e PAGAMENTO | RN09 |
| `CHECK` do arco exclusivo | COBRANCA | RN11 |
| `UNIQUE (ID_CONTRATO)` | CANCELAMENTO | RN24; um cancelamento por contrato |
| `CHECK TP_METODO IN ('bioimpedancia','adipometro')` | AVALIACAO_FISICA | RN12 |
| `UNIQUE (ID_FICHA_TREINO, TP_LETRA)` e `TP_LETRA IN ('a','b','c')` | TREINO | RN13 |
| `CHECK QT_CAPACIDADE_MAXIMA > 0` e `UNIQUE (ID_ALUNO, ID_AULA_COLETIVA, DT_AULA)` | AULA_COLETIVA e AGENDAMENTO_AULA | RN14; sem agendamento duplicado |
| `UNIQUE (ID_FUNCIONARIO)` | PROFESSOR | RN15; um registro de professor por funcionário |
| `UNIQUE (NM_PERFIL)` e `UNIQUE (ID_PERFIL, ID_PERMISSAO)` | PERFIL e PERFIL_PERMISSAO | RN16 |
| `CHECK (DT_SAIDA IS NULL OR DT_SAIDA >= DT_ENTRADA)` | PONTO | RN17 |
| `CHECK QT_RECEBIDA > 0`, `DT_VALIDADE NOT NULL` | LOTE_PRODUTO | RN18 |
| `CHECK TP_SITUACAO IN ('operacional','interditado')` | EQUIPAMENTO | RN23 |
| `CHECK (DT_FIM_GARANTIA >= DT_AQUISICAO)` | EQUIPAMENTO | RN25 |
| `CHECK (DT_LIBERACAO >= DT_ABERTURA)` | CHAMADO_TECNICO | RN23 |
| `IN_ORIGINAL BOOLEAN DEFAULT 1` | PECA_TROCADA | RN26 |
| `TP_COMPONENTE IN ('cabo','polia','estofado','estrutura')` | INSPECAO_EQUIPAMENTO | RN29 |
| `TP_GRAVIDADE IN ('leve','moderada','grave')` | INCIDENTE_SEGURANCA | RN30 |

Regras que o banco não impõe sozinho (capacidade de 30, 20 minutos, 3 meses de trancamento, bloqueio por atraso, aviso de 7 dias, liberação com teste de carga) são conferidas pelas **consultas de auditoria** A01 a A13.

**Decisões arquiteturais:** (1) modelo conceitual sem FKs e lógico com FKs; (2) COBRANCA separada de PAGAMENTO; (3) REGISTRO_ACESSO único para catraca, bloqueio e frequência; (4) uma tabela por entidade na generalização; (5) manutenção preventiva e corretiva separadas (EXECUCAO_MANUTENCAO e CHAMADO_TECNICO), porque uma é periódica e a outra nasce de uma falha; (6) permissões em tabela, e não em texto, para a recepção ter mais acesso que a gerente sem alterar código; (7) PROFESSOR e ESCALA_TRABALHO para atender escalas sem fixá-las no cadastro.

### 1.6 Proposta de Arquitetura Analítica para BI e IA

**Informações estratégicas:** inadimplência e seu efeito no acesso; receita por plano e forma de pagamento; cancelamentos e retenção; frequência e ocupação das aulas; estado, histórico e custo dos equipamentos; produtos de maior giro e vencimentos.

**Indicadores (KPIs):**

| KPI | Fórmula | Período | Meta sugerida | Consulta |
|---|---|---|---|---|
| Acessos indevidos de alunos bloqueados | Acessos permitidos de alunos bloqueados | Dia | 0 | G01 |
| Valor e dias de inadimplência | Soma de cobranças atrasadas; dias desde o vencimento | Mensal | Reduzir | G02 |
| Receita mensal por forma de pagamento | Soma de VL_PAGO | Mensal | Crescer | G03 |
| Receita e ticket médio por plano | Soma e média de VL_PAGO por plano | Mensal | Crescer | G11 |
| Taxa de cancelamento | Cancelamentos ÷ contratos iniciados | Mensal | Abaixo de 2% | G10 |
| Ocupação das aulas coletivas | Agendados ÷ 30 vagas | Semanal | Acima de 60% | G12 |
| Frequência média semanal | Acessos permitidos ÷ alunos ativos ÷ semanas | Quinzenal | Acompanhar | G13 |
| Lotes vencidos ou a vencer | Contagem de lotes com validade ≤ 30 dias | Semanal | 0 vencidos | G04 |
| Produtos mais vendidos e saldo baixo | Vendido; recebido menos vendido | Mensal | Saldo acima de 10 | G16 |
| Equipamentos interditados | Contagem por situação e categoria | Diário | Mínimo | G05 |
| Defeitos por equipamento | Chamados por equipamento | 12 meses | Apoia substituição | G06 |
| Tempo médio de parada | Horas entre abertura e liberação | Mensal | Até 48 h | G14 |
| Gasto de manutenção | Serviços mais peças | Mensal | Acompanhar | G09 |
| Condição de cabos e estofados | Última inspeção diferente de "boa" | Diário | 0 em "ruim" | G15 |
| Garantias e revisões a vencer | Garantias em 60 dias; revisões atrasadas | Semanal | 0 atrasadas | G07, G08 |
| Treinos a trocar | Fichas com troca prevista vencida | Semanal | 0 | G17 |
| Avaliações por professor | Avaliações realizadas | Mensal | Acompanhar | G18 |

**Potencial para BI:** o modelo suporta um painel com quatro visões: financeira (G03, G10, G11), de acesso e frequência (G01, G13), de equipamentos (G05, G06, G08, G09, G14, G15) e de produtos (G04, G16). As tabelas de fato naturais são PAGAMENTO, REGISTRO_ACESSO, AGENDAMENTO_AULA, ITEM_VENDA e CHAMADO_TECNICO; as dimensões são tempo, ALUNO, PLANO, PROFESSOR, PRODUTO e EQUIPAMENTO.

**Aplicações de análise e IA:** previsão de cancelamento a partir da frequência, da inadimplência e do plano; manutenção preditiva, usando a frequência de defeitos, a condição de cabos e estofados e o tempo de uso para prever rompimentos de cabo; previsão de demanda de suplementos para a reposição de duas vezes por semana; recomendação de horários de aula pela ocupação; e detecção de anomalias no acesso, como os acessos indevidos de bloqueados.

**Rastreabilidade:**

| Problema ou KPI | Tabelas envolvidas | Consulta |
|---|---|---|
| P1 — catraca libera quem não pagou | ALUNO, CONTRATO, COBRANCA, REGISTRO_ACESSO | G01, A01 |
| P2 — cobrança manual de inadimplentes | ALUNO, CONTRATO, COBRANCA | G02 |
| P3 — controle financeiro em planilhas | PAGAMENTO, COBRANCA, CONTRATO, PLANO | G03, G11 |
| P4 — validade dos suplementos | PRODUTO, LOTE_PRODUTO, ITEM_VENDA | G04, G16 |
| P5 — sem cadastro e histórico das máquinas | EQUIPAMENTO, CHAMADO_TECNICO, INSPECAO_EQUIPAMENTO | G05, G06, G15 |
| P6 — garantias e revisões sem alerta | EQUIPAMENTO, EXECUCAO_MANUTENCAO, ROTINA_MANUTENCAO | G07, G08 |
| P7 — custos de manutenção dispersos | CHAMADO_TECNICO, PECA_TROCADA | G09 |
| KPI taxa de cancelamento | CANCELAMENTO, CONTRATO | G10 |
| KPI ocupação das aulas | AGENDAMENTO_AULA, AULA_COLETIVA, MODALIDADE | G12 |
| KPI frequência média | REGISTRO_ACESSO, ALUNO | G13 |
| KPI tempo médio de parada | CHAMADO_TECNICO | G14 |
| KPI treinos a trocar | FICHA_TREINO, ALUNO | G17 |
| KPI avaliações por professor | AVALIACAO_FISICA, PROFESSOR, FUNCIONARIO | G18 |

---

## 2. Modelo Lógico Relacional

Diagrama: [`docs/modelo_logico.png`](docs/modelo_logico.png). **Software em que foi criado:** [PREENCHER: informe o software realmente usado, por exemplo "brModelo"]. O diagrama mostra as 41 tabelas, as colunas com tipo, `PK`, `FK` e as cardinalidades `(mín, máx)` já convertidas. As restrições `NOT NULL`, `UNIQUE`, `CHECK` e `DEFAULT` estão no [dicionário de dados](dicionario_de_dados.md) e no script SQL.

## 3. Script SQL Completo

- **Arquivo:** [`docs/script.sql`](docs/script.sql) — executa em MySQL 8, do zero, na ordem: `CREATE DATABASE`, `CREATE TABLE`, constraints, `INSERT INTO`, `SELECT` com `JOIN` (J01 a J04), `UPDATE` (U01 a U03), `DELETE` (D01), consultas gerenciais (G01 a G18) e consultas de auditoria (A01 a A13).
- **Teste:** [PREENCHER: "o grupo executou o script do zero, em banco vazio, no MySQL do laboratório / MySQL Workbench / myCompiler, sem erros" e a data].
- **Versão reduzida para execução online:** [`docs/script_mycompiler.sql`](docs/script_mycompiler.sql), com a mesma estrutura, as mesmas consultas e um décimo dos alunos, para ambientes que não aceitam arquivos grandes.

## 4. Massa de Dados

A massa é **fictícia** (nomes, CPFs, contatos e senhas inventados, gerados com semente fixa) e está no mesmo `script.sql`, na seção de `INSERT`. O volume vem dos números da academia:

| Tabela(s) | Registros | Como o número se justifica | Origem do número |
|---|---|---|---|
| ALUNO | 850 (790 ativos, 25 bloqueados, 35 inativos) | Cerca de 850 alunos cadastrados (RNF01); cerca de 5% menores de idade | Entrevista |
| CONTRATO | 2088 | Um contrato atual por aluno mais o histórico de renovações (planos mensais encadeados e planos de 3 a 12 meses): 780 vigentes, 15 trancados, 29 cancelados, 1264 encerrados | Estimativa do grupo |
| COBRANCA, PAGAMENTO, NOTA_FISCAL | 2376, 2347, 2314 | Uma cobrança por contrato e uma por venda (288); nota fiscal em 98% dos pagamentos, para a auditoria ter o que mostrar | Derivado das demais tabelas |
| TRANCAMENTO, CANCELAMENTO | 17, 29 | Cerca de 5 cancelamentos por mês ao longo de 6 meses; trancamentos de até 3 meses | Entrevista (período a confirmar) |
| REGISTRO_ACESSO | 2816 | 14 dias de catraca, com cerca de 280 a 320 passagens por dia útil e 120 aos sábados, mais negados e liberações manuais | Hipótese do grupo (confirmar acessos por dia) |
| NOTIFICACAO | 109 | Aviso de término 7 dias antes (RN04) e aviso de novo treino | Derivado dos contratos e fichas |
| AVALIACAO_FISICA | 170 | Cerca de 50 avaliações por mês, mais as agendadas para os próximos 14 dias | Hipótese do grupo |
| FICHA_TREINO, TREINO, TREINO_EXERCICIO, REGISTRO_CARGA, EXERCICIO | 751, 2253, 9012, 2243, 40 | Uma ficha por aluno com acesso vigente, 3 treinos (A, B e C) com 4 exercícios cada, cargas registradas em parte (a cada 3 meses); catálogo de 40 exercícios | Entrevista e hipótese |
| MODALIDADE, AULA_COLETIVA, AGENDAMENTO_AULA | 2, 12, 510 | STEP e Pilates, 12 aulas por semana (6 de cada, com professores e horários diferentes), 2 semanas de agendamentos, capacidade de 30 | Entrevista |
| FUNCIONARIO, PROFESSOR, PERFIL, PERMISSAO, PERFIL_PERMISSAO, ESCALA_TRABALHO, PONTO | 19, 14, 3, 18, 32, 19, 228 | 19 funcionários (1 gerente, 4 recepcionistas, 14 professores); ponto de 14 dias; 3 perfis | Entrevista |
| PRODUTO, FORNECEDOR, COMPRA, LOTE_PRODUTO | 24, 9, 18, 88 | Reposição duas vezes por semana durante 8 semanas, com 4 lotes por compra; fornecedores de suplementos, equipamentos e peças | Entrevista |
| VENDA, ITEM_VENDA | 288, 542 | Cerca de 6 vendas por dia útil durante 8 semanas, com 1 a 3 itens | Hipótese do grupo (confirmar vendas por semana) |
| EQUIPAMENTO | 70 | Cerca de 70 aparelhos: 38 máquinas, 12 pesos livres, 8 esteiras, 4 escadas e 8 bicicletas (distribuição estimada) | Entrevista (total); distribuição estimada |
| ROTINA_MANUTENCAO, EXECUCAO_MANUTENCAO, ASSISTENCIA_TECNICA | 15, 260, 1 | 3 rotinas por categoria; revisão mensal na musculação e a cada 15 dias nos aeróbicos, por 1 empresa terceirizada | Entrevista |
| CHAMADO_TECNICO, PECA_TROCADA | 31, 25 | Cerca de 5 chamados por mês em 6 meses (cabos e bicicletas são os defeitos mais comuns), 55% com troca de peça, 3 abertos hoje | Entrevista (tipos de defeito); quantidade é hipótese |
| INSPECAO_EQUIPAMENTO, INCIDENTE_SEGURANCA | 696, 6 | Inspeção diária dos últimos 7 dias; poucos incidentes em 6 meses | Entrevista e hipótese |
| Total | 30356 |  |  |

**Atenção:** as linhas marcadas "Hipótese" são estimativas e devem ser confirmadas com a gerente (acessos por dia, vendas por semana, avaliações por mês e chamados por mês). Se os números reais forem diferentes, o grupo ajusta a escala e regenera a massa.

**Variedade:** a massa cobre alunos ativos, inativos e bloqueados; contratos vigentes, trancados, cancelados e encerrados; cobranças pagas e atrasadas; pagamentos nas quatro formas; acessos permitidos, negados e manuais, inclusive **12 acessos indevidos de alunos bloqueados e 5 de contratos vencidos** (a falha real da catraca, P1); lotes vencidos e a vencer; produtos com saldo baixo; equipamentos interditados, com garantia a vencer e com revisão atrasada; cabos e estofados em condição de atenção e ruim; e incidentes. Por isso as consultas G e A retornam resultados

## 6. Uso de Inteligência Artificial

Ferramenta: **Claude (Anthropic)**, em conversa única com o grupo, para as etapas abaixo. As respostas foram revisadas pelo grupo.

| Item | Uso 1 — Correção da Entrega 1 e requisitos | Uso 2 — Modelo conceitual e lógico, conversão e normalização | Uso 3 — Script SQL, consultas e massa de dados | Uso 4 — Dicionário, levantamento, KPIs, README e roteiro do pitch |
|---|---|---|---|---|
| **Etapa** | Análise da avaliação da Entrega 1 e atualização de requisitos e regras | DER conceitual, lista de tabelas, PKs e FKs, normalização 1FN a 3FN | `CREATE TABLE`, constraints, consultas G e A, `UPDATE` e `DELETE`; geração da massa por um script em Python com semente fixa | Dicionário no modelo 02-03g, texto do levantamento, KPIs, rastreabilidade, README e roteiro |
| **Motivação** | Entender os descontos e corrigi-los com os dados reais da academia | Obter uma proposta de modelo completa e consistente com os requisitos | Ganhar tempo e garantir variedade de dados | Padronizar a documentação |
| **Prompts** | "Eu preciso arrumar todos esse criterios pois tive um desempenho ruim. Eu precisava fazer usando o esqueleto de entrega 1." e envio do documento de requisitos e das respostas da gerente | "Consegue me ajudar na questão do DER para eu colocar [...] usando os dados da entrevista"; envio do DER do grupo para revisão | "me ajude a fazer o script para eu colocar no myCompiler (MySQL)" | "Agora quero que você faça o que tinha dito que faria [...] Faça tudo que puder" |
| **Resposta recebida** | Diagnóstico por critério e lista de conflitos entre as entrevistas | Modelo com 41 tabelas e guia de desenho no brModelo | Script SQL, versão reduzida e consultas de auditoria | Arquivos em Markdown |
| **Fontes verificadas** | A IA não citou fontes externas; o grupo conferiu cada dado com as entrevistas e com o Google e o site da academia | Conferência do modelo com os requisitos RF01 a RF46 | O grupo executou o script do zero e conferiu os resultados das consultas [PREENCHER] | O grupo revisou o texto contra o levantamento |
| **Rejeitado ou corrigido** | RF28 (dança e luta viraram STEP e Pilates); "instrutor" virou "professor"; números de porte seguem o documento de requisitos; cancelamento só no sistema; falha da catraca descrita corretamente | Entidade RELATORIO do DER do grupo não entrou (relatório é consulta, não dado); PERMISSAO foi adotada do DER do grupo; TP_TURNO removido por ser derivado | Ajustes de sintaxe e dialeto MySQL [PREENCHER o que o grupo corrigir ao testar] | [PREENCHER] |
| **Justificativa da escolha final** | Manteve-se o que correspondia às entrevistas | Manteve-se o modelo com 41 tabelas por cobrir todos os dados levantados | Manteve-se o script por executar do zero e cobrir todos os problemas | Manteve-se por seguir os modelos da disciplina |
| **Reflexão crítica** | A IA conhece a academia apenas pelos dados repassados pelo grupo, sem observar o local nem validar as informações; lacunas foram preenchidas com suposições (tipos, perfis, índices), revisadas pelo grupo | O modelo proposto é mais amplo do que o desenho do grupo; exige redesenhar o DER; a IA não desenha no brModelo | A massa é fictícia e os volumes de acesso e de vendas são estimativas do grupo; um script gerado por IA pode ter erro de dialeto e deve ser testado | Texto gerado por IA pode soar genérico; o grupo deve conferir cada afirmação |

**Etapas em que a IA não foi usada:** [PREENCHER: declare o que o grupo fez sem IA, por exemplo as entrevistas, as visitas, as fotos e o desenho dos diagramas]. O uso de IA para "levantar processos, requisitos e regras típicos" de academias não foi realizado; o levantamento partiu das entrevistas. O roteiro de entrevista de equipamentos foi "organizado e revisado" para leitura: [PREENCHER: informe se isso foi feito com IA e qual]
