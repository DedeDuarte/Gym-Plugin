# Decisões técnicas e rastreabilidade

## Prioridades do produto

A primeira versão deve completar o ciclo **planejar → consultar → registrar → acompanhar → adaptar**. Redução do treino por tempo, substituições, replanejamento após faltas, progressão e evolução são P0 porque atendem às dificuldades centrais da [persona](../Personas/persona1.md).

Lembretes, preferência de tom, exportação completa e troca de armazenamento são P1. A consulta ao histórico, o controle de acesso e a possibilidade de solicitar exclusão permanecem essenciais.

Os IDs dos RF e RNF foram preservados. “Perfil” significa contexto de treino coletado pelo chat; não exige tela de cadastro nem login próprio do Gym.

## Integração e futuro servidor

O Sheets é o armazenamento inicial. A escolha entre uma integração existente e ferramentas próprias permanece aberta. O servidor, se necessário, deve sustentar os mesmos RF, com autorização, validação de dados e tratamento de falhas nas ferramentas, sem depender do modelo para decidir acesso.

| ID | Decisão pendente | Validação necessária |
| :-- | :-- | :-- |
| D01 | Integração com Sheets, identidade e recuperação entre conversas. | Demonstrar leitura, gravação e recuperação da planilha autorizada. O vínculo deve ser persistente e protegido, com credenciais fora das células. Validar acesso revogado, outra conta e arquivo incompatível antes de escolher o mecanismo de autenticação. |
| D02 | Canal e agendador dos lembretes. | Testar permissão, segundo plano, remarcação, cancelamento, repetição e atraso. Meta proposta: em 30 dias, 95% dos avisos aceitos pelo canal em até 5 minutos do horário; incluir perdas e excluir cancelamentos anteriores ao vencimento. Aceite não comprova entrega. |
| D03 | Retenção, exclusão e responsabilidades. | Definir prazos para dados ativos, credenciais, logs, revisões e cópias antes de coletar dados reais. Meta proposta de exclusão dos dados ativos sob controle do Gym: até 30 dias; definir também expiração das cópias e informar limites externos. |
| D04 | Clientes suportados e desempenho. | Validar ao menos um cliente móvel. Medir leitura, lote de 20 séries e correção, incluindo rede, tentativas e Sheets; separar geração do chat. Metas propostas: p95 de 5 s para leitura e 8 s para escrita, sujeitas ao ensaio abaixo. |

**Ensaio de D04:** 100 amostras por operação, 10 usuários simultâneos e, por usuário, 250 sessões, 5.000 séries, 50 exercícios e 24 versões em 12 meses. Registrar latências, erros e timeouts. Clientes candidatos: navegador, iOS e Android; validar autorização, texto ampliado, leitor de tela, teclado/foco quando aplicável e operações P0. Nenhum suporte ou meta está comprovado ainda.

## Controles da implementação

- Autorizar cada operação na ferramenta com a identidade validada e a planilha vinculada; não confiar em identidade digitada no chat. Revogação e desconexão devem impedir novas operações.
- Usar HTTPS, permissões mínimas, proteção de segredos e criptografia do armazenamento próprio. Não guardar credenciais em células ou código versionado.
- Usar IDs estáveis para séries, sessões e planos. Reconhecer retentativas da mesma gravação e verificar o resultado quando a confirmação se perder.
- Testar falha parcial e alterações concorrentes antes de liberar escrita. Bloquear operações dependentes de gravação incompleta e não resolver conflitos por sobrescrita silenciosa. O mecanismo será escolhido após o teste, sem presumir transações do Sheets.
- Validar formato e edições manuais. Alterações válidas são apresentadas ao usuário para incorporação; conflitos exigem escolha. Um dado antigo em cache não confirma o estado atual da planilha.
- Registrar ID de operação, identidade pseudonimizada, tipo, horário, duração, tentativas, provedor e resultado; restringir acesso e retenção conforme D03, sem conteúdo pessoal ou links de exportação.
- Exportações completas devem usar JSON com IDs, versões, revisões, unidades, fuso e identificação do formato; verificar contagens e sinalizar incompletude.
- Migrações exigem autorização, cópia recuperável e validação de IDs, referências, unidades e contagens.

## Responsabilidades

O Gym protege credenciais e dados sob seu controle, limita acesso e cumpre a exclusão nas cópias que gerencia. Google e integração têm políticas próprias de disponibilidade, versões e retenção que precisam ser verificadas. O usuário controla compartilhamentos e cópias externas; compartilhar a planilha pode expor os dados aos destinatários.

Desconectar não apaga a planilha. Exclusão de registros inclui suas revisões e cópias geridas pelo Gym, conforme o alcance confirmado. Apagar o arquivo inteiro exige confirmação distinta. Acesso revogado pode impedir a exclusão pelo Gym; nesse caso, orientar o proprietário. Mover para a lixeira não comprova exclusão definitiva.

## Necessidades e requisitos

| Necessidade | RF | RNF principais |
| :-- | :-- | :-- |
| Planejar conforme objetivo, rotina e tempo | RF01–RF06 | RNF02, RNF05, RNF06 |
| Entender e substituir exercícios | RF07, RF08 | RNF02, RNF05, RNF06 |
| Registrar, corrigir e recuperar histórico | RF09–RF12, RF18, RF22, RF23 | RNF07–RNF11, RNF13, RNF14 |
| Reorganizar, progredir e acompanhar evolução | RF13–RF17 | RNF05–RNF07 |
| Receber avisos e incentivo | RF19–RF21 | RNF06, RNF11, RNF12 |
| Consultar, exportar e controlar dados | RF24, RF25 | RNF08–RNF10, RNF15 |
| Usar pelo celular | RF04, RF09, RF18 | RNF01–RNF04 |
