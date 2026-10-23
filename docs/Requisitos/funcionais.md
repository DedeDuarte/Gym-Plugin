# Requisitos funcionais

O Gym deve ajudar a planejar, executar e adaptar treinos pelo chat. **P0** é o núcleo da primeira versão; **P1** são complementos. As [personas](../Personas/) orientam as necessidades, sem impor seus horários ou objetivos a todos.

## Planejar e consultar

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF01 | P0 | Conhecer e atualizar o contexto de treino do usuário pelo chat. | Registrar objetivo, experiência, prática atual ou pausa, disponibilidade, equipamentos e limitações declaradas. Perguntar o que faltar para planejar, sem exigir um formulário de cadastro. |
| RF02 | P0 | Definir a disponibilidade para os treinos. | Combinar frequência, dias e tempo por sessão. Esclarecer incompatibilidades e informações desconhecidas antes de fechar o plano. |
| RF03 | P0 | Montar uma rotina semanal conforme o contexto do usuário. | Apresentar sequência das sessões, grupos musculares e descanso, considerando experiência e retorno após pausa. Tornar o plano vigente após aceite. |
| RF04 | P0 | Apresentar o treino do dia usando o plano e o histórico. | Mostrar exercícios em ordem, séries, repetições, carga sugerida ou orientação inicial, intervalos e duração estimada. Consultar novamente não cria outro treino nem muda a prescrição por si só. |
| RF05 | P0 | Planejar sessões dentro do tempo disponível. | Estimar aquecimento, execução, intervalos e transições. Se a sessão não couber, propor uma versão reduzida e explicar o que foi priorizado. |
| RF06 | P0 | Adaptar o treino quando o tempo disponível mudar no dia. | Ao receber “hoje tenho 25 minutos”, propor uma sessão para esse limite. Aplicar após aceite, preservando a duração habitual dos próximos treinos. |

## Executar e registrar

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF07 | P0 | Explicar como executar os exercícios. | Quando solicitado, explicar posicionamento, movimento, músculos trabalhados e cuidados básicos. Esclarecer nomes ou variações ambíguas. |
| RF08 | P0 | Substituir exercícios quando necessário. | Considerar equipamento indisponível, preferência ou desconforto, explicando a alternativa e seu efeito na sessão. Se não houver alternativa compatível, propor rever a sessão; desconfortos seguem RNF06. |
| RF09 | P0 | Registrar o que foi realizado em cada série. | Aceitar séries, repetições e cargas diferentes e treinos incompletos. Perguntar sobre exercício, unidade ou convenção da carga quando houver ambiguidade, sem copiar a prescrição como resultado. |
| RF10 | P0 | Registrar como o usuário se sentiu durante o treino. | Associar esforço, cansaço, sono, desconforto e duração real à sessão ou série, quando informados. Esses campos não são obrigatórios para registrar o treino. |
| RF11 | P0 | Consultar e corrigir treinos anteriores. | Localizar a sessão por data ou contexto, esclarecer ambiguidades e mostrar a correção antes de aplicá-la. Preservar o histórico de alterações conforme RNF07. |
| RF12 | P0 | Distinguir treino planejado, parcial, concluído, faltado, cancelado e remarcado. | A primeira série registrada torna o treino parcial; o usuário informa o encerramento. Consultar não conclui o treino e a passagem do dia não presume falta. Remarcações mantêm referência à data anterior; nenhuma mudança de estado apaga séries realizadas. |

## Adaptar e acompanhar

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF13 | P0 | Reorganizar os próximos treinos após uma falta. | Ao receber “faltei hoje”, registrar a falta e propor a nova sequência conforme disponibilidade e recuperação. Mostrar as datas afetadas e aplicar após aceite, sem acumular automaticamente sessões ou volume. |
| RF14 | P0 | Diferenciar mudanças só para hoje de mudanças no plano. | “Só hoje” afeta a sessão indicada; “daqui para frente” afeta os treinos futuros. Perguntar quando o alcance for ambíguo e preservar o que já foi executado. |
| RF15 | P0 | Sugerir manutenção, aumento ou redução de cargas e repetições. | Usar execuções comparáveis e feedback de esforço, citando a referência e explicando a sugestão. Sem histórico suficiente ou incrementos conhecidos, indicar a incerteza e orientar a próxima tentativa sem inventar um aumento exato. |
| RF16 | P0 | Adaptar a orientação ao cansaço, sono e dificuldades relatadas. | Esclarecer o relato e propor manter, reduzir ou interromper a sessão conforme RNF06. Um relato isolado não altera permanentemente o plano sem pedido do usuário. |
| RF17 | P0 | Mostrar a evolução em um período solicitado. | Apresentar frequência, sessões previstas e realizadas e evolução de cargas e repetições por exercício, com datas e unidades. Separar condições não comparáveis e indicar lacunas. |
| RF18 | P0 | Reutilizar o contexto e o histórico nos próximos atendimentos. | Em outra conversa com acesso autorizado, recuperar o plano e os registros para orientar o próximo treino sem pedir nova digitação. Informar quando os dados não estiverem disponíveis. |

## Complementos

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF19 | P1 | Enviar lembretes do treino no celular. | Permitir ativar ou desativar e escolher horário, fuso e antecedência. Só confirmar o agendamento após verificar um canal e agendador disponíveis. |
| RF20 | P1 | Atualizar lembretes quando a sessão mudar. | Remarcar ou cancelar o aviso correspondente sem duplicá-lo. Informar quando a antecedência desejada já não puder ser cumprida. |
| RF21 | P1 | Ajustar o tom motivador à preferência do usuário. | Permitir adotar ou dispensar incentivo informal, como “Gym Bro”, respeitando RNF06. |

## Apoio: salvar e controlar os dados

Estes requisitos sustentam o acompanhamento. Autenticação, permissões e servidor são tratados nas [decisões técnicas](./decisoes_rastreabilidade.md).

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RF22 | P0 | Salvar contexto, plano e registros no Google Sheets. | Usar a planilha escolhida pelo usuário e recuperar a mesma associação entre conversas. Criar uma planilha somente a pedido, sem sobrescrever conteúdo alheio ao Gym. |
| RF23 | P0 | Ajudar o usuário quando houver problema na planilha. | Informar como restabelecer acesso ou corrigir dados inválidos. Edições manuais válidas podem ser incorporadas após confirmação; em conflito, pedir escolha antes de sobrescrever. |
| RF24 | P1 | Permitir consultar e exportar os dados do Gym. | Consultar por sessão ou período e exportar os dados geridos pelo Gym com datas, unidades e identificação do formato. Indicar quando a exportação estiver incompleta. |
| RF25 | P0 | Permitir desconectar a planilha e solicitar exclusão dos dados. | Explicar o alcance e pedir confirmação antes de excluir. Desconectar não apaga células; apagar a planilha inteira é uma ação distinta, disponível somente se a integração permitir. |

## Regras de treino

- **Datas:** “hoje” usa o fuso do usuário. Sem sessão prevista, informar isso; havendo mais de uma, pedir seleção antes de registrar ou alterar. Pendências anteriores não viram automaticamente o treino de hoje.
- **Comparação:** considerar exercício, variação, equipamento, unidade e convenção da carga (total, por halter, por lado, peso corporal ou assistência). Separar aquecimento de trabalho e explicitar conversões.
- **Frequência:** mostrar sessões concluídas e dias com execução no período; apresentar parciais separadamente. Se houver percentual de adesão, mostrar numerador e denominador, contar remarcações uma vez e indicar cancelamentos à parte.
- **Mudanças:** explicar a proposta e seu alcance antes de aplicá-la. Plano sugerido e execução registrada permanecem distintos.

Dietas, diagnóstico, tratamento, estimativa de gordura corporal e integração com relógios ficam fora do escopo.
