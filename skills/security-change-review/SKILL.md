---
name: "Security Change Review"
description: "Avalia riscos de segurança concretamente relacionados a código alterado em fluxos sensíveis; não use como auditoria geral nem reporte vulnerabilidades sem cenário verificável."
---

# Revisão defensiva de segurança de alterações

Use esta skill de forma restrita para examinar o impacto de segurança do código efetivamente alterado. Ela complementa a revisão independente; não substitui uma auditoria de segurança, análise de dependências ou revisão de infraestrutura.

## Quando usar

- A alteração toca autenticação, autorização, escopo de acesso ou limites de confiança.
- Há mudanças em validação de entrada, consulta de dados, serialização, saída para outro contexto ou tratamento de dados sensíveis.
- Uma alteração muda decisões de acesso, exposição de informações, tratamento de erros ou registros relacionados a segurança.

Não a carregue para toda revisão de diff, para código sem relação com uma superfície sensível ou para procurar problemas hipotéticos sem evidência no fluxo alterado.

## Procedimento

1. **Limite o escopo.** Inspecione o diff e selecione somente caminhos de código afetados. Entenda a origem dos dados e os pontos onde decisões ou efeitos relevantes ocorrem.
2. **Siga dados e controle.** Verifique, quando aplicável, como entradas são validadas, como permissões são decididas, como dados são transformados e para onde são enviados ou persistidos.
3. **Examine controles relacionados.** Considere se autorização é aplicada no ponto que protege o recurso, se erros falham de forma segura, se a validação é adequada ao domínio e se dados sensíveis podem aparecer em respostas ou logs.
4. **Procure regressões introduzidas pela mudança.** Compare com contratos e controles existentes. Não suponha que uma interface visual, validação no cliente ou nome de função prove que a proteção é aplicada no limite correto.
5. **Relate somente achados concretos.** Para cada um, informe localização, condição necessária, caminho afetado e consequência. Separe fatos de perguntas em aberto; não classifique uma suspeita como vulnerabilidade confirmada.
6. **Recomende verificação proporcional.** Consulte testes existentes para os comportamentos de acesso, entrada e erro relevantes. Não execute testes ou ações fora do escopo autorizado.

## Segurança e limites de uso

Mantenha a análise defensiva e restrita ao código autorizado. Não produza payloads de exploração, passos de intrusão, cadeias de ataque ou instruções para atingir sistemas. Para descrever um risco, use o fluxo e as condições observáveis no código e proponha uma verificação segura em testes controlados quando pertinente.

## Critérios de conclusão

Os achados, se existirem, são verificáveis e diretamente ligados ao diff; cada um tem cenário e impacto proporcionais. Se não houver achado concreto, diga que nenhum foi identificado no escopo revisado e registre o que ficou fora da análise. Não declare o sistema seguro com base apenas nesta revisão.

## Limitações

Uma revisão localizada não prova a segurança do sistema inteiro nem substitui análise especializada. A falta de contexto de autenticação, implantação, dados ou chamadas externas deve ser registrada, não preenchida com suposições.
