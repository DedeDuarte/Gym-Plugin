# Contrato mínimo — Google Sheets

Proposta para validar a primeira integração (D01 em [decisões técnicas](./decisoes_rastreabilidade.md)). A distribuição em abas e o mecanismo de gravação serão escolhidos após o teste.

Este documento apoia a implementação; não exige formulários de cadastro. Campos, abas e autenticação serão definidos na validação técnica.

## Dados necessários

| Registro | Campos obrigatórios | Opcionais |
| :-- | :-- | :-- |
| Vínculo | Identidade autenticada, ID da planilha e referência da autorização. | Nenhum. |
| Documento | Versão do formato e versão do plano vigente, nula antes da aprovação. | Nenhum. |
| Perfil | ID e contexto de treino de RF01/RF02 para gerar o plano. | Horários e preferências; dados ainda desconhecidos podem ficar vazios na coleta inicial. |
| Plano e versões | IDs do plano e da versão, estado (proposta, aprovada, substituída ou rejeitada), período, agenda e perfil usado. | Versão anterior e motivo da mudança; data do aceite obrigatória na aprovação. |
| Sessão | ID, versão da prescrição, data prevista, fuso, estado e prescrição usada. | Horário, data real, feedback e referência de remarcação ou reposição; data real obrigatória com execução. |
| Exercício prescrito | ID, sessão/versão, exercício, variação, equipamento, ordem, séries, repetições, descanso e parcelas da duração estimada. | Carga sugerida ou orientação para defini-la. |
| Série realizada | ID, sessão, exercício/variação/equipamento, ordem, tipo (aquecimento ou trabalho), repetições e convenção da carga. | Referência à prescrição quando houver, carga, esforço, duração e feedback; carga e unidade obrigatórias com peso externo. |
| Correção | Registro alterado, revisão, valores anterior e novo e data. | Motivo. |

## Convenções

- IDs são estáveis; posição da linha não identifica registros. Referências ligam séries, sessões e versões sem depender da ordem das linhas.
- Cada registro alterável tem revisão e data de atualização. Cada gravação tem um identificador para reconhecer tentativas repetidas, conforme RNF07.
- Datas: `AAAA-MM-DD`; instantes: ISO 8601; fuso: nome IANA. Horário pode ficar vazio sem lembrete; horário ambíguo exige esclarecimento.
- Duração em segundos; carga numérica em `kg` ou `lb`, com convenção `total`, `por_halter`, `por_lado`, `peso_corporal` ou `assistencia`. Sem peso externo, carga fica vazia; mais assistência não significa mais força.
- Dados desconhecidos ficam vazios, sem inventar zero ou “nenhuma”; limitações distinguem nenhuma declarada, relatadas e não informadas.

Credenciais e vínculo ficam fora das células compartilháveis (RNF08/RNF09). Segurança e histórico seguem os [RNF](./nao_funcionais.md); aprovação, estados e edições manuais seguem os [RF](./funcionais.md).

## Teste da primeira integração

Usar uma planilha de teste para vincular, cadastrar perfil, aprovar plano, consultar sessão, registrar e corrigir séries e recuperar tudo em outra conversa. Reordenar linhas deve preservar as referências.

Repetir uma gravação, perder sua confirmação, interrompê-la parcialmente e alterar o mesmo registro pelo chat e pela planilha. Validar RNF07/RNF11 e a confirmação de edições manuais de RF23 antes de liberar o fluxo de escrita.
