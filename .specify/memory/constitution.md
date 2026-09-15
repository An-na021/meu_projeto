<!--
Sync Impact Report (REMOVER ANTES DO COMMIT)
- Versao: 1.0.0 -> 2.0.0 (MAJOR: redefinicao incompativel de escopo e principios)
- Principios modificados:
  * I. Testes Primeiro (TDD) — INEGOCIÁVEL -> II. Desenvolvimento Guiado por
    Testes (TDD) — INEGOCIÁVEL (renomeada e reelaborada)
  * II. Biblioteca Autocontida (Library-First) -> removida (escopo mudou de
    biblioteca Python para sistema de pagamentos)
  * III. Código em Inglês -> removida (regra absorvida em Restrições Adicionais)
  * IV. Documentação em Português -> removida (regra absorvida em Restrições
    Adicionais)
  * V. Simplicidade e Qualidade (YAGNI) -> V. Simplicidade e Foco (YAGNI)
    (mantida e reelaborada)
- Seções adicionadas: I. Arquitetura em Camadas, III. Erro como Valor,
  IV. Neutralidade de Infraestrutura (padrões de engenharia sem citação de
  tecnologia), seção "Restrições Adicionais" reescrita e seção "Workflow de
  Desenvolvimento" reelaborada
- Seções removidas: nenhuma (todas as seções do template preservadas;
  conteúdo técnico de Python/pytest/pyproject removido por violar a regra de
  neutralidade de infraestrutura)
- TODOs: nenhum placeholder pendente
-->

# Constituição do Sistema de Pagamentos

## Princípios Centrais

### I. Arquitetura em Camadas

O sistema DEVE ser organizado em camadas com responsabilidades bem definidas
e dependências em direção única ao interior: cada camada DEVE depender apenas
das camadas mais internas, nunca do contrário. A lógica de negócio (domínio)
DEVE estar isolada das preocupações técnicas de apresentação, integração e
infraestrutura. Nenhuma camada externa DEVE acessar recursos de
infraestrutura (ex.: persistência, comunicação externa) contornando o
domínio. As fronteiras entre camadas DEVERÃO ser definidas por abstrações
(interfaces/portas) declaradas da camada interna para a externa, nunca o
inverso.

Rationale: camadas com dependências unidirecionais tornam cada parte
testável isoladamente, substituível e evoluível sem efeito cascata,
qualidade essencial em um sistema de pagamentos.

### II. Desenvolvimento Guiado por Testes (TDD) — INEGOCIÁVEL

Todo novo comportamento DEVE começar por um teste escrito antes da
implementação. O ciclo Red-Green-Refactor DEVE ser seguido estritamente nesta
ordem: escrever o teste, confirmar que ele falha pela razão certa (Red),
implementar a quantidade mínima de código para o teste passar (Green) e
refatorar somente com toda a suíte verde. Nenhum código de produção é aceito
sem um teste antecedente que tenha sido observado falhando. Alterar um teste
existente para fazer o código passar é proibido; qualquer alteração exige
justificativa explícita de contrato ou de requisito.

Rationale: o teste escrito primeiro define o contrato esperado da camada,
torna o domínio auditável e fornece a rede de segurança para refatorações.

### III. Erro como Valor

Falhas de negócio NÃO DEVEM ser representadas como exceções de sistema;
DEVEM ser retornadas e tratadas como valores explícitos no fluxo do domínio
(ex.: resultado tipado que representa sucesso ou falha comercial prevista).
Cada caminho de código DEVE declarar em seu contrato as falhas de negócio
possíveis, e o chamador DEVE tratar explicitamente a falha retornada.
Exceções de sistema ficam reservadas a condições imprevistas e de
infraestrutura (ex.: recurso indisponível, erro interno não mapeado) e NÃO
DEVEM ser usadas para expressar regra de negócio.

Rationale: uma recusa de pagamento é um comportamento comercial previsto,
não um colapso do sistema; representá-la como valor a torna testável,
rastreável e obrigatoriamente tratada pela camada que a recebe.

### IV. Neutralidade de Infraestrutura

Esta constituição DEVE tratar apenas de padrões de engenharia e NÃO DEVE
fixar bancos de dados, bibliotecas de estado, frameworks ou qualquer
tecnologia concreta. Essas escolhas pertencem ao artefato de Plano e podem
mudar a cada entrega. O código DEVE depender das abstrações definidas pela
Arquitetura em Camadas (Princípio I), nunca de fornecedores concretos, para
permanecer compatível com qualquer decisão registrada no Plano.

Rationale: decisões tecnológicas são mutáveis e específicas de cada entrega;
isolá-las atrás de abstrações mantém o domínio e a suíte de testes estáveis.

### V. Simplicidade e Foco (YAGNI)

O sistema DEVE permanecer mínimo, claro e com escopo definido para cada
entrega. Não se adiciona abstração, funcionalidade ou integração especulativa;
toda dependência externa DEVE ser justificada no Plano antes de ser adotada.
A qualidade é garantida por testes que documentam o comportamento esperado,
não por código morto ou complexidade acidental.

Rationale: simplicidade preserva a auditabilidade e o baixo custo de
manutenção exigidos de uma operação de pagamentos.

## Restrições Adicionais

- É estritamente proibido citar frameworks, bancos de dados ou bibliotecas
  de estado nesta constituição; essas escolhas pertencem ao artefato de Plano.
- As regras de negócio de pagamento DEVERÃO ser especificadas, representadas
  e testadas sem depender de banco de dados ou de biblioteca de estado.
- O contrato público de cada camada DEVE ser estável e testável de forma
  isolada das demais camadas.
- Os fluxos de erro retornado (Princípio III) DEVERÃO estar cobertos por
  testes tanto quanto os fluxos de sucesso.
- Código e mensagens de commit DEVERÃO ser escritos em inglês; especificações,
  planos, guias e esta constituição DEVERÃO ser escritos em português do
  Brasil.

## Workflow de Desenvolvimento

- Cada feature DEVE ser especificada e desdobrada em tarefas antes da
  implementação, aplicando os padrões de engenharia desta constituição.
- O desenvolvimento DEVE seguir o ciclo Red-Green-Refactor em cada unidade
  entregável, conforme o Princípio II.
- A suíte de testes DEVE passar por completo antes de cada commit.
- Commits são pequenos, relacionados a uma tarefa específica e escritos em
  inglês.
- Alterações em testes existentes exigem justificativa explícita; alterar o
  teste para fazer o código passar é proibido.
- Nenhuma feature é considerada entregue sem a especificação no Plano e a
  atualização correspondente da documentação em português.

## Governança

- Esta constituição prevalece sobre qualquer outra prática interna do
  projeto; conflitos são resolvidos com base nela.
- Emendas à constituição exigem documentação da mudança, incremento de
  versão semântica, aprovação e, quando aplicável, plano de migração.
- Toda revisão (PR/Review) DEVE verificar a conformidade com os princípios
  desta constituição, em especial: Red-Green-Refactor, falhas de negócio como
  valor e a ausência de citação de tecnologia específica neste documento.
- A versão da constituição segue SemVer: MAJOR para remoção ou redefinição
  incompatível de princípios, MINOR para novos princípios ou seções, PATCH
  para esclarecimentos, correções de texto e refinamentos não semânticos.
- A conformidade é revista a cada ciclo de entrega; desvios DEVERÃO ser
  documentados como exceções explícitas.

**Versão**: 2.0.0 | **Ratificada**: 2026-09-15 | **Última Emenda**: 2026-09-15