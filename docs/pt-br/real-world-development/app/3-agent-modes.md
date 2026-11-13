---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi desenvolvido para trabalhar com o fluxo existente de uma organização, garantindo que as soluções certas sejam criadas da maneira certa. Esta é a primeira de várias lições nas quais você seguirá um processo típico de desenvolvimento com agentes: começará por uma issue para criar uma funcionalidade, confirmará que o código é válido e que a funcionalidade se comporta como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo da funcionalidade. Em geral, você teria sessões ou PRs diferentes para os vários tipos de arquivo, mas simplificaremos o processo para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar a funcionalidade no modo **Autopilot**.
- revisar o código.
- validar manualmente a funcionalidade em um canvas de navegador.

Ao continuar o trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para decidir esses detalhes e, depois, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Incorporar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo parecido com este:

1. Abra uma issue registrada com os detalhes do trabalho necessário.
2. Crie um plano do que precisa ser desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, faça a mesclagem.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação desse fluxo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e espera sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você o revisa e aprova antes de o agente executá-lo.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem esperar sua confirmação.

Comece no modo Plan, revise o plano e depois use Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme que o PR das avaliações por estrelas foi mesclado e que sua `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada pelo template; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Exibição da issue no aplicativo GitHub Copilot, com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se funcionarem no terminal, mas não na sessão, verifique o PATH do aplicativo ou a configuração do gerenciador de versões e feche completamente e reabra o aplicativo. Tente novamente a operação e as verificações com falha; não reinstale o npm apenas porque o agente não o encontrou.

## Planejar a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você iniciou pela issue, o Copilot já tem a solicitação como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Discuta comportamentos pouco claros, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes. Mantenha o escopo na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e guarde a URL da issue e os esclarecimentos acordados para validar o trabalho mais adiante.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confira o modo ativo, em vez de presumir que a aprovação o alterou. Confirme que o indicador mostra **Autopilot**.

O Copilot começará a implementação e iterará conforme o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Encerre somente os servidores que você iniciou.

## Revisar e verificar a implementação

Como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar a implementação e executar o site.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de várias categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Corrija as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Envie este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Manter esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, a worktree e a branch para as Lições 4 a 8. Na próxima lição, você aplicará instruções personalizadas ao código de filtragem, depois adicionará melhorias para a skill e o QA antes de criar e mesclar o PR combinado na Lição 8. Depois de encerrar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou a funcionalidade no modo **Autopilot**.
- revisou o código.
- validou manualmente a funcionalidade em um canvas de navegador.

Em seguida, você explorará como gerar código que segue práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
