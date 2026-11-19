# Inventário da base de conhecimento — Olho na Lente
Esta é a lista do que a base vai ter. Na Sprint 01 entregamos o inventário completo e **dois documentos já prontos** (catálogo de ativos e histórico de falhas). O resto será escrito nas próximas sprints.

> **Tudo marcado como `simulado` é fictício**, criado pelo grupo com dados plausíveis. Nenhuma TAG, modelo, fabricante, OS ou número abaixo é da Motiva.


## Quantidade mínima e critérios de conclusão
| Documento | Quantidade mínima | Critério de conclusão |
|---|---|---|
| Catálogo de ativos | 30 linhas | TAGs únicas e campos obrigatórios preenchidos |
| Histórico de falhas | 50 registros | Registros associados às TAGs do catálogo |
| Plano de manutenção | 1 documento | Tarefas e periodicidades definidas |
| POPs | 4 procedimentos	| Etapas e pré-requisitos de segurança |
| Fichas técnicas	| 3 fichas (uma por categoria de ativo)	| Especificações e origem identificadas |
| Matriz de criticidade	| 1 documento	| Critérios e classificação justificados |
| Glossário	| 1 documento	| Siglas e conceitos do domínio |
| Normas e segurança	| 1 documento	| Resumos e referências identificados |
| Briefing Motiva	| 1 documento	| Temas e contexto do desafio |
| Boletins de ocorrência	| 10 registros	| Relatos sintéticos com condições ambientais |


## Documentos
| ID | Documento | Conteúdo e campos | Origem | Tamanho | Status |
|---|---|---|---|---|---|
| KB-01 | Catálogo de ativos | tag, tipo_ativo, fabricante, modelo, km, sentido, praca_portico, direcao_lente, data_instalacao, vida_util_anos, criticidade, vizinhas, sca_proxima. Serve para o assistente saber que equipamento é, onde fica e quais são as vizinhas (tool `consultar_ativo`). Dado fictício criado pelo grupo; numa operação real viria do cadastro de ativos da concessionária. Atualizar a cada instalação, troca ou mudança de posição de um equipamento| simulado | 46 linhas (40 câmeras + 6 SCA) | **pronto (Sprint 1)**: [`catalogo_ativos_simulado.csv`](catalogo_ativos_simulado.csv) |
| KB-02 | Histórico de falhas do FSM | id, data, tag, sintoma, causa_identificada, classificacao (REAL/AMBIENTAL), acao, tempo_reparo_h, peca_trocada, reincidencia, os. Serve para o assistente ver o que já aconteceu com cada ativo (causa, se foi REAL ou AMBIENTAL, reincidência) e calcular métricas de confiabilidade (tools `consultar_historico_falhas` e `calcular_metricas_confiabilidade`). Dado fictício criado pelo grupo; numa operação real viria do FSM, a partir das OS encerradas. Atualizar a cada OS encerrada | simulado | 77 registros | **pronto (Sprint 1)**: [`historico_falhas_simulado.csv`](historico_falhas_simulado.csv) |
| KB-03 | POPs (procedimentos) | POP-CFTV-001 Inspeção visual remota; POP-CFTV-002 Limpeza de lente; POP-CFTV-003 Ajuste de foco; POP-CFTV-004 Teste de comunicação. Cada um: código, versão, pré-requisitos de segurança, passos | simulado | 4 POPs | Planejado para preparação da base |
| KB-04 | Normas e segurança | Resumo dos trechos aplicáveis da NR-10 (eletricidade) e NR-35 (altura) + regras de trabalho em via (sinalização, cone, viatura de apoio) | referência pública (resumo) | 1 doc | Planejado para preparação da base |
| KB-05 | Plano de manutenção preventiva | ativo, tarefa, periodicidade, responsável, tempo estimado, recursos, condição de execução; inclui a fórmula R(t)=e^(−λt) e como achar o intervalo para 90% | simulado | 1 doc | Planejado para preparação da base |
| KB-06 | Matriz de criticidade | criticidade A/B/C × impacto (segurança viária, disponibilidade do monitoramento) → prioridade P1 (até 4 h), P2 (até 24 h), P3 (até 72 h) | simulado | 1 doc | Planejado para preparação da base |
| KB-07 | Fichas técnicas | CFTV PTZ, CFTV fixa e SCA: faixa de operação, IR, IP, alimentação, sensores da SCA (chuva, visibilidade) | real sobre o contexto do desafio (CFTV, PTZ, SAT) + simulado (SCA, TAGs, PTZ) | 3 fichas | Planejado para preparação da base |
| KB-08 | Glossário de siglas | CFTV, PTZ, PMV, SAT, SCA, CCO, CBM, RCM, WCM, FSM, RGA, LDA, λ, MTBF, MTTR, WIM, OCR, IR, POP, OS | real | 1 doc | Planejado para preparação da base |
| KB-09 | Documentos do briefing Motiva | contexto da Motiva, parque de equipamentos, Temas 1, 2 e 2.1 | material fornecido para o desafio | 1 doc | A extrair do briefing |
| KB-10 | Boletins / laudos de campo | texto livre do técnico ao atender: o que viu, condição do tempo, foto descrita (sem pessoas nem placas) | simulado | 12 boletins | Planejado para preparação da base |

Quatro câmeras têm histórico pensado para virar casos de teste:
| TAG | Perfil | Uso no eval set |
|---|---|---|
| CAM-SP330-KM418-S | Suja muito: limpezas e teia repetidas → λ alto | C02, C04, C12, C13 |
| CAM-SP330-KM412-N | Voltada para leste: alarmes de sol de frente pela manhã (AMBIENTAL) + 1 desfoque real | C01, C10 e exemplo few-shot |
| CAM-SP330-KM421-N | Região sujeita a neblina: possui alarmes AMBIENTAIS recorrentes e também registro de falha REAL por obstrução da lente | C11 |
| CAM-SP330-KM424-N | PTZ com falhas mecânicas e de sinal (peça trocada) | C11 (vizinha) |

As fichas utilizar datasheets públicos de fabricantes como referência técnica. Modelos, fabricantes e equipamentos não confirmados no briefing serão identificados como referências ou dados simulados, sem associação à operação real da Motiva. Cada especificação deverá apresentar sua fonte.


## Padrão de identificação e rastreabilidade dos documentos
| Campo | Descrição |
|---|---|
| `doc_id` | Identificador único do documento (ex.: KB-01). |
| `tipo_documento` | Categoria do documento (pop, ficha, norma, plano, matriz, glossario, briefing, boletim). |
| `tipo_ativo` | Categoria do equipamento relacionado, quando aplicável (cftv_ptz, cftv_fixa, sca). |
| `tag` | Identificador do ativo relacionado, quando aplicável. |
| `tipo_intervencao` | Tipo de intervenção relacionada ao documento, quando aplicável (preventiva, corretiva, inspeção). |
| `data` | Data de criação, emissão ou referência do conteúdo. |
| `versao` | Versão do documento, utilizada para controle de atualizações. |
| `origem` | Procedência da informação (real, simulado ou referência pública). |
| `responsavel` | Responsável pela elaboração ou revisão do documento. |
| `status` | Situação do documento (planejado, em elaboração, validado ou desativado). |

Essa padronização permitirá filtrar os resultados por tipo de documento, equipamento, TAG, intervenção, origem e versão, além de facilitar a identificação de informações desatualizadas ou conflitantes.
Os metadados permitirão identificar a procedência das informações, controlar versões e evitar a recuperação de documentos desatualizados.


## Tratamento de informações conflitantes
Quando dois ou mais documentos apresentarem informações divergentes, o assistente deverá:

1. Verificar a origem, a versão e a data de atualização dos documentos.
2. Priorizar informações validadas e com procedência identificada.
3. Não combinar dados incompatíveis para produzir uma conclusão.
4. Informar a existência da divergência quando não houver evidência
   suficiente para determinar qual informação está correta.
5. Encaminhar a situação para validação do responsável técnico.

Nenhuma informação conflitante deverá ser resolvida por suposição ou geração de dados pelo modelo.



## Integração com as ferramentas do assistente
- A base de conhecimento deverá atuar de forma complementar às ferramentas especificadas em `/tools/tools_spec.json`.
- Informações estruturadas e consultáveis por TAG, como dados cadastrais dos ativos e histórico de falhas, terão como fonte principal as respectivas ferramentas.
- Documentos técnicos, procedimentos operacionais, normas e referências serão utilizados como fontes de recuperação contextual pelo RAG.
- Quando uma informação estruturada também aparecer em um documento textual, a ferramenta correspondente deverá ser priorizada para consultas de dados atualizados.
- Essa separação busca evitar duplicidade, divergências e múltiplas fontes de verdade para uma mesma informação.


## Como a base é usada no RAG
- **Dados estruturados (KB-01, KB-02)** não vão para o vector store. Eles são lidos pelas tools (`consultar_ativo`, `consultar_historico_falhas`, `calcular_metricas_confiabilidade`). Assim o número vem da tabela, e não de um trecho de texto parecido.
- **Textos (KB-03 a KB-10)** são quebrados em trechos (chunks) e vão para o vector store.


### Tamanho do trecho (chunk)
- **400 a 600 tokens, com 80 de sobreposição.** POP e ficha técnica são documentos de passo a passo: um trecho desse tamanho guarda um procedimento inteiro ou uma seção inteira, sem cortar a lista de segurança no meio.
- Trecho pequeno demais (≈100 tokens) separa o passo do seu pré-requisito de segurança. Trecho grande demais (>1.000) ocupa a janela de contexto com texto que não ajuda e piora a busca.
- Na consulta entram os **4 melhores trechos** (≈2.000 tokens). Ver a política de contexto no README.


### Metadados guardados em cada trecho
`doc_id`, `tipo_documento` (pop, ficha, norma, plano, matriz, glossario, briefing, boletim), `tipo_ativo`, `tag` (quando o trecho é de um ativo), `tipo_intervencao`, `data`, `versao`, `origem` (real/simulado). Com isso dá para filtrar, por exemplo, "só POP de cftv_ptz".


## Controle de qualidade e atualização da base
Para manter a confiabilidade das informações utilizadas pelo EngMan Copilot, os documentos da base de conhecimento devem seguir critérios de validação antes de serem disponibilizados para consulta.


### Critérios de validação
- **Identificação da origem:** todo documento deve indicar se seus dados são reais, simulados ou provenientes de documentação técnica.
- **Consistência:** verificar se TAGs, identificadores e referências cruzadas correspondem aos registros do catálogo de ativos.
- **Rastreabilidade:** manter código ou identificação do documento, versão, data de atualização e responsável pela revisão.
- **Segurança:** verificar a ausência de dados pessoais, imagens identificáveis e informações operacionais confidenciais.
- **Atualização:** documentos revisados devem ter sua versão atualizada, evitando que informações antigas sejam recuperadas pelo RAG.


### Cuidados na recuperação de informações
O sistema deve priorizar informações compatíveis com o contexto da consulta, considerando o tipo de equipamento, a origem dos dados e a versão do documento.

Quando não houver informação suficiente na base, o assistente deve informar a limitação, sem inventar procedimentos, diagnósticos ou dados técnicos.


## O que NÃO entra na base (LGPD)
- Imagens e quadros das câmeras (podem ter rostos e placas). A base guarda só o **texto** do alarme e do laudo.
- Placas, nomes de motoristas, dados de ocupantes de veículos.
- Dados pessoais de empregados: nome do técnico, matrícula, telefone, escala. O boletim guarda só "equipe" (ex.: "Equipe CFTV-2").
- Dados contratuais e de arrecadação da concessionária.
- Qualquer dado real de operação da Motiva enquanto o projeto for acadêmico.
