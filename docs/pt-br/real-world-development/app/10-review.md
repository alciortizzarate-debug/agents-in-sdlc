---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo do aplicativo, os dois marcos de PR, os exercícios de canvas e as práticas reutilizáveis de qualidade; depois, explore outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o aplicativo GitHub Copilot em um fluxo contínuo da Tailspin Toys. Você:

- conectou um repositório, explorou o espaço de trabalho do aplicativo e o backlog predefinido e experimentou um chat rápido.
- iniciou uma sessão específica de avaliação por estrelas, revisou o resultado em um canvas de navegador e fez manualmente o merge do primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo **Plan**, desenvolveu-a no modo **Autopilot** e a revisou no modo **Interactive**.
- orientou o agente com instruções personalizadas e depois personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o **Agent Merge** no segundo PR.
- usou o canvas Database Explorer existente e, depois, criou e testou um canvas de triagem vinculado ao repositório.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cartões.
2. **Fluxo de filtragem e qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Do planejamento da filtragem à abertura do PR, você usou a mesma sessão, worktree e branch. Reunimos esse trabalho em um único PR para simplificar o workshop. Depois, você usou o Database Explorer existente e criou um canvas de triagem vinculado ao repositório sem repetir o fluxo de PR.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados e revisou os requisitos e a cobertura de testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que nenhum teste novo é necessário também pode estar correta. Ferramentas ausentes, verificações ignoradas e falhas são bloqueios visíveis, não aprovações. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA, configurou um servidor MCP e criou um canvas. Reutilize essas personalizações entre sessões e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas, os servidores MCP conectam ferramentas externas e os canvases oferecem superfícies interativas compartilhadas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Associe o **modo e o modelo** à tarefa. Use **Plan** para avaliar uma abordagem antes de desenvolver, **Interactive** para acompanhar alterações focadas e **Autopilot** somente para tarefas isoladas e com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais avançado, com maior esforço de raciocínio, para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* influencia significativamente o resultado. Chats rápidos são úteis para definir o escopo de uma ideia antes de transformá-la em uma sessão completa.

## Mais para explorar

Você concluiu o fluxo principal. Há outros recursos que vale a pena conhecer:

- [**Automações**][using-automations] para tarefas recorrentes ou sob demanda, como resumir o trabalho recente. Revise a agenda, as permissões e o escopo antes de adotar uma automação; criá-la é um próximo passo, não parte deste workshop.
- **Rubber duck** para discutir um problema e receber feedback útil antes de começar a desenvolver.
- [`/chronicle`][chronicle] para gerar uma narrativa do que aconteceu em uma sessão.
- [Bring your own key (BYOK)][byok] para usar modelos do seu próprio provedor, incluindo modelos locais por meio do Ollama, Foundry Local ou LM Studio.
- [Deep links][deep-links] para abrir o aplicativo diretamente em um repositório, uma sessão ou um prompt.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você planeja criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com as experiências dela. E, como sempre, explore a documentação.

Para conhecer melhor o ecossistema do GitHub Copilot, confira o [percurso do VS Code][vscode-harness], o [percurso do Copilot CLI][cli-harness] ou o [percurso do agente de nuvem][cloud-harness].

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Personalizar o aplicativo GitHub Copilot][customize]
- [Usar automações][using-automations]
- [Trabalhar com extensões de canvas][canvas-docs]

[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
[cloud-harness]: ../../cloud/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[customize]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
[using-automations]: https://docs.github.com/copilot/how-tos/github-copilot-app/using-automations
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[chronicle]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/chronicle
[byok]: https://docs.github.com/copilot/how-tos/github-copilot-app/use-byok-models
[deep-links]: https://docs.github.com/copilot/how-tos/github-copilot-app/open-with-deep-links
