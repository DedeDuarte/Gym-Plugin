# Requisitos Funcionais

## Escopo e prioridades propostas

O Gym é um plugin para o ChatGPT, com invocação pretendida como **@Gym**, para planejar musculação, registrar execução, acompanhar evolução e adaptar sessões. O armazenamento inicial será o **Google Sheets**. Estes são requisitos para desenvolvimento, não funcionalidades implementadas. As [personas](../Personas/) fornecem exemplos de necessidades; seus horários, objetivos e dispositivos não são restrições gerais.

- **P0 — primeira versão:** fluxo completo de vincular planilha, cadastrar perfil, aprovar e recuperar plano, consultar sessão, registrar e corrigir séries e recuperar dados em outra conversa. Inclui segurança, integridade, privacidade e falhas.
- **P1 — evoluções:** automações e análises que ampliam o fluxo básico. Continuam sendo requisitos previstos, sem compromisso de entrega definido.

Os IDs RF01–RF21 foram preservados. RF22–RF25 acrescentam integração e controle de dados. Os critérios de qualidade estão em [não funcionais](./nao_funcionais.md); as regras de armazenamento, no [contrato de dados](./contrato_dados.md). Dependências e rastreabilidade estão em [decisões e rastreabilidade](./decisoes_rastreabilidade.md).

## Perfil e planejamento

| ID | Prioridade | Requisito | Critério de aceite e erros |
| :-- | :--: | :-- | :-- |
| RF01 | P0 | Cadastrar e atualizar objetivos, experiência, situação de prática, disponibilidade, duração e limitações declaradas. | Dado um perfil incompleto, salvar os campos conhecidos e identificar os desconhecidos, sem converter ausência de resposta em “sem limitações”. Atualizar um objetivo deve preservar o histórico e não substituir automaticamente o plano vigente. |
| RF02 | P0 | Obter os dados mínimos para gerar um plano. | Exigir objetivo, experiência declarada, prática atual ou pausa, frequência pretendida, dias disponíveis, limite de duração por sessão, equipamentos, fuso e resposta sobre limitações (nenhuma declarada, relatadas ou prefere não informar). Horário exato é opcional sem lembretes. Se faltar um campo mínimo ou a frequência superar as sessões autorizadas nos dias disponíveis (uma por dia, salvo escolha explícita por mais), pedir esclarecimento antes de gerar. Se o usuário preferir não informar limitações, explicitar essa incerteza e não presumir ausência; aplicar RNF06 a qualquer relato posterior. |
| RF03 | P0 | Gerar uma proposta de rotina e torná-la vigente somente após aceite explícito. | Com RF02 atendido, apresentar sessões, sequência, grupos musculares, descanso e estimativas. Uma proposta tem ID e versão, mas não substitui o plano atual. Após aceite inequívoco da versão apresentada, persistir sua ativação e sessões; informar sucesso somente após verificar a gravação. Recusa ou falha deve manter a versão vigente anterior. |
| RF04 | P0 | Consultar sessões do plano vigente sem criar sessões ou regenerar o plano por efeito da leitura. | Duas consultas à mesma sessão devem retornar o mesmo ID, versão e prescrição, salvo alteração explicitamente aplicada entre elas. Exibir data, foco, ordem, séries, repetições, carga ou orientação para defini-la, descanso e estimativa. Aplicar as regras de “hoje” e “próximo treino” abaixo; se houver ambiguidade, pedir seleção. Sem plano, informar a ausência e oferecer RF03. |
| RF05 | P0 | Calcular uma duração estimada reproduzível. | Exibir aquecimento, tempo estimado de execução de cada série, intervalos efetivamente contados e transições. Usar a fórmula abaixo, identificando premissas. Se o total exceder o limite, não aprovar silenciosamente a proposta: oferecer redução ou revisão da disponibilidade. Se nem a versão reduzida couber, informar a impossibilidade. Não garantir duração real. |
| RF06 | P1 | Adaptar pontualmente a sessão a um novo limite de tempo. | Dado “só hoje tenho menos tempo”, propor cortes ou substituições com novo cálculo RF05; aplicar após aceite e manter os limites futuros. Se não couber, informar isso e oferecer cancelar ou remarcar. Depende de RF05, RF12 e RF14. |

## Execução e registro

| ID | Prioridade | Requisito | Critério de aceite e erros |
| :-- | :--: | :-- | :-- |
| RF07 | P0 | Explicar os exercícios prescritos quando solicitado. | Para um exercício identificado, explicar posicionamento, movimento, músculos e cuidados básicos, considerando a experiência declarada. Para nome ambíguo, identificar a variação antes de instruir. Avaliar presença dessas informações e os limites de RNF06, sem exigir texto idêntico. |
| RF08 | P1 | Propor substituições por equipamento indisponível, preferência ou desconforto. | Considerar equipamentos, limitações e função do exercício. Apresentar alternativa e diferença de estímulo, se houver, recalcular duração e aplicar somente após aceite. Se nenhuma alternativa satisfizer essas condições, informar a ausência e oferecer retirar o exercício, rever a sessão ou interromper diante de alerta; não inventar equivalência. Esclarecer alcance pontual ou recorrente por RF14. |
| RF09 | P0 | Registrar séries efetivamente realizadas separadamente da prescrição. | Aceitar registros parciais, cargas e repetições por série. Antes de gravar, resolver sessão, exercício/variação, quantidade de séries, unidade e convenção da carga. “3 de 10 com 20”, sem unidade ou convenção conhecida, deve gerar esclarecimento. “20 kg por halter” com contexto inequívoco deve registrar essa convenção. Campos opcionais ausentes ficam nulos; não copiar prescrição como execução. Repetição técnica da mesma operação não pode duplicar séries. |
| RF10 | P0 | Associar feedback opcional à sessão ou série pertinente. | Aceitar esforço percebido, cansaço, sono, desconforto e duração real, se fornecidos. Sem feedback, permitir RF09; com alvo ambíguo, perguntar antes de salvar. Relatos devem informar ajustes futuros sem alterar permanentemente o plano por si só. |
| RF11 | P0 | Consultar e corrigir registros anteriores com preservação da revisão. | Identificar sessão e série por IDs ou contexto inequívoco e apresentar valor anterior e corrigido. Uma correção confirmada deve incrementar a revisão e preservar o histórico de alteração no contrato. Se os dados mudarem durante a operação, apresentar conflito e solicitar decisão, sem sobrescrever a revisão mais recente. |
| RF12 | P0 | Controlar estados e transições das sessões, inclusive cancelamento e remarcação. | Executar as transições definidas abaixo. Consultar não altera estado; registrar uma primeira série torna a sessão parcial; concluir exige manifestação explícita. Falta e cancelamento não devem apagar execução. Transição inválida deve ser recusada com explicação e sem mudança persistida. |

## Adaptação e evolução

| ID | Prioridade | Requisito | Critério de aceite e erros |
| :-- | :--: | :-- | :-- |
| RF13 | P1 | Propor reorganização após falta. | Após RF12 marcar uma falta, propor nova sequência usando disponibilidade, recuperação e preferências. Mostrar datas afetadas e aplicar após aceite. Não duplicar volume nem concluir a sessão perdida. A falta permanece no histórico; a reposição referencia sua origem. Na primeira versão, registro da falta, cancelamento e remarcação explícita já são cobertos por RF12. |
| RF14 | P0 | Distinguir alterações pontuais de recorrentes e versionar a prescrição. | “Só hoje” gera revisão da prescrição apenas nos itens ainda não executados da sessão alvo; “daqui para frente” gera proposta de nova versão do plano. Perguntar quando o alcance for ambíguo. Após aceite, preservar versões e execuções anteriores; em sessão parcial, alterar somente itens pendentes, sem reescrever a prescrição associada às séries realizadas. Na ativação recorrente, reconciliar sessões futuras por ID, sem anexar cópias: preservar o ID ao mudar apenas a prescrição, usar RF12 ao mudar a data e explicitar cancelamentos. Itens já executados conservam sua revisão original. |
| RF15 | P1 | Sugerir progressão com referências comparáveis. | Exigir ao menos uma execução anterior com repetições, carga, unidade, convenção e identificação comparável conforme regra abaixo, além de feedback sobre esforço ou dificuldade. Sem esses dados, pedir complemento ou sugerir coleta de referência, sem aumento numérico automático. Com dados, citar sessões usadas, justificar manter/aumentar/reduzir e respeitar incrementos disponíveis; sem conhecer o incremento, não fornecer aumento exato. |
| RF16 | P0 | Considerar cansaço, sono e dificuldades relatadas nas orientações. | Diante de relato de cansaço, perguntar sobre intensidade do relato, desconfortos e sinais de alerta antes de recomendar esforço. Explicar manter, reduzir ou interromper; aplicar RNF06. Alteração de prescrição exige RF14; não registrar conclusão nem mudança recorrente somente pelo relato. A automação para reduzir uma sessão ao novo tempo depende de RF06. |
| RF17 | P1 | Apresentar evolução, frequência e adesão em período escolhido. | Aplicar as fórmulas abaixo e mostrar numerador, denominador, datas, fuso e lacunas. Separar evolução por chave de comparabilidade; não concluir composição corporal a partir de cargas ou frequência. Se o denominador da adesão for zero, mostrar “não aplicável”, sem divisão por zero. |
| RF18 | P0 | Recuperar perfil, plano e registros em outra conversa autenticada. | Com a mesma identidade e vínculo válido RF22, recuperar os mesmos IDs, plano vigente e séries sem exigir nova digitação. Uma identidade diferente não herda a associação. Sem acesso, aplicar RF23, sem usar a memória da conversa como confirmação do conteúdo da planilha. |

## Lembretes e comunicação

| ID | Prioridade | Requisito | Critério de aceite e erros |
| :-- | :--: | :-- | :-- |
| RF19 | P1 | Configurar e desativar lembretes no celular. | Exigir horário exato, fuso, antecedência, canal validado e autorização. Calcular horário do aviso a partir da sessão; identificar sessão e fornecer acesso à prescrição atual. Sem integração comprovada, informar que não foi agendado. Depende de D02 e RNF12. |
| RF20 | P1 | Reconciliar lembretes após mudança ou perda de disparo. | Remarcação substitui aviso pendente; cancelamento e conclusão antecipada o cancelam. Se o horário já passou, aplicar RNF12 e informar atraso, sem dizer que a antecedência foi cumprida. Repetir uma operação não pode criar avisos duplicados. |
| RF21 | P1 | Ajustar o tom motivador conforme preferência. | Permitir ativar ou dispensar incentivo informal, como “Gym Bro”. Sem preferência, usar tom respeitoso e claro. Em qualquer tom, vedar humilhação, culpa, depreciação corporal e incentivo a treinar com dor. Esses limites se aplicam também em P0. |

## Integração e controle dos dados

| ID | Prioridade | Requisito | Critério de aceite e erros |
| :-- | :--: | :-- | :-- |
| RF22 | P0 | Vincular uma conta Google e selecionar ou criar a planilha de armazenamento. | Por fluxo autorizado da integração escolhida, validar identidade, leitura e escrita e o contrato antes de confirmar vínculo. Exigir seleção explícita da planilha; criar somente a pedido do usuário. Planilha existente com conteúdo incompatível não deve ser sobrescrita. Persistir associação de identidade autenticada e ID estável da planilha fora da memória do chat; não armazenar credenciais nas células. Criação e escopos efetivos dependem de D01. |
| RF23 | P0 | Tratar problemas de acesso, estrutura e conflitos no Sheets. | Acesso revogado: solicitar nova autorização. Planilha excluída: informar indisponibilidade e oferecer selecionar/criar outra sem presumir recuperação. Se o provedor não distinguir exclusão de falta de acesso, declarar essa incerteza. Formato inválido: bloquear escritas e indicar campos/IDs afetados. Indisponibilidade: não afirmar gravação nem usar cache como dado atual. Edições manuais e conflitos seguem o contrato; nunca escolher silenciosamente a versão vencedora. |
| RF24 | P0 | Consultar e exportar os dados do usuário. | Permitir consulta por entidade, sessão ou período e exportação de todos os dados geridos pelo Gym em JSON estruturado, incluindo IDs, unidades, versões, revisões e fuso. Conferir contagens com a leitura validada e identificar versão do formato e data da exportação. Exportação incompleta deve ser identificada como falha, sem afirmar completude. Sem autorização, negar acesso. Não exportar outras abas ou dados de terceiros fora do contrato. |
| RF25 | P0 | Distinguir desconexão, exclusão de registros e exclusão da planilha. | Antes de operação destrutiva, apresentar alvo, alcance e efeitos e obter confirmação explícita. Desconectar remove o vínculo e as credenciais geridas pelo Gym, sem apagar células. Excluir registros remove dados e revisões selecionados, sem apagar o arquivo inteiro; preservar referências por exclusão em cascata confirmada. Apagar a planilha inteira é ação separada: só executar se a integração permitir e houver confirmação específica; caso contrário, orientar a ação pelo proprietário. Acesso revogado impede prometer exclusão; aplicar RNF10. |

## Regras verificáveis

### Datas e seleção da sessão

- **Hoje:** data local do instante da consulta no fuso IANA do perfil. Datas são persistidas como `AAAA-MM-DD`; horários com fuso seguem o contrato. Não usar o fuso do servidor como substituto.
- **Dia sem sessão:** informar que não há sessão prevista; consultar não cria uma. Sessões pendentes anteriores podem ser listadas separadamente, sem virar automaticamente o treino de hoje.
- **Múltiplas sessões:** listar ID curto, horário (se informado), foco e estado e pedir seleção antes de registrar ou alterar. Um ID explícito dispensa a pergunta.
- **Próximo treino:** primeira sessão ativa (planejada ou parcial) com data igual ou posterior a hoje, ordenada por data e horário. Sessão sem horário na mesma data de outra exige seleção, assim como horários iguais. Se não existir, informar ausência; pendências vencidas são apresentadas separadamente.
- **Virada de dia:** não transforma automaticamente sessão planejada em falta. Perguntar ao usuário seu resultado; datas vencidas sem confirmação são pendências, não execuções nem faltas presumidas.

### Duração estimada

`total_segundos = aquecimento + soma(execução estimada de cada série) + soma(intervalos previstos entre séries/exercícios) + soma(transições)`.

Cada parcela deve constar da prescrição em segundos, com indicação de valor informado ou estimado. Não contar descanso após a última série se a sessão já terminou; entre exercícios, registrar descanso e transição separadamente e não somar a mesma parcela duas vezes. A duração pode ser estimada diretamente por série; se derivada de repetições e cadência, registrar ambos. Arredondar para minutos somente depois da soma.

**Exemplo de verificação:** 300 s de aquecimento + 6 séries de 40 s + 4 intervalos de 60 s + 1 intervalo entre exercícios de 90 s + 1 transição de 60 s = 930 s = 15,5 min. O valor deve ser reproduzível a partir das parcelas, não do texto gerado pelo modelo. Esse exemplo não fixa a duração ou a estrutura de todos os treinos.

### Estados e transições

| Estado de origem | Evento explícito | Estado resultante e efeito |
| :-- | :-- | :-- |
| Planejada | Primeira série registrada | Parcial; mantém prescrição e execução separadas. |
| Planejada | Usuário confirma que terminou sem registrar séries | Solicitar ao menos um registro de execução antes de concluir; não inventar séries. |
| Parcial | Usuário confirma fim | Concluída; itens não realizados continuam sem execução, mesmo que o programa tenha ficado incompleto. |
| Planejada | Falta confirmada | Faltada; sem execução. |
| Planejada | Cancelamento confirmado | Cancelada; guardar motivo, se fornecido. |
| Planejada | Remarcação confirmada | Remarcada; criar uma sucessora planejada com novo ID e referência à origem, na mesma cadeia lógica. |
| Parcial | Usuário decide encerrar | Concluída com indicação de execução incompleta; não permitir falta, cancelamento ou remarcação que ocultem séries feitas. Itens restantes podem originar outra sessão após aceite. |
| Concluída, faltada ou cancelada | Correção explícita de estado | Exigir motivo e revisão; reabrir como planejada se não houver séries, ou parcial se houver. Não apagar execução. |
| Remarcada | Tentativa de registrar na origem | Redirecionar à sucessora e solicitar seleção se necessário. Correção de remarcação exige resolver a cadeia inteira sem criar duas sessões ativas. |

A falta e o cancelamento são distintos: falta é ausência a uma sessão que permaneceu prevista; cancelamento retira a sessão da agenda por decisão explícita. Reposição de falta cria nova cadeia com referência à sessão faltada; remarcação preserva a mesma cadeia. Toda transição é revisionada; nenhuma leitura altera estado.

### Comparabilidade, frequência e adesão

- **Chave de comparabilidade:** exercício canônico + variação + equipamento (incluindo máquina quando relevante) + unidade + convenção da carga (`total`, `por_halter`, `por_lado`, `peso_corporal` ou `assistencia`). Peso corporal e assistência não são cargas livres equivalentes. Unidade diferente só é comparável após conversão explícita registrada; ausência de convenção bloqueia comparação numérica. Séries de aquecimento são separadas das séries de trabalho.
- **Frequência realizada:** número de sessões concluídas no período, pela data real de execução, acompanhado de número de dias distintos com execução (inclusive parcial). Mostrar sessões parciais separadamente; duas sessões no mesmo dia contam como duas sessões e um dia ativo.
- **Adesão ao plano:** sessões concluídas entre as cadeias elegíveis / total de cadeias elegíveis, multiplicado por 100. Cadeias elegíveis são as cuja data prevista final está no período, incluindo concluídas, faltadas, parciais e planejadas pendentes. Canceladas são excluídas e exibidas à parte com contagem; isso deve ser mostrado para não ocultar redução do planejamento. Remarcações contam uma única vez na data da sucessora final; origens remarcadas não entram novamente. Reposição de falta não apaga a falta original. Sessões avulsas são exibidas separadamente e não entram na adesão.
- **Reprodutibilidade:** cada relatório informa instante de corte, período inclusivo e fuso. Remarcações posteriores podem mudar relatórios futuros; a exportação do relatório deve registrar o conjunto de IDs e revisões usado. Conclusão com execução incompleta conta como comparecimento concluído, sem alegar cumprimento de todo o volume prescrito.

## Limites

Prescrição aprovada e execução registrada são dados distintos. Alterações preservam o histórico, exceto exclusão solicitada pelo usuário em RF25. Nenhum nível de experiência autoriza presumir carga atual. Dietas, diagnóstico, tratamento, estimativa de percentual de gordura e integrações com relógios ficam fora deste escopo. O lembrete não condiciona o acesso à consulta do treino.

## Exemplos adicionais de aceite

- **Seleção:** dadas duas sessões planejadas para hoje, uma consulta lista ambas; “anote 10 repetições” sem identificar sessão/exercício não grava. Escolher uma delas permite continuar sem criar outra sessão.
- **Progressão:** 20 kg por halter e 20 kg totais são referências distintas; séries em máquinas diferentes também não são comparadas automaticamente. Mesmo com números iguais, RF15 deve indicar a incompatibilidade.
- **Adesão:** num período com duas cadeias concluídas, uma faltada, uma parcial, uma cancelada e uma origem remarcada cuja sucessora concluída também está no período, o denominador é cinco e o numerador três: 60%. A cancelada é exibida à parte; a origem remarcada não duplica a sucessora. A frequência é três sessões concluídas, mais uma parcial indicada separadamente. Se a sucessora mudar para fora do período, sua cadeia deixa o denominador desse período no próximo relatório, com novo instante de corte.
- **Erro de confirmação:** se RF09 gravou séries e a resposta se perdeu, a retomada consulta o mesmo ID de operação; registros completos geram confirmação do sucesso anterior, sem novas séries. Estado não determinável é informado como pendente, nunca como falha comprovada.
