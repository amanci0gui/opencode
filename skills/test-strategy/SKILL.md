---
name: "Test Strategy"
description: "Planeja e avalia testes para mudanças de comportamento em qualquer stack, incluindo sucesso, limites, falhas e regressões; não use apenas para executar um comando já definido."
---

# Estratégia de testes orientada ao comportamento

Use esta skill para decidir **o que precisa ser verificado** quando uma mudança afeta comportamento. Ela não substitui as convenções do repositório nem as responsabilidades do agente.

## Quando usar

- Uma funcionalidade ou correção altera comportamento observável.
- É necessário acrescentar um teste de regressão ou avaliar uma lacuna de cobertura.
- Uma alteração de risco requer selecionar verificações proporcionais ao impacto.
- Uma revisão precisa avaliar se os testes cobrem os critérios de aceitação relevantes.

Não a carregue só para executar uma verificação já escolhida ou para tarefas sem comportamento testável afetado.

## Procedimento

1. **Estabeleça o comportamento esperado.** Use critérios de aceitação, contrato existente e exemplos fornecidos. Se forem ambíguos, explicite a ambiguidade em vez de inventar uma regra.
2. **Inspecione os testes existentes.** Identifique os projetos ou módulos de teste, convenções, fixtures, ferramentas e limites já usados. Não presuma framework, gerenciador ou camada de testes.
3. **Derive os cenários relevantes.** Considere o fluxo normal e, quando aplicável, limites, entradas inválidas, falhas, permissões, estados assíncronos e regressões. Priorize cenários que distinguem resultados corretos de incorretos.
4. **Escolha a menor camada útil.** Prefira testar a regra na camada em que o comportamento é definido. Use teste de integração ou de componente quando a interação entre partes fizer parte do contrato; não suba de camada sem benefício concreto.
5. **Mantenha os testes robustos.** Verifique comportamento observável em vez de detalhes internos frágeis. Reaproveite padrões existentes, evite dependências novas e mocks que ocultem justamente a integração em questão.
6. **Valide proporcionalmente ao risco.** Execute primeiro as verificações focadas e amplie para as verificações relevantes do projeto quando o impacto justificar. Informe o que foi executado, o resultado e o que ficou sem validação.

## Informações a inspecionar

- Critérios de aceitação e contratos afetados, incluindo APIs ou formatos públicos quando relevantes.
- Testes próximos da implementação e padrões para casos de erro, integração ou UI.
- Scripts, manifests e ferramentas que o próprio projeto já configura.
- Alterações preexistentes que possam interferir na execução ou interpretação dos resultados.

## Conclusão

Considere o plano completo quando cada critério importante tiver uma verificação observável ou uma limitação explicitamente registrada, os casos de falha e regressão pertinentes forem considerados e os resultados não forem apresentados como mais abrangentes do que realmente são.

## Limitações

Testes não provam ausência de defeitos e não definem requisitos de produto. Se não houver ambiente, dados ou infraestrutura de teste disponíveis, proponha verificações viáveis e registre o que não pôde ser confirmado, sem instalar ferramentas por padrão.
