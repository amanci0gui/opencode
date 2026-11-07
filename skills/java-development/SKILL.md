---
name: "Java Development"
description: "Apoia mudanças em projetos Java detectados, seguindo toolchain, organização, exceções e testes existentes; considere Spring somente quando o projeto o utilizar."
---

# Desenvolvimento em Java

Use esta skill somente depois de confirmar que o projeto utiliza Java. O objetivo é respeitar a versão e as convenções existentes, não converter o projeto para um framework ou arquitetura predeterminada.

## Quando usar

- Implementar ou modificar código Java, APIs, módulos ou testes.
- Diagnosticar falhas de compilação ou execução em um projeto Java identificado.
- Alterar tratamento de exceções, concorrência ou integração com um framework já presente.

Não use em repositórios sem Java, nem imponha Spring Boot, Maven, Gradle, JUnit ou bibliotecas que não estejam adotadas ou justificadas pelo projeto.

## Pré-requisitos e inspeção

Examine o que existir: `pom.xml`, wrappers e configurações do Gradle, `settings.gradle(.kts)`, toolchains, módulos, versão-alvo do Java, estrutura de pacotes, dependências, testes e convenções locais. Identifique o framework efetivamente utilizado, se houver, e leia os chamadores e contratos afetados.

## Procedimento recomendado

1. Mantenha a organização de pacotes e os limites entre responsabilidades já praticados. Não crie camadas ou abstrações sem uma necessidade concreta.
2. Use tipagem e APIs compatíveis com a versão-alvo. Preserve contratos públicos, comportamento observável e compatibilidade com consumidores.
3. Trate exceções de forma explícita e coerente com o projeto. Não capture e descarte falhas nem converta erros em sucesso aparente.
4. Considere gerenciamento de recursos, concorrência e cancelamento somente quando fizerem parte do fluxo alterado; preserve os mecanismos já adotados.
5. Em APIs, siga o framework e os padrões existentes para entrada, validação, status e erros. Use padrões Spring somente se o projeto realmente usar Spring.
6. Acrescente ou ajuste testes conforme os padrões detectados e execute as verificações já configuradas, ampliando o escopo proporcionalmente ao risco.
7. Para erros de build ou runtime sem causa demonstrada, use investigação baseada em evidências antes de editar; não trate uma mensagem isolada como prova da causa.

## Critérios de conclusão

A implementação corresponde à versão e ao build detectados, integra-se aos pacotes e contratos locais, considera os erros relevantes e tem testes e verificações relatados sem alegar resultados não observados.

## Limitações

Os padrões variam significativamente entre Java simples e aplicações com frameworks. Não recomende framework, biblioteca, ferramenta de build ou desenho arquitetural sem necessidade demonstrável no repositório.
