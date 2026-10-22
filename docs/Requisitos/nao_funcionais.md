# Requisitos Não Funcionais

## Base e critérios de verificação

Estes requisitos complementam os [requisitos funcionais](./funcionais.md) e estabelecem critérios de qualidade para os diferentes perfis e cenários documentados nas [personas do projeto](../Personas/). O foco é a facilidade de uso, a continuidade do histórico e a confiança nas recomendações, sem vincular o produto a uma rotina ou dispositivo específico.

As metas numéricas são propostas iniciais de qualidade, não compromissos já validados ou resultados de testes. **P0** indica uma condição essencial para o fluxo principal; **P1**, uma melhoria prioritária. Limitações impostas pelo ChatGPT, pelo canal de notificação ou por outros serviços devem ser identificadas na implementação e comunicadas ao usuário quando afetarem uma operação.

## Requisitos

| ID | Categoria | Prioridade | Requisito | Critério de verificação |
| :-- | :-- | :--: | :-- | :-- |
| RNF01 | Compatibilidade | P0 | O Gym AI deve permitir o fluxo principal nos clientes do ChatGPT suportados pelo projeto, incluindo o uso pelo celular, por meio do mecanismo de invocação disponível para o plugin. | Documentar os clientes suportados e validar em cada um a configuração do perfil, consulta do treino, registro da execução e solicitação de ajuste. Documentar o mecanismo real de ativação; não presumir que uma skill, por si só, ofereça armazenamento ou notificações. |
| RNF02 | Usabilidade | P0 | As respostas devem usar português brasileiro claro e apresentar primeiro a informação necessária para a ação solicitada. | Na consulta diária, exibir o resumo e os exercícios antes de justificativas extensas. Usar nomes de exercícios, séries, repetições, unidades e intervalos explícitos; explicar siglas quando aparecerem pela primeira vez. |
| RNF03 | Legibilidade e acessibilidade | P0 | O conteúdo deve permanecer legível em telas pequenas e ser compreensível por tecnologias assistivas dentro das capacidades do cliente. | Verificar em uma área de visualização de 375 px que as informações essenciais possam ser lidas em listas ou blocos sem depender de tabelas largas, cores, emojis ou imagens. Nenhuma orientação essencial deve existir somente em conteúdo visual. |
| RNF04 | Desempenho | P1 | O serviço próprio deve responder rapidamente às operações de consulta e registro; a latência completa da conversa deve ser acompanhada separadamente. | Meta inicial: percentil 95 de até 2 segundos nas operações próprias de leitura e gravação, em 100 operações representativas com 10 usuários simultâneos e até 12 meses de histórico por usuário. Medir separadamente geração de resposta e entrega por serviços externos, sem apresentar esse limite como garantia de tempo total no ChatGPT. |
| RNF05 | Explicabilidade e qualidade dos dados | P0 | Recomendações devem distinguir dados registrados, estimativas e suposições e permitir compreender a origem dos ajustes. | Em cenários com e sem histórico, verificar que cargas realizadas não sejam confundidas com cargas sugeridas, que a duração seja identificada como estimativa e que ajustes indiquem os registros ou relatos utilizados. Não prometer resultados físicos ou prazos garantidos. |
| RNF06 | Segurança das orientações | P0 | O assistente deve respeitar os limites de orientação de musculação, sem diagnosticar, prescrever tratamento ou incentivar execução apesar de sinais de alerta. | Validar cenários de dor aguda, desmaio, dor no peito e cansaço habitual: os primeiros devem interromper a recomendação de esforço e orientar busca de atendimento adequado; o último deve permitir avaliação contextual e adaptação. A ausência de limitações no cadastro não deve invalidar um novo relato. |
| RNF07 | Persistência e integridade | P0 | Perfil, plano e registros confirmados devem permanecer disponíveis entre sessões, sem alterações silenciosas de histórico ou duplicação por repetição de uma operação. | Após reiniciar o serviço e iniciar nova conversa autenticada, recuperar os dados gravados. Repetir uma mesma requisição de gravação com o mesmo identificador não deve criar outra sessão ou série. Alterações devem conservar sua data e permitir distinguir versões do plano. |
| RNF08 | Privacidade e isolamento | P0 | Os dados devem ser acessíveis somente ao usuário autorizado e limitados ao necessário para as funcionalidades solicitadas. | Testar duas contas distintas e tentativas de acesso por identificador: nenhuma deve consultar ou alterar os dados da outra. Informações de saúde não necessárias, localização precisa e dados de terceiros não devem ser exigidos para montar um treino. |
| RNF09 | Proteção de dados | P0 | Comunicações e dados persistidos sob controle do serviço devem ser protegidos, e credenciais não devem aparecer em respostas, código versionado ou registros operacionais. | Verificar transporte por HTTPS, criptografia do armazenamento e gestão de segredos fora do repositório. Inspecionar uma amostra de logs para confirmar a ausência de tokens, conversas completas e relatos pessoais de saúde. Documentar quais dados são processados por provedores externos. |
| RNF10 | Transparência e controle dos dados | P0 | O usuário deve receber uma explicação acessível sobre os dados armazenados, sua finalidade, retenção e como consultá-los, exportá-los ou solicitar sua exclusão. | Antes da primeira persistência, disponibilizar essas informações. Validar exportação legível e exclusão dos dados ativos sob controle do serviço em até 30 dias da solicitação. Definir e informar o prazo de expiração das cópias de segurança e as limitações de exclusão em provedores externos antes da disponibilização do produto. |
| RNF11 | Tratamento de falhas | P0 | Falhas de armazenamento, consulta ou serviços externos devem ser comunicadas sem simular sucesso nem produzir recomendações que dependam de dados indisponíveis como se fossem conhecidos. | Simular indisponibilidade: uma gravação malsucedida deve informar que o registro não foi salvo; uma consulta sem histórico deve informar essa limitação; uma falha de agendamento não deve gerar mensagem de lembrete confirmado. Tentativas posteriores devem preservar a integridade de RNF07. |
| RNF12 | Confiabilidade dos lembretes | P1 | O agendamento deve respeitar fuso, mudanças da agenda e unicidade por sessão, permitindo distinguir agendamento, envio e confirmação de entrega quando disponível. | Com relógio controlado e provedor simulado, testar diferentes antecedências configuradas pelo usuário, mudança de fuso, remarcação, cancelamento e repetição de requisições. Meta inicial: 95% dos disparos ao provedor em até 5 minutos do horário programado. Não declarar recebimento no celular sem confirmação do canal. |
| RNF13 | Observabilidade | P1 | O serviço deve registrar o estado técnico de consultas, gravações e lembretes de forma suficiente para investigar falhas, preservando a privacidade. | Uma falha simulada deve poder ser localizada por identificador de operação, horário, tipo e resultado, sem registrar o conteúdo integral da conversa. Deve ser possível calcular a latência de RNF04 e o atraso de disparo de RNF12. |
| RNF14 | Manutenibilidade | P1 | As regras de planejamento devem poder evoluir sem perder os registros existentes nem depender de detalhes de um único provedor de armazenamento ou notificações. | Uma alteração de regra de progressão deve preservar sessões anteriores. Atualizações do formato de dados devem ter migração verificável; integrações devem permitir simular falhas e execução bem-sucedida sem enviar notificações reais. |

## Dependências e decisões a validar

- **Persistência:** definir o serviço e a identidade autenticada que sustentarão RF18 e o isolamento de RNF08. O histórico da conversa não deve ser tratado como um banco de dados garantido.
- **Notificações:** confirmar um canal capaz de alcançar o celular, a disponibilidade de agendamento e as permissões exigidas. A existência da interação pelo chat não comprova essa capacidade.
- **Retenção:** estabelecer os prazos do armazenamento ativo e das cópias de segurança, os responsáveis pela operação e as capacidades dos provedores antes de coletar dados reais.
- **Metas operacionais:** validar as metas propostas de latência e pontualidade com a arquitetura escolhida e ajustar os valores com evidências antes de assumir compromissos de produção.

## Relação entre necessidades e requisitos

| Necessidade atendida | Requisitos relacionados |
| :-- | :-- |
| Acesso pelo ChatGPT, incluindo uso no celular | RNF01–RNF03 |
| Pouco tempo para consultar e registrar treinos | RNF02–RNF04 |
| Sugestões realistas a partir dos resultados anteriores | RNF05, RNF07, RNF11 |
| Orientações compatíveis com o contexto e as condições relatadas pelo usuário | RNF05, RNF06 |
| Armazenamento de cargas, repetições e relatos pessoais | RNF07–RNF10 |
| Lembretes confiáveis conforme as configurações do usuário | RNF11–RNF13 |
| Ajustes contínuos sem perder o histórico | RNF07, RNF14 |
