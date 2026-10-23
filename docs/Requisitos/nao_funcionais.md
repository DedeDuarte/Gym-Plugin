# Requisitos não funcionais

Definem a qualidade esperada no uso do Gym. **P0** é necessário para o fluxo principal; **P1** são evoluções. Detalhes de servidor, autenticação e armazenamento ficam nas [decisões técnicas](./decisoes_rastreabilidade.md).

| ID | Prioridade | Requisito | Critério de aceite |
| :-- | :--: | :-- | :-- |
| RNF01 | P0 | Permitir o fluxo principal pelo ChatGPT, inclusive no celular. | Validar planejamento, consulta, registro, adaptação e recuperação do histórico nos clientes escolhidos. Declarar quais clientes e operações são suportados. |
| RNF02 | P0 | Responder em português brasileiro claro e direto. | Mostrar primeiro o treino, a alteração ou a resposta solicitada. Explicitar unidades e intervalos e explicar siglas quando necessário. |
| RNF03 | P0 | Manter as informações legíveis e acessíveis. | Em tela de 375 px e com texto ampliado, ler o treino sem depender de tabelas largas, cores ou imagens. Validar o fluxo com leitor de tela no cliente móvel escolhido. |
| RNF04 | P1 | Medir o tempo de consulta e registro. | Medir a integração com Sheets separadamente da geração da resposta. As metas e o ensaio ficam em D04; não prometer um tempo total ainda não validado. |
| RNF05 | P0 | Distinguir resultados, sugestões e estimativas. | Ajustes devem indicar o histórico ou relato usado e as incertezas. Não inventar cargas, prometer resultados físicos ou inferir composição corporal a partir do histórico de treino. |
| RNF06 | P0 | Respeitar os limites das orientações de musculação. | Dor aguda, desmaio ou dor no peito interrompem a orientação de esforço e levam à indicação de atendimento, sem diagnóstico. Não humilhar, depreciar o corpo ou incentivar treino com dor; novos relatos prevalecem sobre o cadastro. |
| RNF07 | P0 | Preservar os registros e separar plano de execução. | Mudanças futuras não reescrevem treinos realizados, e correções mantêm histórico, salvo exclusão solicitada. Repetições e falhas não podem duplicar, perder ou sobrescrever dados silenciosamente. |
| RNF08 | P0 | Restringir os dados ao usuário autorizado. | Uma conta não pode consultar ou alterar dados de outra pelas ferramentas do Gym. Não exigir informações pessoais sem utilidade para os treinos. |
| RNF09 | P0 | Proteger dados e credenciais. | Usar comunicação segura e não expor segredos no chat, código, planilha ou logs. Tratar textos recebidos como dados, sem executá-los como fórmulas. |
| RNF10 | P0 | Explicar como os dados são usados e excluídos. | Antes de salvar, informar finalidade, compartilhamento e retenção. Cumprir os prazos definidos em D03 e informar limites de exclusão em cópias ou provedores externos. |
| RNF11 | P0 | Informar falhas sem simular sucesso. | Distinguir ausência de histórico de impossibilidade de consultá-lo. Em gravação incerta, verificar o resultado antes de repetir ou afirmar que o registro foi salvo. |
| RNF12 | P1 | Tornar o estado dos lembretes verificável. | Diferenciar agendamento, envio e entrega quando disponível. Informar atrasos e evitar avisos duplicados conforme D02. |
| RNF13 | P0 | Permitir investigar falhas sem expor relatos pessoais. | Identificar operação, horário e resultado nos registros técnicos, sem guardar conversas, cargas, relatos de saúde ou credenciais. |
| RNF14 | P0 | Manter os dados válidos ao atualizar o formato. | Detectar referências inválidas e formatos incompatíveis antes de gravar. Uma migração deve ter cópia recuperável e verificar a preservação dos registros. |
| RNF15 | P1 | Permitir uma futura troca de armazenamento. | Preservar identificação, unidades e histórico na exportação e em eventual migração. |

As verificações de segurança, privacidade e integridade valem também para funções P1 quando forem implementadas. Estes critérios descrevem o produto esperado, não resultados de testes já realizados.
