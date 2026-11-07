---
name: "React and TypeScript Development"
description: "Apoia mudanças de componentes React com TypeScript em repositórios detectados, respeitando framework, hooks, tipagem, estados assíncronos e testes locais; não impõe bibliotecas."
---

# Desenvolvimento com React e TypeScript

Use esta skill quando o repositório e a tarefa realmente envolverem React com TypeScript. Siga o framework, o gerenciador, o roteamento, as convenções de estado e os testes existentes.

## Quando usar

- Criar ou modificar componentes, hooks, estado, integração com API ou formulários React/TypeScript.
- Investigar regressões de UI relacionadas a tipos, efeitos, estado ou renderização.
- Avaliar testes, acessibilidade ou comportamento responsivo da interface alterada.

Não use em projetos sem React/TypeScript ou para impor Redux, TanStack Query, biblioteca de componentes, formulário ou acesso a dados.

## Pré-requisitos e inspeção

Inspecione `package.json`, lockfiles, `tsconfig.json`, framework e roteamento, convenções de componentes e estado, clientes de API, testes e scripts disponíveis. Identifique o gerenciador de pacotes pelos arquivos do projeto; não o presuma.

## Procedimento recomendado

1. Modele props, eventos e dados com os tipos existentes. Evite `any` desnecessário, conversões que ocultem incompatibilidades e duplicação de modelos sem justificativa.
2. Siga as regras de hooks e os padrões de estado do projeto. Use efeitos para sincronização ou efeitos colaterais necessários; não mantenha estado derivado quando puder ser calculado com segurança.
3. Para interfaces assíncronas, considere carregamento, erro, vazio e sucesso de acordo com o fluxo real. Evite respostas antigas ou efeitos repetidos quando isso puder alterar o resultado observado.
4. Para formulários, siga o padrão existente de validação e apresente erros de forma compreensível e acessível. Preserve os contratos com a API e a prevenção de submissões incorretas quando aplicável.
5. Considere semântica HTML, nomes acessíveis, teclado, foco e mensagens de erro quando relevantes. Preserve os padrões responsivos existentes sem inventar breakpoints ou sistema visual.
6. Teste comportamentos observáveis e interações relevantes usando as ferramentas já instaladas. Inclua estados assíncronos ou de validação somente se fizerem parte da mudança.
7. Execute as verificações pertinentes configuradas pelo projeto e relate resultados e limitações reais.

## Critérios de conclusão

O componente respeita tipos e padrões existentes; estados relevantes não ficam indefinidos; a interface continua utilizável nos contextos afetados; testes e verificações pertinentes foram executados ou suas limitações foram explicitadas.

## Limitações

Frameworks React, roteadores e bibliotecas de dados diferem. Inspecione o repositório antes de usar uma API ou padrão. Esta skill não seleciona bibliotecas nem define sozinha requisitos visuais ou de acessibilidade ausentes.
