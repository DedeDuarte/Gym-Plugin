# Decisões propostas e rastreabilidade

## O que está definido e o que é proposta

O contexto solicitado define o Gym como plugin para ChatGPT, invocado como @Gym, com armazenamento inicial no Google Sheets. Os documentos descrevem requisitos, não funcionalidades implementadas nem capacidades já verificadas da plataforma. O banco de dados continua sendo uma possibilidade futura, sem decisão de migração.

As [personas](../Personas/) orientam necessidades e cenários. Atualmente, [P01](../Personas/persona1.md) é a fonte documentada; suas características individuais são exemplos, não valores padrão para todos. Novas personas podem acrescentar necessidades e levar à revisão das prioridades, sem reutilizar IDs antigos.

## Primeira versão e evoluções

| Etapa proposta | RF | RNF e dependências | Justificativa |
| :-- | :-- | :-- | :-- |
| Conectar e validar planilha | RF22, RF23 | RNF07–RNF11, RNF13, RNF14; D01, D03 | Sem vínculo persistente, autorização e formato validado, não há registro confiável. |
| Cadastrar perfil e aprovar plano | RF01–RF05, RF07 | RNF01–RNF03, RNF05–RNF07, RNF14; D01 | Entrega planejamento com dados mínimos, duração verificável, explicações e aprovação antes da ativação. |
| Consultar, registrar e corrigir | RF09–RF12, RF14, RF16 | RNF02, RNF03, RNF05–RNF11, RNF13 | Completa o treino com histórico, limites de orientação, estados e ajustes explicitamente solicitados. |
| Recuperar e controlar dados | RF18, RF24, RF25 | RNF01, RNF07–RNF11, RNF13, RNF14; D01, D03 | Fecha o ciclo em nova conversa e permite consultar, exportar, desconectar e excluir. |
| Evoluir adaptação e análise (P1) | RF06, RF08, RF13, RF15, RF17, RF21 | Base P0; RF05/RF14 para adaptação, RF09/RF10 para progressão; RNF04, RNF15 | Automatiza redução, substituição, replanejamento e análise sobre dados já confiáveis. O tom respeitoso e a segurança continuam essenciais. |
| Acrescentar lembretes (P1) | RF19, RF20 | RF12, RF22; RNF11–RNF13; D02 | Exige canal e agendador comprovados, além do acesso conversacional. |

**P0 completo:** RF01–RF05, RF07, RF09–RF12, RF14, RF16, RF18, RF22–RF25; RNF01–RNF03, RNF05–RNF11, RNF13, RNF14. **P1:** RF06, RF08, RF13, RF15, RF17, RF19–RF21; RNF04, RNF12, RNF15. RNF04 contém metas propostas de desempenho; observar falhas e medir operações básicas já é P0 em RNF13.

A divisão mantém integridade, privacidade, tratamento de falhas e limites de orientação na primeira versão. P01 ficará **parcialmente atendida**: lembretes duas horas antes, progressão sugerida, análise de evolução e reorganização automática após faltas dependem de P1. P0 permite registrar faltas e remarcar explicitamente, consultar/corrigir histórico e ajustar prescrições por solicitação, mas não promete todas as automações esperadas pela persona.

## Decisões pendentes reais

| ID | Pendência | Recomendação proposta | Evidência necessária e critérios dependentes |
| :-- | :-- | :-- | :-- |
| D01 | Ferramentas próprias ou outra integração para ChatGPT/Sheets; identidade entre conversas; permissões; concorrência e persistência do vínculo | Preferir ferramentas controladas pelo Gym para autorização, validação do contrato, registro de operações e reconciliação, usando integração autorizada com Sheets. Outra integração é aceitável se demonstrar os mesmos controles; não escolher por suposição de capacidade. | Prova de vínculo em nova conversa, criação/leitura/escrita e verificação, acesso mínimo necessário, serialização, confirmação perdida, edição manual concorrente e recusa de identidade indevida. Definir onde guardar vínculo e último estado validado. Bloqueia aceite de RF18, RF22–RF25 e RNF01, RNF07–RNF09, RNF11, RNF14. Se não houver controle confiável, redesenhar antes de liberar escrita. |
| D02 | Canal móvel, agendador e meios de confirmar/reconciliar envio | Tratar lembretes como capacidade separada, com ID estável e política de perdas RNF12; não depender de uma conversa aberta para supor execução futura. | Testar permissão, aplicação em segundo plano, atraso, cancelamento e confirmação perdida. Bloqueia RF19, RF20 e aceite de RNF12, sem bloquear o restante de P0. |
| D03 | Política operacional de retenção, proteção e exclusão das cópias próprias e fronteira com provedores | Minimizar dados fora do Sheets; não armazenar conversas completas; definir retenção por classe e informar limitações das versões/cópias do Google. Manter meta proposta de até 30 dias para exclusão dos dados ativos controlados pelo Gym. | Definir responsáveis, prazos de vínculo/credenciais, logs, revisões e backups, procedimento de revogação e exclusão. Verificar contratos/capacidades dos provedores antes de alegar garantias. Bloqueia coleta real e aceite de RF25, RNF09, RNF10, RNF13. |
| D04 | Metas de latência e clientes efetivamente suportados | Usar matriz e volumes propostos em RNF01/RNF04; priorizar um cliente móvel comprovado e publicar limitações por operação. | Executar ensaios com versões reais e medir latência do Sheets incluída. Falta de cliente móvel validado impede declarar compatibilidade P0; números de desempenho permanecem proposta P1 até medição. |

**Não pendentes:** uso inicial do Google Sheets, separação entre prescrição e execução, IDs estáveis em vez de posição de linha, aceite antes de ativar plano e ausência de gravação silenciosa em conflito. O contrato detalha propostas de implementação desses requisitos; D01 deve provar sua viabilidade, não removê-los implicitamente.

## Rastreabilidade entre necessidades, RF e RNF

A tabela relaciona necessidades gerais às evidências atuais, sem converter exemplos da persona em restrições. Fontes: seções da [P01](../Personas/persona1.md) e decisões do contexto desta revisão. Detalhamento de campos, exportação e controle de dados também sustenta a verificabilidade e privacidade do produto, mesmo quando não aparece como pedido literal na persona.

| Necessidade / origem | RF | RNF |
| :-- | :-- | :-- |
| Planejar de acordo com objetivos, experiência e rotina — P01 §§2–5 | RF01–RF05 | RNF02, RNF05, RNF06, RNF14 |
| Usar o ChatGPT e consultar treino pelo celular — P01 §§6–7 | RF04, RF18, RF22 | RNF01–RNF03, RNF08 |
| Explicar exercícios e adaptar indisponibilidade ou pouco tempo — P01 §§4, 8–9 | RF05–RF08, RF14 | RNF02, RNF05, RNF06 |
| Registrar pesos, repetições e percepção; corrigir histórico — P01 §§5, 7, 9 | RF09–RF12, RF18 | RNF07–RNF11, RNF13, RNF14 |
| Faltas e mudanças pontuais ou futuras — P01 §§6–7 | RF12–RF14 | RNF05, RNF07, RNF11, RNF13 |
| Progressão realista e visualização da evolução — P01 §§4–5, 8–9 | RF15, RF17 | RNF05, RNF07, RNF14 |
| Cansaço, sono e dificuldades — P01 §§6, 9 | RF10, RF16 | RNF05, RNF06 |
| Lembrete no celular — P01 §5; duas horas é um exemplo configurável | RF19, RF20 | RNF01, RNF11–RNF13 |
| Incentivo conforme preferência — P01 §9 | RF21 | RNF02, RNF06 |
| Armazenar e recuperar entre conversas — P01 §§5, 9 e contexto Google Sheets | RF18, RF22, RF23 | RNF07–RNF11, RNF13, RNF14 |
| Consultar, exportar, desconectar e excluir — controle de dados solicitado nesta revisão | RF11, RF24, RF25 | RNF08–RNF11, RNF15 |
| Responder com desempenho mensurável e evoluir armazenamento — qualidade operacional proposta | RF04, RF09, RF11, RF24 | RNF04, RNF13, RNF15 |

## Verificação documental e futura validação

Antes de aceitar uma implementação, executar o fluxo P0 inteiro no mesmo vínculo e repeti-lo em outra conversa. Usar os cenários de falha do contrato e critérios de RF/RNF, incluindo acesso negado e reconciliação de gravação. Para IA, verificar presença das informações, referências, incertezas e limites, e não igualdade literal de frases. Não usar uma revisão de Markdown como evidência de integração funcional.

A revisão documental deve conferir IDs únicos, referências locais, prioridades consistentes, cobertura na rastreabilidade e ausência de afirmações de funcionalidades já implementadas. Não exige conectar contas, operar planilhas reais, publicar ou implementar o plugin.
