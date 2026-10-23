# Gym — Plugin para ChatGPT

O **@Gym** ajuda a organizar treinos de musculação, registrar resultados e ajustar as próximas sessões com base no histórico e na rotina do usuário. O armazenamento inicial será o **Google Sheets**.

## Proposta

O fluxo principal é planejar a semana → consultar o treino → registrar a execução → acompanhar a evolução → ajustar os próximos treinos.

A primeira versão deve permitir adaptar sessões ao tempo disponível, substituir exercícios, reorganizar treinos após faltas e sugerir progressão. Lembretes no celular e personalização do tom ficam para uma etapa posterior.

Objetivo, experiência, equipamentos e disponibilidade são informações de treino coletadas pelo chat; não exigem uma tela de cadastro. As [personas](docs/Personas/) orientam o produto sem fixar uma rotina para todos.

A documentação descreve o comportamento esperado. A integração com ChatGPT e Sheets ainda precisa ser validada.

## Documentação

- [Requisitos funcionais: o que o Gym deve fazer](docs/Requisitos/funcionais.md)
- [Requisitos não funcionais: qualidade e limites](docs/Requisitos/nao_funcionais.md)
- [Decisões técnicas e rastreabilidade](docs/Requisitos/decisoes_rastreabilidade.md)
- [Dados necessários no Sheets](docs/Requisitos/contrato_dados.md)

## Objetivos pessoais

1. Estruturar meus próprios treinos.
2. Aprender a criar plugins para o ChatGPT e desenvolver habilidades com IA e automações.
3. Aplicar conhecimentos de Engenharia de Software.
