# Inventário da base de conhecimento — Olho na Lente

Esta é a lista do que a base vai ter. Na Sprint 01 entregamos o inventário completo e **dois documentos já prontos** (catálogo de ativos e histórico de falhas). O resto será escrito nas próximas sprints.

> **Tudo marcado como `simulado` é fictício**, criado pelo grupo com dados plausíveis. Nenhuma TAG, modelo, fabricante, OS ou número abaixo é da Motiva.

## Documentos

| ID | Documento | Conteúdo e campos | Origem | Tamanho | Status |
|---|---|---|---|---|---|
| KB-01 | Catálogo de ativos | tag, tipo_ativo, fabricante, modelo, km, sentido, praca_portico, direcao_lente, data_instalacao, vida_util_anos, criticidade, vizinhas, sca_proxima | simulado | 46 linhas (40 câmeras + 6 SCA) | **pronto**: [`catalogo_ativos_simulado.csv`](catalogo_ativos_simulado.csv) |
| KB-02 | Histórico de falhas do FSM | id, data, tag, sintoma, causa_identificada, classificacao (REAL/AMBIENTAL), acao, tempo_reparo_h, peca_trocada, reincidencia, os | simulado | 77 registros | **pronto**: [`historico_falhas_simulado.csv`](historico_falhas_simulado.csv) |
| KB-03 | POPs (procedimentos) | POP-CFTV-001 Inspeção visual remota; POP-CFTV-002 Limpeza de lente; POP-CFTV-003 Ajuste de foco; POP-CFTV-004 Teste de comunicação. Cada um: código, versão, pré-requisitos de segurança, passos | simulado | 4 POPs | a escrever (Sprint 03) |
| KB-04 | Normas e segurança | Resumo dos trechos aplicáveis da NR-10 (eletricidade) e NR-35 (altura) + regras de trabalho em via (sinalização, cone, viatura de apoio) | real (resumo) | 1 doc | a escrever |
| KB-05 | Plano de manutenção preventiva | ativo, tarefa, periodicidade, responsável, tempo estimado, recursos, condição de execução; inclui a fórmula R(t)=e^(−λt) e como achar o intervalo para 90% | simulado | 1 doc | a escrever |
| KB-06 | Matriz de criticidade | criticidade A/B/C × impacto (segurança viária, disponibilidade do monitoramento) → prioridade P1 (até 4 h), P2 (até 24 h), P3 (até 72 h) | simulado | 1 doc | a escrever |
| KB-07 | Fichas técnicas | CFTV PTZ, CFTV fixa e SCA: faixa de operação, IR, IP, alimentação, sensores da SCA (chuva, visibilidade) | real + simulado | 3 fichas | a escrever |
| KB-08 | Glossário de siglas | CFTV, PTZ, PMV, SAT, SCA, CCO, CBM, RCM, WCM, FSM, RGA, LDA, λ, MTBF, MTTR, WIM, OCR, IR, POP, OS | real | 1 doc | a escrever |
| KB-09 | Documentos do briefing Motiva | contexto da Motiva, parque de equipamentos, Temas 1, 2 e 2.1 | real | 1 doc | a extrair do briefing |
| KB-10 | Boletins / laudos de campo | texto livre do técnico ao atender: o que viu, condição do tempo, foto descrita (sem pessoas nem placas) | simulado | 12 boletins | a escrever |

São **10 documentos** (mínimo pedido: 8).

## Ativos com histórico completo (personas sintéticas)

Quatro câmeras têm histórico pensado para virar casos de teste:

| TAG | Perfil | Uso no eval set |
|---|---|---|
| CAM-SP330-KM418-S | Suja muito: limpezas e teia repetidas → λ alto | C02, C04, C12, C13 |
| CAM-SP330-KM412-N | Voltada para leste: alarmes de sol de frente pela manhã (AMBIENTAL) + 1 desfoque real | C01, C10 e exemplo few-shot |
| CAM-SP330-KM421-N | Região de neblina: alarmes AMBIENTAIS | C11 |
| CAM-SP330-KM424-N | PTZ com falhas mecânicas e de sinal (peça trocada) | C11 (vizinha) |

## Como a base é usada no RAG

- **Dados estruturados (KB-01, KB-02)** não vão para o vector store. Eles são lidos pelas tools (`consultar_ativo`, `consultar_historico_falhas`, `calcular_metricas_confiabilidade`). Assim o número vem da tabela, e não de um trecho de texto parecido.
- **Textos (KB-03 a KB-10)** são quebrados em trechos (chunks) e vão para o vector store.

### Tamanho do trecho (chunk)

- **400 a 600 tokens, com 80 de sobreposição.** POP e ficha técnica são documentos de passo a passo: um trecho desse tamanho guarda um procedimento inteiro ou uma seção inteira, sem cortar a lista de segurança no meio.
- Trecho pequeno demais (≈100 tokens) separa o passo do seu pré-requisito de segurança. Trecho grande demais (>1.000) ocupa a janela de contexto com texto que não ajuda e piora a busca.
- Na consulta entram os **4 melhores trechos** (≈2.000 tokens). Ver a política de contexto no README.

### Metadados guardados em cada trecho

`doc_id`, `tipo_documento` (pop, ficha, norma, plano, matriz, glossario, briefing, boletim), `tipo_ativo`, `tag` (quando o trecho é de um ativo), `tipo_intervencao`, `data`, `versao`, `origem` (real/simulado). Com isso dá para filtrar, por exemplo, "só POP de cftv_ptz".

## O que NÃO entra na base (LGPD)

- Imagens e quadros das câmeras (podem ter rostos e placas). A base guarda só o **texto** do alarme e do laudo.
- Placas, nomes de motoristas, dados de ocupantes de veículos.
- Dados pessoais de empregados: nome do técnico, matrícula, telefone, escala. O boletim guarda só "equipe" (ex.: "Equipe CFTV-2").
- Dados contratuais e de arrecadação da concessionária.
- Qualquer dado real de operação da Motiva enquanto o projeto for acadêmico.
