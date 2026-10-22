# Contrato mínimo de dados — Google Sheets

## Status e organização proposta

Este contrato é uma **proposta para a primeira versão**, vinculada a RF22 e RNF07; não descreve uma integração existente. A escolha das ferramentas está pendente em D01, em [decisões e rastreabilidade](./decisoes_rastreabilidade.md). O desenho deve ser validado por prova de capacidade antes da implementação. Não pressupõe transações, bloqueios, agendamento ou recuperação automática oferecidos pela plataforma.

Recomenda-se uma planilha por vínculo de usuário, com abas lógicas abaixo. A identidade de um registro é seu ID estável, jamais o número da linha, a ordem das abas ou o nome do arquivo. Ordenar linhas não deve alterar referências. O ID da planilha é parte do vínculo; renomeá-la não deve criar outro vínculo.

O vínculo proposto é um registro protegido externo às células: `subject_id` da identidade autenticada do Gym, referência da identidade Google autorizada, `spreadsheet_id`, referência da credencial autorizada e estado do vínculo. Sua recuperação entre conversas é requisito, não capacidade presumida do ChatGPT. Uma única planilha ativa por usuário e um único vínculo Gym ativo por planilha são os limites iniciais propostos; troca explícita de planilha não migra nem mescla dados automaticamente. Não usar e-mail digitado pelo usuário como prova de identidade.

## Convenções comuns

- `schema_version` inicial proposto: `1.0`. Metadados declaram a versão do documento; todos os registros usam esse formato. Versão desconhecida bloqueia escrita até validação/migração por RNF14.
- IDs são UUIDs opacos, imutáveis e únicos por entidade. Referências devem apontar para registros existentes no mesmo vínculo. `revision` é inteiro positivo incrementado em toda alteração; IDs e revisões são validados antes de escrever.
- Todas as entidades mutáveis têm `created_at`, `updated_at` (instantes ISO 8601 em UTC), `revision` e `last_operation_id`. Datas locais usam `AAAA-MM-DD`; fuso usa nome IANA, como `America/Sao_Paulo`. Horário local usa `HH:MM` e pode ser nulo; se presente, guardar também o instante UTC resolvido. Horário inexistente ou ambíguo por mudança de fuso exige esclarecimento.
- Campo obrigatório deve estar preenchido e passar pela validação. Campo opcional desconhecido é nulo, sem usar zero, texto vazio ou “nenhuma” para disfarçar ausência. No Sheets, célula vazia representa nulo em campo opcional; listas e objetos são JSON validado. Números são valores numéricos, não texto com unidade embutida. Na exportação, usar JSON e nulos explícitos.
- Cargas são números não negativos com unidade `kg` ou `lb` e convenção explícita. Sem carga externa: `carga = null`, com convenção `peso_corporal`. Assistência usa valor positivo e convenção `assistencia`; não inferir que mais assistência significa mais força. Duração em segundos; repetições e ordens são inteiros positivos. Esforço opcional, quando numérico, usa escala RPE 1–10; texto livre não é convertido automaticamente em número.
- Campos de texto provenientes da conversa são dados, não instruções ou fórmulas. Gravação deve impedir interpretação de texto como fórmula de planilha. Não guardar tokens, segredos ou conteúdo integral das conversas.
- Dados de prescrição são snapshots versionados; execução referencia a versão usada. Corrigir execução não altera a prescrição; aprovar outra versão não reescreve séries antigas.

## Entidades e campos

Todos os campos de identidade e referência citados nesta tabela são obrigatórios, salvo indicação explícita de opcionalidade. Os campos comuns de auditoria são obrigatórios nas entidades mutáveis. O registro técnico do vínculo não deve ficar em uma aba compartilhável.

| Aba lógica | Campos obrigatórios específicos | Campos opcionais e validação |
| :-- | :-- | :-- |
| `metadados` | `document_id`, `schema_version`, `timezone`, `active_plan_version_id` (nulo antes da primeira ativação), `document_revision` | `migration_state`. Deve existir um único registro; ponteiro de plano deve apontar para versão aprovada. |
| `perfil` | `profile_id`, `objectives`, `experience`, `practice_status`, `weekly_frequency`, `available_weekdays`, `session_limit_seconds`, `equipment`, `limitations_status`, `timezone` | `pause_description`, `limitations_description`, horários por dia, preferências de exercício e tom. Antes de RF02 terminar, campos mínimos ainda não preenchidos podem ser nulos e o perfil fica `incompleto`; `profile_status` obrigatório distingue `incompleto` e `pronto`. Frequência deve ser positiva e não superar as sessões autorizadas nos dias disponíveis; assumir uma sessão por dia somente se o usuário não tiver solicitado mais. Guardar `sessions_per_weekday` quando houver múltiplas sessões no mesmo dia; limite deve ser positivo. `limitations_status`: `nenhuma_declarada`, `relatadas` ou `nao_informadas`; `relatadas` exige descrição. |
| `planos` | `plan_id`, `profile_id` | Nome do plano. Identidade lógica estável entre versões. |
| `versoes_plano` | `plan_version_id`, `plan_id`, `version_number`, `status`, snapshot do perfil usado, `schedule_spec`, `estimation_method_version` | `previous_version_id`, `accepted_at`, motivo de alteração. Estados: `proposta`, `aprovada`, `substituida`, `rejeitada`. Aceite exige `accepted_at`; só uma versão é vigente no ponteiro de metadados. `schedule_spec` descreve dias, ordem e sessões de uma janela com início e fim explícitos; novos períodos exigem aprovação, não nascem de consultas. |
| `sessoes` | `session_id`, `chain_id`, `plan_version_id`, `scheduled_date`, `timezone`, `status`, `prescription_revision`, `prescription_snapshot`, `is_ad_hoc` | Horário e instante UTC, `rescheduled_from_id`, `replacement_for_id`, `actual_started_at`, `actual_finished_at`, `actual_date`, feedback e motivo de transição. `actual_date` obrigatória ao registrar execução; confirmar data se divergente da prevista. Estados e cadeia seguem RF12. `is_ad_hoc=true` pode ter `plan_version_id=null` e exige prescrição própria aprovada. |
| `exercicios` | `exercise_id`, nome canônico, `variation`, `equipment_key` | Descrição. Catálogo é do vínculo; variação e equipamento não podem ser omitidos em comparações. Alteração semântica de identidade exige novo ID, preservando o anterior. |
| `itens_prescritos` | `prescription_item_id`, `plan_version_id`, `session_template_key`, `exercise_id`, ordem, especificação de séries, faixa de repetições, intervalos, transições e parcelas de execução estimada em segundos | Carga sugerida, unidade e convenção (obrigatórias quando há carga numérica), orientação sem carga de referência. Aquecimento da sessão consta em `schedule_spec` e no snapshot. Snapshot da sessão preserva esses itens e a chave de comparabilidade, inclusive em ajustes pontuais. |
| `series_executadas` | `set_id`, `session_id`, `prescription_revision`, `prescription_item_id`, `exercise_id`, ordem da série, `set_type`, repetições reais, convenção da carga, data real de execução | Carga/unidade obrigatórias com carga externa, esforço, duração, feedback. `set_type`: `aquecimento` ou `trabalho`. Se exercício não prescrito foi realizado, `prescription_item_id=null` e a divergência deve ser explícita, com chave de comparabilidade completa. Guardar a revisão da prescrição efetivamente usada e nunca a substituir por uma versão posterior. |
| `revisoes` | `change_id`, entidade, `record_id`, revisão anterior e nova, `operation_id`, tipo de ator (`usuario_chat`, `edicao_manual_importada`, `sistema`), instante, campos alterados e valores anteriores/novos | Motivo. Preserva correções e snapshots, mas contém dados pessoais e participa da exclusão RF25. Não confundir esta aba com logs técnicos. |
| `operacoes` | `operation_id`, tipo, estado, manifesto de IDs/revisões esperadas e resultantes, instante, checksum dos dados gravados | Estados: `pendente`, `confirmada`, `conflito`, `falha`, `resultado_incerto`. Manifesto serve para reconciliação e não deve conter conversas ou tokens. A persistência confiável desse controle depende de D01. |

## Ativação, identidade e validação

1. Confirmar identidade por credencial da integração e autorizar acesso ao arquivo escolhido. Validar leitura e escrita por operação controlada em área reservada e limpeza do marcador; não alterar dados de treino para testar permissão.
2. Para planilha nova, criar somente após pedido, inicializar abas e versão e validar o resultado antes de associar o vínculo. Para existente, verificar formato e conteúdo antes de propor inicialização; abas alheias ao Gym permanecem fora da gestão e não são apagadas.
3. Validar unicidade dos IDs, referências, revisões, tipos, unidades, estados e um único plano vigente. Duplicatas de ID, referência quebrada ou versão desconhecida bloqueiam escrita; relatar entidade e campo, evitando expor dados de terceiros.
4. Materializar a janela aprovada do plano uma única vez com IDs previamente definidos. Somente confirmar ativação quando plano, sessões e ponteiro tiverem sido reconciliados. Leitura não estende janela nem cria versões. Erro parcial mantém operação pendente e impede uso de uma ativação incompleta como plano válido.

## Edições manuais e concorrência

**Política proposta:** aceitar edições manuais válidas, sem adotar “última escrita vence”. Antes de cada mutação e recuperação, comparar conteúdo e revisões com o último estado validado. Detectar mudanças mesmo que o usuário não tenha incrementado `revision`, por checksum normalizado (ordem de linha irrelevante). Edição de um ID existente é inválida; para registro novo, fornecer novo ID ou passar por importação validada, sem usar posição como identidade.

- Mudança manual válida sem operação concorrente: mostrar diferenças, importar após confirmação, registrar revisão e revalidar referências. Até a reconciliação, apresentar divergência; não usar silenciosamente valores alterados como histórico validado.
- Alterações no mesmo registro ou em dependências entre a leitura e a gravação: interromper, reler e apresentar as duas propostas. Exigir escolha ou combinação explícita; reaplicar sobre a revisão mais recente. Registros independentes só podem ser combinados após validar todas as referências.
- Alteração manual inválida ou remoção de linha referenciada: bloquear escrita e orientar reparo/importação ou exclusão por RF25. Não recriar linha nem apagar dependentes automaticamente.
- Dados importados manualmente que alterem uma prescrição já executada exigem preservar a versão anterior; não reescrever snapshots. Se não houver snapshot recuperável, marcar a lacuna e bloquear comparações afetadas.
- Uma leitura em nova conversa não deve aceitar cegamente uma planilha editada. A estratégia de checksum e o último estado validado precisam sobreviver entre conversas; seu local protegido de persistência é parte de D01.

## Escritas, repetição e falhas

Cada intenção confirmada recebe `operation_id` estável. Retentativas técnicas conservam esse ID; uma intenção nova recebe outro. Quando a conversa não permitir distinguir repetição do relato de uma nova série, perguntar, sem deduplicar séries legítimas só porque os valores coincidem.

O mecanismo escolhido deve serializar mutações do mesmo vínculo, verificar revisão esperada e detectar alterações externas; bloqueios internos não impedem edição manual no Sheets. Recomendação: um único escritor controlado pelo Gym, área de preparação e manifesto de operação, validação anterior e posterior à publicação. A publicação de uma operação em várias abas não deve ser apresentada como transação nativa do Sheets. A prova de capacidade de D01 deve demonstrar ausência de sobrescrita concorrente silenciosa; se não demonstrar, a integração não satisfaz P0 e o fluxo de escrita não pode ser liberado como confiável.

| Falha | Resultado exigido |
| :-- | :-- |
| Gravação terminou, resposta se perdeu | Consultar manifesto e registros pelo `operation_id`; se completos e validados, retornar sucesso já ocorrido sem anexar novas linhas. |
| Apenas parte das linhas foi gravada | Não declarar sucesso nem publicar estado incompleto; completar idempotentemente a operação ou reverter somente seus efeitos comprovados, preservando mudanças externas. Bloquear operações dependentes até reconciliação. |
| Timeout sem saber o resultado | Marcar `resultado_incerto`, avisar que a confirmação está pendente e consultar antes de repetir. Não afirmar que nada foi salvo. |
| Duas gravações com mesma revisão esperada | Uma pode ser confirmada; a outra deve receber conflito ou ser revalidada explicitamente. Não perder correção de outro cliente. |
| Conta revogada, arquivo inacessível ou serviço indisponível | Interromper mutação e informar o diagnóstico comprovável, sem usar uma cópia antiga como se fosse atual. Reautorizar ou selecionar outro arquivo por RF22/RF23. |

## Desconexão, exclusão e responsabilidades

- **Desconectar:** invalidar associação e remover credenciais mantidas pelo Gym. Explicar como revogar também a autorização Google caso a ferramenta não ofereça essa ação. A planilha e o compartilhamento existente permanecem.
- **Excluir registros:** apresentar IDs e dependências antes de confirmar; remover dados selecionados, suas revisões, snapshots e cópias geridas pelo Gym que contenham os mesmos dados, conforme RNF10. Manter, se necessário, recibo técnico sem conteúdo pessoal. Dependências não podem ficar quebradas: limpar o ponteiro de plano vigente se a versão for excluída; a exclusão de perfil/plano deve apresentar a cascata de versões, sessões e séries antes do aceite. Uma exclusão parcial não confirmada permanece pendente de reconciliação.
- **Excluir planilha:** operação separada que afeta todo o arquivo, inclusive abas fora do contrato. Exigir confirmação específica e capacidade comprovada; se indisponível, o proprietário executa no Google. Não confundir mover para lixeira com exclusão definitiva.
- **Gym:** autorizar ferramentas, limitar dados acessados e guardados, proteger credenciais e armazenamento próprio, informar falhas e cumprir exclusão nas cópias sob seu controle.
- **Google Sheets/integração:** mecanismos de acesso, disponibilidade, versionamento, lixeira e retenção seguem as capacidades e condições do provedor, ainda a validar. Não prometer exclusão de versões ou backups fora do controle do Gym.
- **Usuário/proprietário:** decide compartilhamentos e edições diretas, controla cópias/exportações que criar e ações disponíveis apenas na conta Google. Compartilhar a planilha pode expor os dados aos destinatários, mesmo que o Gym isole usuários em suas ferramentas. O Gym deve explicar isso antes do vínculo.
