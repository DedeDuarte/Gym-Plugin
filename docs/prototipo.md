# Protótipo do Gym

## Objetivo

Organizar os treinos pelo chat, com o Gym criando e cuidando da planilha no Google Sheets após autorização. O usuário não precisa fornecer um modelo nem preencher células: informa sua rotina, consulta o treino e conta o que realizou.

O Gym coleta objetivo, experiência, prática atual ou pausa, equipamentos, disponibilidade e limitações declaradas. Com essas informações, propõe a rotina e, após aceite, salva o plano. A planilha segue o [contrato de dados](Requisitos/contrato_dados.md) e é atualizada conforme os treinos e as mudanças combinadas pelo chat.

## O que testar

| Conversa | Resultado esperado |
| :-- | :-- |
| “Configure meus treinos.” | Coletar o contexto que faltar, obter autorização, criar a planilha e propor a rotina. Salvar o plano após aceite. |
| “Qual é o treino de hoje?” | Consultar o plano e o histórico e mostrar exercícios, séries, repetições, carga sugerida, descanso e duração estimada. |
| “Hoje tenho 25 minutos.” | Propor uma sessão reduzida e, após aceite, ajustar só hoje, mantendo a duração habitual dos próximos treinos. |
| “Fiz três séries de 10 com 20 kg por halter. A última foi difícil.” | Salvar cada série separadamente e associar o feedback ao registro, sem marcar exercícios não informados como realizados. |
| “Anotei a carga errada na segunda série.” | Identificar o registro, perguntar o valor correto e confirmar a correção antes de salvar. |
| “Faltei ontem.” | Registrar a falta e propor uma nova sequência conforme a disponibilidade, sem apagar a falta ou acumular sessões automaticamente. |
| “Dormi mal e estou muito cansado.” | Esclarecer o relato e orientar manter, reduzir ou interromper o treino, seguindo os limites de segurança dos RNF. |
| “Como evoluí neste mês?” | Mostrar frequência e resultados por exercício, com datas e unidades, indicando lacunas e condições diferentes. |
| Em outra conversa: “Qual carga tento no próximo treino?” | Recuperar os registros da planilha e justificar a sugestão pelo histórico e esforço relatado. Sem referência suficiente, explicar o que falta. |

## Quando considerar que funcionou

O Gym deve criar a planilha, manter plano e execução separados, registrar e corrigir resultados e recuperar os mesmos dados em outra conversa autorizada. Ajustes aceitos devem aparecer na planilha sem duplicar registros ou reescrever treinos já realizados.

Criar e manter a planilha automaticamente é parte do teste. Se a integração não permitir isso, registrar o impedimento; uma planilha preparada manualmente não atende a esse objetivo.

Pedidos ambíguos devem gerar esclarecimento. Em falha de gravação, não afirmar que salvou; se o resultado for incerto, verificar antes de repetir.

## Limites desta etapa

Uso pessoal, sem lembretes ou publicação. As orientações seguem os [RF](Requisitos/funcionais.md) e os limites de segurança, privacidade e integridade dos [RNF](Requisitos/nao_funcionais.md).

Este documento é o roteiro do teste, ainda não executado. Não implementa a integração, cria a planilha ou configura uma skill.
