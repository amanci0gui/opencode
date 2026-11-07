# Skills de terceiros para OpenCode

## Origem e licença

- Repositório: [farmage/opencode-skills](https://github.com/farmage/opencode-skills)
- Snapshot importado: `2ec14733c1aaa6f98c4b12ecd078ff7c0454710b` (2026-09-27)
- Importação: 2026-10-25
- Licença: MIT; texto preservado em `farmage-opencode-skills-LICENSE`.
- O repositório de origem identifica o conteúdo original como baseado em [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills), também MIT, e a adaptação para OpenCode como trabalho de `farmage`.

Foram copiadas as pastas completas, incluindo referências. As sete skills personalizadas que já existiam nesta configuração foram preservadas. Nenhum comando, instalador, servidor MCP ou configuração de CLI do repositório de origem foi instalado.

## Skills importadas

**Arquitetura, APIs e documentação:** `api-designer`, `architecture-designer`, `code-documenter`, `code-reviewer`, `legacy-modernizer`, `microservices-architect`, `spec-miner`, `graphql-architect`, `websocket-engineer`.

**Linguagens e ferramentas:** `cpp-pro`, `golang-pro`, `javascript-pro`, `kotlin-specialist`, `php-pro`, `python-pro`, `rust-engineer`, `sql-pro`, `swift-expert`, `typescript-pro`, `cli-developer`.

**Frameworks e plataformas:** `angular-architect`, `django-expert`, `django-storages-s3`, `fastapi-expert`, `flutter-expert`, `laravel-specialist`, `nestjs-expert`, `nextjs-developer`, `rails-expert`, `react-native-expert`, `spring-boot-engineer`, `vue-expert`, `vue-expert-js`, `wordpress-pro`, `salesforce-developer`, `shopify-expert`.

**Infraestrutura, dados e IA:** `cloud-architect`, `database-optimizer`, `devops-engineer`, `kubernetes-specialist`, `monitoring-expert`, `sre-engineer`, `terraform-engineer`, `embedded-systems`, `game-developer`, `fine-tuning-expert`, `ml-pipeline`, `pandas-pro`, `prompt-engineer`, `rag-architect`, `spark-engineer`, `mcp-developer`, `playwright-expert`.

## Skills não importadas

- `csharp-developer`, `dotnet-core-expert`, `java-architect` e `react-expert`: sobreposição com as skills locais mais específicas para .NET, Java e React/TypeScript.
- `debugging-wizard` e `test-master`: sobreposição com `root-cause-debugging` e `test-strategy` locais. `test-master` também inclui exemplos de testes de exploração que não são necessários para esta configuração.
- `postgres-pro`: sobreposição com `database-optimizer` e `sql-pro`.
- `security-reviewer`, `secure-code-guardian` e `the-fool`: incluem instruções de pentest, payloads de exploração ou red teaming que não são necessários para o uso defensivo configurado.
- `chaos-engineer`: inclui exemplos de injeção de falhas com comandos direcionados a ambientes de produção; preferi não habilitar esse fluxo por padrão.
- `feature-forge`: depende de `AskUserQuestions` e de delegação via `Task`, ferramentas que não correspondem ao fluxo configurado aqui.
- `atlassian-mcp`: seus exemplos de configuração usam `mcpServers`, incompatível com o formato V2 documentado para OpenCode; não há integração Atlassian configurada neste ambiente.
- `fullstack-guardian`: descrição e fluxo muito abrangentes, com etapas obrigatórias que se sobrepõem às instruções locais de implementação.

## Compatibilidade com esta configuração

As skills estão no diretório global reconhecido pelo OpenCode V2 (`~/.config/opencode/skills/<id>/SKILL.md`); não foi necessário alterar `opencode.json`. O conteúdo de cada skill é orientativo: as instruções globais e as convenções do repositório em trabalho continuam prevalecendo.

Removi o campo `allowed-tools` de `code-reviewer` e `spec-miner`, pois ele não é um campo de permissão reconhecido pela documentação V2 de skills. Atualizei os metadados de relacionamento para não apontar a skills ausentes, substituindo referências por equivalentes locais quando apropriado, e atualizei o texto de `code-reviewer`/`django-storages-s3` para usar essas alternativas.

Também acrescentei salvaguardas de aprovação explícita para mudanças operacionais em `cloud-architect`, `devops-engineer`, `kubernetes-specialist`, `sre-engineer`, `salesforce-developer` e `shopify-expert`. Os exemplos de injeção de falhas ficaram limitados a staging/teste e inativos por padrão; removi o exemplo de apagar todas as NetworkPolicies e evitei imprimir valores de Secrets.
