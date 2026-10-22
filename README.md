# Gym — Plugin para ChatGPT

Projeto de um plugin para o ChatGPT, com invocação pretendida como **@Gym**, para ajudar a organizar treinos de musculação personalizados e acompanhar a evolução do usuário. Esta documentação descreve o comportamento esperado do produto, sem indicar que as funcionalidades já estejam implementadas.

## Objetivos do plugin

- Auxiliar na criação de treinos personalizados, considerando os objetivos, a rotina e o tempo disponível do usuário.
- Acompanhar a evolução com base nos exercícios realizados, nas cargas, nas repetições e nos relatos de cada treino.
- Ajustar os treinos conforme o histórico, as faltas e as mudanças de disponibilidade informadas pelo chat.
- Registrar inicialmente os dados dos treinos no Google Sheets, com a possibilidade de utilizar um banco de dados futuramente.

## Contexto de uso

O Gym é voltado a pessoas que buscam apoio para organizar seus treinos de musculação, acompanhar resultados e adaptar o planejamento à própria rotina.

A proposta é permitir que o usuário consulte o treino do dia pelo ChatGPT, registre o que realizou e solicite ajustes para uma sessão específica ou para os próximos treinos, considerando seu histórico e sua disponibilidade.

As [personas do projeto](docs/Personas/) documentam os perfis, as necessidades e os cenários de uso que orientam seu desenvolvimento.

## Documentação e escopo proposto

- [Requisitos funcionais](docs/Requisitos/funcionais.md): comportamento, critérios de aceite e regras das sessões.
- [Requisitos não funcionais](docs/Requisitos/nao_funcionais.md): integridade, privacidade, acessibilidade e critérios de qualidade.
- [Contrato de dados](docs/Requisitos/contrato_dados.md): estrutura proposta no Google Sheets, versões e tratamento de conflitos.
- [Decisões e rastreabilidade](docs/Requisitos/decisoes_rastreabilidade.md): primeira versão, evoluções e pendências de integração.

A primeira versão proposta prioriza conectar a planilha, cadastrar o perfil, criar e recuperar um plano aprovado, consultar sessões, registrar e corrigir séries e recuperar os dados em outra conversa. Lembretes, análises de evolução e automações adicionais ficam como evoluções. A escolha das ferramentas de integração ainda precisa ser validada; o acesso ao Sheets e o envio de notificações não são capacidades presumidas apenas pela invocação @Gym.

## Objetivos pessoais

Estou criando este plugin para:

1. Ajudar a estruturar meus próprios treinos.
2. Aprender a criar plugins para o ChatGPT e desenvolver minhas habilidades com inteligência artificial e automações.
3. Aplicar conhecimentos de Engenharia de Software.
