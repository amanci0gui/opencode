---
name: "Root Cause Debugging"
description: "Investiga bugs ou falhas de compilação e execução com causa incerta antes de alterar o código; não use para erros triviais cuja causa já está demonstrada."
---

# Diagnóstico de causa raiz

Use esta skill para orientar uma investigação baseada em evidências. O objetivo é descobrir por que o comportamento falha antes de escolher uma correção, não acumular hipóteses nem fazer alterações exploratórias sem critério.

## Quando usar

- O resultado observado diverge do esperado e a causa ainda não está clara.
- Um teste, build ou processo de execução falha sem uma explicação já demonstrada.
- O defeito parece depender de estado, fluxo entre módulos ou condições específicas.

Não a carregue para erros óbvios e localizados cuja causa esteja confirmada, nem para implementar uma funcionalidade sem defeito a diagnosticar.

## Procedimento

1. **Delimite o sintoma.** Registre o resultado esperado, o resultado observado, as condições em que ocorre e o alcance conhecido. Separe fatos de suposições.
2. **Busque uma reprodução ou evidência concreta.** Use o teste, log, mensagem de erro, entrada ou fluxo disponível. Se não houver reprodução, diga isso e identifique a informação mínima necessária; não afirme uma causa como certa.
3. **Inspecione o caminho relevante.** Leia o código relacionado, seus chamadores, estado e configuração necessários, contratos e testes próximos. Compare com o último comportamento conhecido quando houver evidência disponível.
4. **Formule hipóteses discrimináveis.** Para cada hipótese, diga que evidência a apoiaria ou refutaria. Investigue uma de cada vez, começando pela verificação mais simples e informativa que esteja disponível.
5. **Isole a causa.** Siga o fluxo de dados e controle até o ponto em que o resultado se desvia do esperado. Não confunda uma exceção, mensagem ou linha que manifesta a falha com a causa que a produz.
6. **Corrija somente após sustentar a causa.** Faça a menor alteração que trate a causa identificada. Não combine uma refatoração ampla ou mudanças sem relação com a correção.
7. **Confirme a correção.** Reproduza a falha antes da mudança quando possível, acrescente ou ajuste um teste de regressão e execute as verificações relevantes já disponíveis no projeto.

## Informações a inspecionar

Mensagens de erro e testes falhos, comportamento esperado, alterações relacionadas, código e chamadores pertinentes, configuração efetivamente usada e evidências de execução disponíveis. Proteja dados sensíveis em logs e relatórios.

## Conclusão

Conclua quando a causa estiver apoiada por evidências, a mudança tratar essa causa sem expandir o escopo e a reprodução ou o teste relevante confirmar a correção. Se não for possível reproduzir ou validar, informe a incerteza e os próximos dados necessários.

## Limitações

Uma análise somente de leitura não pode confirmar comportamento que dependa de execução, estado externo ou dados indisponíveis. Não invente logs, resultados ou causas. Esta skill não substitui a estratégia de testes mais ampla quando a alteração exigir cobertura adicional.
