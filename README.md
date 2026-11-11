# novo-teste-aes-simple03

Fábrica de software (*software factory*) orientada por um pipeline declarativo. Tarefas e issues do GitHub passam por etapas automatizadas — PRD, especificação técnica, tarefas, build, gate de checagens e revisão — com agentes e skills do Claude Code, até chegar a um Pull Request.

A configuração inteira vive em `factory/pipeline.yaml`: ordem das etapas, perfis, ciclos, modos de aprovação e rótulos do GitHub.

## Tecnologias

- Python 3 (motor do pipeline em `factory/bin/`)
- Bash (scripts de merge, PR e gate em `factory/scripts/`)
- Git e GitHub CLI (`gh`)
- Claude Code (agentes em `.claude/agents/` e skills em `.claude/skills/`)

## Pré-requisitos

- Git
- Python 3.10+
- [GitHub CLI](https://cli.github.com/) (`gh`), autenticado com `gh auth login`
- [Claude Code](https://claude.com/claude-code)

## Instalação

```bash
git clone https://github.com/<usuario>/novo-teste-aes-simple03.git
cd novo-teste-aes-simple03
```

Não há dependências externas a instalar para o motor.

## Uso

Validar o pipeline (confere o arquivo e os agentes, skills e scripts citados):

```bash
python3 factory/bin/factory validate
```

Ver a ordem efetiva das etapas de um ciclo:

```bash
python3 factory/bin/factory plan <ciclo>
```

Exibir o pipeline como JSON:

```bash
python3 factory/bin/factory json
```

Para executar jobs, use a skill `/factory` dentro do Claude Code.

## Testes e checagens

O gate (`factory/scripts/gate.sh`) executa as checagens do projeto. Ele detecta automaticamente `package.json` (scripts `lint`, `typecheck`, `test`, `build`) e `pytest` (quando há `pyproject.toml` ou `requirements.txt`). Enquanto nenhuma dessas detecções existir, o gate falha com uma mensagem pedindo que os comandos reais sejam configurados em `factory/scripts/gate.sh`.

## Estrutura

```
factory/
  pipeline.yaml   configuração da fábrica (fonte única)
  prompts/        texto de cada etapa
  bin/            motor: factory, job, route, gate, _pipeline.py
  scripts/        gate.sh, pr.sh, merge.sh
  dashboard.html  painel da fábrica
.claude/
  agents/         agentes por etapa (builder, reviewers, approver...)
  skills/         skills do SDLC e da fábrica
```

Jobs em execução ficam em `factory/jobs/` e worktrees em `.worktrees/`; ambos não são versionados.
