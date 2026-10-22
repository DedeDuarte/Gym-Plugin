# Requisitos Funcionais

## Base e escopo

Estes requisitos descrevem o comportamento esperado do Gym AI para organizar e acompanhar treinos de musculação pelo chat. As [personas do projeto](../Personas/) orientam sua evolução; objetivos, experiência, rotina e preferências devem ser tratados como dados de cada usuário, sem assumir um único perfil de uso.

O fluxo principal é: configurar o perfil → planejar a semana → consultar o treino → registrar a execução → acompanhar a evolução → ajustar os próximos treinos.

**Prioridades propostas:** **P0** = essencial para o fluxo principal; **P1** = complemento prioritário. A classificação é uma proposta de implementação, sujeita à revisão conforme as necessidades documentadas nas personas. Os critérios abaixo são condições para aceitar a funcionalidade, não evidência de que ela já está implementada.

## Perfil e planejamento

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF01 | P0 | O sistema deve cadastrar e permitir atualizar objetivos, experiência, período sem treinar quando aplicável, disponibilidade semanal, duração das sessões e limitações informadas pelo usuário. | Deve permitir registrar e atualizar os dados de cada usuário sem impor objetivo, nível de experiência, frequência ou duração de treino fixos. Campos desconhecidos devem permanecer identificados como não informados; ausência de informação sobre limitações não deve ser tratada como ausência de limitações. |
| RF02 | P0 | O sistema deve coletar os dias e horários disponíveis, os equipamentos acessíveis e as preferências necessárias para elaborar o plano. | Antes de fechar uma agenda ou prescrever um exercício que dependa de equipamento, deve obter a informação ou apresentar uma suposição explícita e ajustável. Dias, horários e equipamentos não informados não podem ser tratados como conhecidos. |
| RF03 | P0 | O sistema deve gerar uma rotina semanal compatível com o perfil, distribuindo as sessões e os grupos musculares ao longo dos dias disponíveis. | Deve apresentar as sessões, sua sequência, os grupos trabalhados e os dias de descanso, respeitando os dias disponíveis e a frequência acordada com o usuário. Deve considerar a experiência e eventuais pausas informadas e explicar brevemente a organização proposta. |
| RF04 | P0 | O sistema deve gerar e permitir consultar o treino do dia, usando o plano vigente, o histórico disponível e os ajustes registrados. | A consulta deve apresentar data, foco, exercícios em ordem, séries, repetições ou faixas, carga sugerida ou orientação para defini-la, descanso e duração total estimada. Sem histórico, deve explicitar a ausência de uma carga de referência. |
| RF05 | P0 | O sistema deve estimar a duração da sessão, considerando aquecimento, execução, intervalos e transições entre exercícios. | Cada treino deve apresentar uma estimativa compatível com o limite informado. Se não for possível acomodar a sessão planejada, deve propor uma versão reduzida e explicar o que foi priorizado. |
| RF06 | P0 | O sistema deve adaptar o treino quando o usuário informar uma mudança de tempo disponível no próprio dia. | Ao receber um novo limite de tempo para o dia, deve recalcular a sessão para esse limite, apresentar os cortes ou substituições e preservar a duração previamente configurada para as sessões seguintes, salvo pedido contrário. |

## Execução e registro

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF07 | P0 | O sistema deve explicar a execução dos exercícios prescritos quando solicitado. | A explicação deve incluir posicionamento, movimento, músculos trabalhados e cuidados básicos, em linguagem compatível com a experiência do usuário. |
| RF08 | P0 | O sistema deve substituir exercícios mediante indisponibilidade de equipamento, preferência ou desconforto relatado. | Deve apresentar uma alternativa, explicar sua função na sessão e atualizar a estimativa de duração. Em caso de desconforto, deve aplicar os limites de segurança de RNF06. |
| RF09 | P0 | O sistema deve registrar os exercícios efetivamente realizados, incluindo séries, repetições e cargas com suas unidades. | Deve aceitar registros parciais ou completos pelo chat e preservar diferenças entre séries. “3 séries de 10 com 20 kg” deve gerar três séries registradas; dados ausentes não devem ser inventados. |
| RF10 | P0 | O sistema deve registrar o feedback relevante da sessão, como esforço percebido, cansaço, sono informado, desconfortos e duração real, quando fornecidos. | O feedback deve ficar associado à sessão correspondente e disponível para os ajustes futuros. Deve ser possível registrar o treino sem preencher todos esses campos. |
| RF11 | P0 | O sistema deve permitir consultar e corrigir registros de sessões anteriores. | O usuário deve conseguir localizar uma sessão por data e corrigir uma carga ou repetição. Se houver mais de uma sessão possível, o sistema deve esclarecer qual será alterada antes de gravar a correção. |
| RF12 | P0 | O sistema deve diferenciar os estados planejado, parcialmente realizado, concluído, faltado e remarcado de uma sessão. | Consultar um treino ou receber um lembrete não deve marcar sua conclusão. Uma sessão parcialmente registrada deve manter a execução conhecida sem assumir que os demais exercícios foram realizados. |

## Adaptação e evolução

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF13 | P0 | O sistema deve reorganizar a sequência de sessões após uma falta, considerando os próximos dias disponíveis e o histórico de execução. | Ao receber “faltei hoje”, deve marcar a falta, propor uma reorganização conforme a disponibilidade, a recuperação e as preferências informadas pelo usuário, mostrando o impacto na agenda. Não deve acumular automaticamente duas sessões no mesmo dia nem tratar a sessão perdida como realizada. |
| RF14 | P0 | O sistema deve distinguir alterações para uma sessão específica de alterações recorrentes no plano. | “Só hoje” deve afetar apenas a sessão indicada; “daqui para frente” deve atualizar as sessões futuras correspondentes e preservar o histórico executado. Se o alcance estiver ambíguo, deve esclarecê-lo antes de salvar a mudança. |
| RF15 | P0 | O sistema deve sugerir manutenção, aumento ou redução de cargas e repetições com base em registros comparáveis, esforço e feedback disponíveis. | A sugestão deve citar a sessão de referência e explicar a decisão. Sem dados suficientes ou sem conhecer os incrementos de carga disponíveis, deve explicitar a incerteza e orientar como definir a próxima tentativa, sem inventar precisão. |
| RF16 | P0 | O sistema deve considerar cansaço, sono ruim e dificuldade de execução no ajuste do treino atual e dos próximos treinos pertinentes. | Ao receber um relato de cansaço, deve esclarecer o necessário para propor manter, reduzir ou interromper a sessão, explicar a escolha e registrar o contexto relevante. Um relato isolado não deve alterar permanentemente o plano sem indicação do usuário. |
| RF17 | P0 | O sistema deve apresentar a evolução do usuário em um período solicitado. | Deve exibir sessões planejadas e realizadas, frequência e evolução de cargas e repetições por exercício, com datas e unidades. Comparações devem distinguir exercícios e condições diferentes e indicar lacunas; não devem concluir mudanças na composição corporal apenas a partir de cargas ou frequência. |
| RF18 | P0 | O sistema deve reutilizar o perfil, o plano vigente e o histórico persistido nas consultas posteriores do mesmo usuário. | Em uma nova conversa autenticada, deve recuperar o próximo treino e os últimos registros pertinentes sem exigir que o usuário os digite novamente. Deve indicar quando o histórico estiver indisponível. |

## Lembretes e comunicação

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF19 | P1 | O sistema deve permitir ativar, configurar e desativar lembretes para o celular, com antecedência definida pelo usuário em relação ao início previsto do treino. | Deve obter horário do treino, antecedência desejada, fuso e canal disponível. Deve calcular o horário do lembrete a partir dessas configurações, identificar a sessão e fornecer acesso ao treino atualizado. A ativação depende de integração de agendamento e notificações e da autorização necessária do usuário. |
| RF20 | P1 | O sistema deve atualizar ou cancelar lembretes pendentes quando a sessão for remarcada, cancelada ou realizada antecipadamente. | Uma remarcação deve substituir o agendamento anterior sem duplicá-lo. Se a mudança ocorrer depois do horário calculado para o lembrete, deve informar que a antecedência configurada não poderá ser cumprida e oferecer aviso imediato, quando suportado. |
| RF21 | P1 | O sistema deve permitir ajustar o tom da comunicação motivadora conforme a preferência do usuário. | Deve permitir adotar ou dispensar um tom informal de incentivo, como o estilo “Gym Bro”, sem impô-lo a todos os usuários. Deve incentivar a retomada após faltas e manter orientações claras. A cobrança não deve incluir humilhação, culpa, comentários depreciativos sobre o corpo ou incentivo a treinar com dor. |

## Regras de negócio e limites

- **Plano e execução são distintos:** sugestões não devem substituir os resultados efetivamente registrados. Mudanças futuras devem preservar as sessões já executadas.
- **Tempo é uma restrição por sessão:** a duração deve seguir a disponibilidade informada pelo usuário, ajustável a qualquer momento. A estimativa não é uma garantia de duração real.
- **Faltas exigem replanejamento:** a reorganização deve considerar as preferências do usuário, a disponibilidade e a recuperação, sem compensação automática de volume.
- **Progressão depende de evidências:** o nível de experiência informado não autoriza presumir cargas ou capacidade atual, inclusive após uma pausa.
- **Horários dependem de configuração:** dias, horários, fuso, canal e antecedência dos lembretes devem ser definidos conforme a rotina e as preferências de cada usuário. A consulta ao treino deve estar disponível independentemente do recebimento de lembretes.
- **Escopo inicial:** planejamento e acompanhamento de musculação. Dietas, diagnóstico, tratamento, estimativa de percentual de gordura e integrações com relógios ou aplicativos de saúde ficam fora deste escopo.

## Relação entre necessidades e requisitos

| Necessidade atendida | Requisitos relacionados |
| :-- | :-- |
| Planejamento compatível com objetivos, experiência e eventuais pausas | RF01, RF03, RF15 |
| Organização dos treinos conforme a disponibilidade semanal e o tempo por sessão | RF02–RF06 |
| Consulta ao treino do dia conforme a rotina do usuário | RF04, RF18 |
| Registrar pesos, repetições e como se sentiu | RF09–RF12 |
| Falta de visualização da evolução e dúvida sobre quando progredir | RF15, RF17 |
| Faltas e mudanças pontuais ou recorrentes pelo chat | RF13, RF14 |
| Cansaço, sono ruim e dúvidas sobre intensidade ou descanso | RF10, RF16 |
| Recomendações que aproveitem treinos anteriores | RF04, RF15, RF18 |
| Lembretes no celular conforme o horário e a antecedência configurados | RF19, RF20 |
| Interação pelo ChatGPT e comunicação conforme a preferência do usuário | RF21; RNF01, RNF02 |
