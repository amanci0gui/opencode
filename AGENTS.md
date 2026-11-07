# Diretrizes globais de desenvolvimento

Estas orientações se aplicam a diferentes repositórios e stacks. São condicionais: siga os requisitos do usuário, os contratos do software e as convenções existentes no repositório; não imponha uma arquitetura, ferramenta ou dependência apenas por preferência.

## Análise e orquestração

As regras desta seção se aplicam ao agente principal. Se estiver atuando como subagente, cumpra o escopo e o contrato do seu agente especializado; não assuma a orquestração da solicitação.

- Use o agente principal nativo `build` como orquestrador. Não crie outro agente principal.
- Antes de propor ou executar mudanças, identifique a stack pelos arquivos do projeto, leia as instruções aplicáveis e inspecione os arquivos e o comportamento relevantes. Em repositórios Git, confira o estado e o diff antes de editar para preservar alterações preexistentes.
- Identifique critérios de aceitação. Peça esclarecimento quando uma ambiguidade puder mudar materialmente o escopo, o comportamento ou uma decisão arquitetural.
- Mantenha o plano proporcional: uma correção localizada pode ser analisada e implementada diretamente pelo agente principal; funcionalidades novas, mudanças arquiteturais ou tarefas com dependências entre módulos devem receber um plano explícito do `architect`.
- Delegue a implementação ao `developer` somente quando houver um escopo claro e a delegação trouxer benefício. Não delegue a vários agentes alterações que possam tocar os mesmos arquivos. Evite delegações sem objetivo, contexto ou benefício claro.
- Peça ao `reviewer` uma revisão independente para alterações relevantes ou de maior risco. Forneça requisitos, critérios de aceitação, diff e resultados de validação disponíveis. O revisor não implementa correções; o agente principal avalia os achados e coordena as correções pertinentes.
- Consolide os resultados dos agentes com base nos requisitos, no código e em evidências. Evite ciclos repetidos de revisão; leve ao usuário questões que dependam de decisão de produto ou arquitetura.

## Instruções condicionais por stack

Detecte a stack antes de escolher ferramentas, convenções ou comandos. Use as recomendações abaixo somente quando a tecnologia estiver presente.

### C# e .NET

- Inspecione a solução (`.sln`/`.slnx`), os projetos (`.csproj`), `global.json`, `Directory.Build.*` e os testes existentes; identifique o SDK e os frameworks-alvo efetivamente usados.
- Respeite nomenclatura, organização e injeção de dependências do projeto. Separe responsabilidades sem adicionar camadas desnecessárias.
- Considere validação de entradas, erros, cancelamento e logging quando forem pertinentes. Preserve contratos públicos e compatibilidade.
- Execute as verificações existentes e relevantes, como build ou testes, sem presumir que todo repositório usa a mesma estrutura.
- Não imponha Clean Architecture, CQRS, MediatR, Entity Framework ou bibliotecas novas sem necessidade demonstrável.

### Java

- Inspecione `pom.xml`, arquivos e wrappers do Gradle, `settings.gradle(.kts)`, toolchains e testes; identifique a versão do Java e o sistema de build realmente usados.
- Respeite pacotes, convenções e frameworks já existentes. Use tipagem adequada e tratamento explícito de erros, sem abstrações ou camadas sem propósito concreto.
- Preserve contratos e compatibilidade com consumidores existentes. Execute os testes pertinentes disponíveis no projeto.
- Não imponha Spring Boot, arquitetura específica ou dependências de terceiros quando o projeto não precisar delas.

### React e TypeScript

- Inspecione `package.json`, `tsconfig.json`, arquivos de lock e a estrutura do projeto. Identifique o gerenciador de pacotes e os scripts configurados; não os presuma.
- Siga os padrões existentes de componentes, hooks, estado e roteamento. Prefira tipos explícitos e evite `any` desnecessário, componentes excessivamente grandes e abstrações prematuras.
- Para interfaces assíncronas, considere estados de carregamento, erro, vazio e sucesso. Considere acessibilidade e responsividade quando pertinentes.
- Use os testes, lint e verificações de tipos já configurados. Evite dependências novas quando os recursos existentes forem suficientes.
- Não imponha Redux, TanStack Query, bibliotecas de componentes ou uma estrutura de pastas sem necessidade concreta.

## Implementação, validação e comunicação

- Implemente incrementalmente e dentro do escopo aprovado. Não sobrescreva mudanças preexistentes nem altere arquivos sem relação com a tarefa.
- Escolha comandos a partir dos scripts, wrappers e documentação do repositório. Execute as verificações relevantes disponíveis; não afirme que algo foi executado ou aprovado sem evidência.
- Examine o diff ao concluir. Informe comandos que não puderam ser executados e o motivo.
- Na resposta final, resuma o que foi alterado, os arquivos principais, decisões técnicas, testes e resultados, limitações conhecidas e pendências que dependam do usuário. Não declare a implementação concluída se uma validação essencial falhou.

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

When the user types `/graphify`, use the installed graphify skill or instructions before doing anything else.

Rules:
- When graphify-out/graph.json exists, run `graphify query "<question>"` before your first grep, search or source-file read for a codebase question, and put this rule in every subagent prompt that explores code. Read raw files only for lines the graph or an earlier read already located. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- Dirty graphify-out/ files are expected after hooks or incremental updates; dirty graph files are not a reason to skip graphify. Only skip graphify if the task is about stale or incorrect graph output, or the user explicitly says not to use it.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).
- A committed graphify-out/graph.json is rewritten whole on every build, so a large diff to it in a pull request is expected. Do not flag it as churn or suggest skipping the graph rebuild or its hooks.

<!-- context7 -->
Use Context7 MCP to fetch current documentation whenever the user asks about a library, framework, SDK, API, CLI tool, or cloud service — even well-known ones like React, Next.js, Prisma, Express, Tailwind, Django, or Spring Boot. This includes API syntax, configuration, version migration, porting code between libraries, platforms, or SQL dialects, library-specific debugging, setup instructions, and CLI tool usage. Use even when you think you know the answer — your training data may not reflect recent changes. Prefer this over web search for library docs.

Do not use for: refactoring, debugging, or reviewing code that does not depend on a library or platform API, or general programming concepts.

## Steps

1. Always start with `resolve-library-id` using the library name and what to look up in the library's documentation, unless the user provides an exact library ID in `/org/project` format
2. Pick the best match (ID format: `/org/project`) by: exact name match, description relevance, code snippet count, source reputation (High/Medium preferred), and benchmark score (higher is better). If results don't look right, try alternate names or queries (e.g., "next.js" not "nextjs", or rephrase the question). Use version-specific IDs when the user mentions a version
3. `query-docs` with the selected library ID and what to look up in the library's documentation (not single words), scoped to a single concept. If the question spans multiple distinct concepts (e.g. routing and auth and caching), make a separate `query-docs` call per concept with the same library ID, unless the question is about how the concepts interact — combined queries dilute ranking and return shallow results for each topic
4. Answer using the fetched docs
<!-- context7 -->
