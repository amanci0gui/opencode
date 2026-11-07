---
name: "Safe Refactoring"
description: "Orienta refatorações de código existente que devem preservar comportamento observável e contratos; não use quando a tarefa muda o comportamento intencionalmente."
---

# Refatoração com preservação de comportamento

Use esta skill quando a intenção for mudar a estrutura interna mantendo o comportamento externo. Ela complementa as regras do agente sobre escopo e contratos; não transforma toda alteração de código em refatoração.

## Quando usar

- A tarefa pede reorganizar, simplificar ou melhorar a estrutura sem alterar comportamento público.
- É necessário avaliar o risco de uma refatoração que atravessa chamadores ou módulos.

Não use para funcionalidades ou correções cuja mudança de comportamento seja intencional. Se uma correção de bug for descoberta durante a refatoração, separe-a ou peça autorização para mudar o escopo.

## Procedimento

1. **Defina o limite da refatoração.** Identifique o que deve mudar estruturalmente e o que precisa continuar observável para consumidores.
2. **Mapeie comportamento e contratos.** Inspecione interfaces públicas, chamadores relevantes, formatos serializados, persistência, erros, ordenação, efeitos colaterais e estados quando aplicáveis.
3. **Avalie a linha de base.** Leia testes e verificações existentes. Se faltar cobertura de um comportamento crítico, proponha um teste de caracterização quando viável; se não for viável, registre o risco antes de avançar.
4. **Faça mudanças graduais.** Altere uma preocupação estrutural por vez. Preserve a lógica e os efeitos; não aproveite a refatoração para adicionar abstrações, dependências ou mudanças funcionais sem justificativa e autorização.
5. **Compare a superfície externa.** Revise assinaturas, contratos, formatos, erros e efeitos observáveis que possam ter sido afetados, além do diff interno.
6. **Valide a preservação.** Execute os testes focados e outras verificações pertinentes já configuradas. Amplie a validação de acordo com o alcance da refatoração e informe limitações.

## Informações a inspecionar

Comportamento atual, contratos e consumidores, testes existentes, limites entre módulos e alterações preexistentes. Não assuma que uma assinatura interna é privada se houver consumidores externos ou chamadas indiretas relevantes.

## Critérios de conclusão

- A estrutura pretendida foi alterada sem mudança pública não planejada.
- Os comportamentos importantes têm verificações pertinentes ou limitações explícitas.
- A alteração permanece restrita à refatoração autorizada.

## Limitações

Testes não demonstram equivalência completa. Se o comportamento existente for desconhecido ou não puder ser exercitado, não declare a refatoração segura sem ressalvas. Esta skill não prescreve arquitetura, padrão de projeto ou ferramenta de teste.
