<!-- ===== Sync Impact Report (temporary, remove before commit) =====
Version change: not previously versioned (template scaffold) -> 1.0.0
Modified principles: n/a (no prior constitution)
Added sections: Full initial constitution on the resolved scaffold:
  1.0 Security Requirements - Logs e Dados Sensiveis (principal no. 1)
  1.1 Inspection-First Development (principal no. 2)
  1.2 Test Discipline (principal no. 3)
  1.3 Strict Typing (principal no. 4)
  1.4 Change Control - Ask-First (principal no. 5)
  2.0 Security Requirements (detail)
  3.0 Development Workflow (detail)
  4.0 Governance (detail)
Removed sections: n/a
Deferred items: project display name provisional "Financial Application";
  confirm preferred name via /speckit.amend
================================================================== -->

# Financial Application Constitution

Constituição de governança para o projeto de aplicação financeira.
Salvaguarda (prevalece sobre) quaisquer outras práticas, documentos ou acordos locais.
Qualquer alteração nos princípios, seções ou regras de governança exige o processo
formal de emenda (emendas) descrito em Governança.

## Core Principles

### 1. Security Requirements - Logs e Dados Sensíveis Sem PII

Logs de aplicação DEVEM conter zero PII e zero senhas; dados sensíveis de transações
(números, saldos, contas, chaves) NÃO PODEM ser registrados ou expostos (zero PII /
senhas em logs). É PROIBIDO ler ou exibir arquivos `.env` e qualquer outro dado
sensível de transações fora do fluxo autorizado. Segredos e credenciais NUNCA são
armazenados em texto simples (nenhuma gravação de segredos em texto simples).
Racional: em aplicações financeiras, vazamento de PII/segredos gera exposição
regulatória, fraude e perda de confiança; a restrição é absoluta e auditável.

### 2. Inspeção Primeiro (Inspection-First)

Antes de editar qualquer arquivo, o agente DEVEM inspecionar o arquivo e o contexto
circundante (importações, padrões, arquivos relacionados). Nenhuma edição pode ser
feita às cegas ou baseada em suposição sobre o conteúdo atual.
Racional: edição cega quebra contratos e convenções existentes; inspeção prévia
garante mudanças mínimas, idiomáticas e consistentes com o padrão do projeto.

### 3. Disciplina de Testes (Test Discipline)

Todo trabalho deve rodar os testes locais afetados e o resultado deve estar
verde/verificado antes de finalizar. É expressamente PROIBIDO remover ou desativar
testes para esconder falhas; falhas devem ser corrigidas, nunca silenciadas.
Racional: testes são a rede de segurança do software financeiro; ocultar falhas
corrompe a confiança e aumenta o risco de defeitos em produção.

### 4. Tipagem Estrita (Strict Typing)

Tipagem estrita DEVE ser mantida em todo o código: sem `any`, `@ts-ignore` ou
bypasses de tipo sem justificativa documentada e aprovada. O nível de strictness do
projeto não pode ser rebaixado.
Racional: contratos de tipos explícitos previnem classes de bugs e facilitam
refatoração segura em uma base de código financeira crítica.

### 5. Controle de Mudanças - Ask-First

As seguintes ações exigem autorização explícita e prévia (Ask-First) do responsável
humano antes de qualquer execução: adicionar novas dependências (NPM/libs);
modificar migrações ou o esquema de banco de dados; alterar infraestrutura/Docker.
Racional: dependências, esquema de banco e infraestrutura são riscos de alto
impacto e difíceis de reverter; a gate humana preserva segurança e estabilidade.

## Security Requirements

Requisitos não negociáveis de segurança de dados:

- **Zero PII/senhas em logs**: nenhum valor pessoal, senha, token ou segredo pode
  aparecer em logs (structured logs com redação/máscara obrigatória).
- **Proibição de leitura/exibição de dados sensíveis**: é proibido ler ou exibir
  `.env` ou dados sensíveis de transações; acesso só ocorre em fluxo autorizado.
- **Nunca armazenar segredos em texto simples**: credenciais e chaves usam somente
  um gerenciador de segredos/injeção de ambiente; nada em plain text versionado.
- **Nunca expor dados em logs**: transações, contas, valores ou chaves não podem
  ser registrados ou aparecer em saída de diagnóstico.

Estas regras são auditáveis: qualquer log, artefato ou commit que viole os pontos
acima constitui falha de não conformidade e deve ser revertido/corrigido.

## Development Workflow

- **Inspecionar antes de editar**: todo arquivo a ser modificado é lido antes; o
  padrão do contexto é respeitado (mesmas convenções, bibliotecas e estilos já
  utilizados).
- **Rodar testes locais afetados**: após cada mudança, a suite de testes afetada
  roda e deve passar; falha exige correção (nunca exclusão ou desativação).
- **Manter tipagem estrita**: sem `any`/`@ts-ignore` salvo exceção documentada e
  aprovada; verificar typecheck do projeto após mudanças.
- **Gates de qualidade**: lint e typecheck devem estar limpos no resultado final.

## Governance

Esta constituição é a autoridade de governança do projeto e prevalece sobre outras
práticas de desenvolvimento. Ações do tipo Ask-First (dependências, migrações,
infra/Docker) exigem aprovação explícita antes de executar. Qualquer emenda deve
incluir documentação, aprovação do responsável e plano de migração quando aplicável.

**Procedimento de emenda**: propostas de emenda são registradas, revisadas e
aprovadas; o documento é atualizado com novo número de versão e data; o Sync Impact
Report temporário é removido antes do commit.

**Política de versionamento (SemVer)**: MAJOR para remoção/redefinição de princípio
incompatível; MINOR para princípio/seção novo ou expansão material; PATCH para
clarificação, correção de redação ou refinamento semântico.

**Expectativa de revisão de conformidade**: todos os PRs e revisões DEVEM verificar
aderência aos princípios (zero PII em logs, testes verdes, tipagem estrita, Ask-First
e regras de segurança); desvio exige justificativa documentada. Use `AGENTS.md` (raiz do
repo) como guia de orientação de desenvolvimento em runtime.

**Version**: 1.0.0 | **Ratified**: 2026-09-15 | **Last Amended**: 2026-09-15