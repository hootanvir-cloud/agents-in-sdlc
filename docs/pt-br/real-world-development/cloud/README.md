---
slug: pt-br/real-world-development/cloud
title: "Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui tarefas no GitHub e o agente de nuvem as executa em segundo plano — explorando o repositório, fazendo alterações e abrindo um pull request — enquanto você pode se dedicar a outras atividades.

Nestes exercícios, você adicionará instruções personalizadas para orientar o agente de nuvem e, em seguida, atribuirá a ele uma issue do GitHub para implementação. Você revisará e usará agentes personalizados para orientar a abordagem, acompanhará e conduzirá as sessões no painel de agentes e concluirá revisando os pull requests e iterando sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para orientar o agente de nuvem |
| [2. Agente de nuvem do GitHub Copilot][ex2] | Agente assíncrono | Atribua issues ao agente de nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Acompanhe e oriente sessões de agente |
| [5. Iterar][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente de nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][student-plan]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> O agente de nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — todos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este percurso não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [percurso da CLI][cli-harness] ou o [percurso do VS Code][vscode-harness].

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[student-plan]: https://github.com/education/students
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
