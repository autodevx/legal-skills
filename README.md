# legal-skills 🇧🇷

**Skills de IA para operações jurídicas no Brasil.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-4%20disponíveis-green)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## O que é isso?

Um conjunto de skills compatíveis com [Claude Desktop](https://claude.ai) e [skills.sh](https://skills.sh) para automatizar tarefas do dia a dia de **departamentos jurídicos, controllers e equipes de Legal Ops** no Brasil.

## Por que isso existe?

As ferramentas de IA jurídica dominantes — Harvey, CoCounsel, LexisNexis Protégé — são 100% anglófonas e ignoram o sistema processual brasileiro.

As ferramentas nacionais focam em petições para advogado contencioso individual. **Ninguém cobre o dia a dia operacional** de quem faz Legal Ops, controller jurídico ou compliance corporativo.

Estas skills foram construídas para quem trabalha com CNJ, PJe, eSAJ, CPC/2015, LGPD e os tribunais brasileiros — de verdade.

## Skills disponíveis

| Skill | Persona | O que faz |
|-------|---------|-----------|
| [`cnj-parser`](skills/cnj-parser/) | Analista / Dev | Valida e extrai componentes de números de processos no formato CNJ |
| [`contrato-review`](skills/contrato-review/) | Advogado / Analista | Analisa contratos brasileiros e identifica cláusulas problemáticas |
| [`contingencia-report`](skills/contingencia-report/) | Controller / CFO | Gera relatórios de contingência jurídica (CPC 25 + IAS 37) |
| [`audiencia-classifier`](skills/audiencia-classifier/) | Analista / Legal Ops | Classifica publicações de audiência por tipo e modalidade |

## Como usar

### Opção 1: Claude Desktop (recomendado)

1. Clone este repositório:
```bash
git clone https://github.com/autodevx/legal-skills.git
```

2. No Claude Desktop, adicione o diretório `skills/` como fonte de skills

3. Use linguagem natural:
```
"valida esse número CNJ: 0002150-34.2023.8.13.0024"
"analisa esse contrato e aponta cláusulas abusivas"
"gera relatório de contingência para esses 15 processos"
"classifica essas publicações de audiência"
```

### Opção 2: skills.sh

```bash
# Instalar skill individual
skills install autodevx/legal-skills/cnj-parser

# Instalar todas
skills install autodevx/legal-skills
```

### Opção 3: Uso direto dos scripts

```bash
# CNJ Parser (Python, sem dependências externas)
python skills/cnj-parser/scripts/cnj_validator.py "0002150-34.2023.8.13.0024"

# Batch processing
cat processos.txt | python skills/cnj-parser/scripts/cnj_validator.py --batch --format csv
```

## Estrutura de cada skill

```
skills/nome-da-skill/
├── SKILL.md          ← Instruções e framework (lido pelo Claude)
├── references/       ← Material jurídico de referência (legislação, jurisprudência)
├── scripts/          ← Scripts auxiliares (Python, sem dependências externas)
└── tests/            ← Testes automatizados (pytest)
```

## Roadmap

### Em desenvolvimento

- [ ] `peticao-draft` — Geração de petições iniciais compatíveis com PJe
- [ ] `decisao-summarizer` — Resumo estruturado de acórdãos
- [ ] `processo-extractor` — Extração de dados processuais estruturados
- [ ] `lgpd-compliance` — Auditoria de contratos de tratamento de dados

### Planejado

- [ ] `jurisprudencia-search` — Mapeamento de teses por tribunal
- [ ] `risco-classifier` — Classificação de risco processual por valor e tema
- [ ] `provisao-calculus` — Cálculo de provisão com fórmulas atuariais
- [ ] `exec-report` — Relatório mensal do jurídico para diretoria
- [ ] `sla-tracker` — Dashboard de SLA de respostas jurídicas
- [ ] `prazo-monitor` — Monitoramento de prazos processuais

## Contribuindo

Contribuições são bem-vindas! Veja o [guia de contribuição](CONTRIBUTING.md) para detalhes sobre:

- Como criar uma nova skill
- Padrões de código e qualidade
- Processo de pull request

**Especialmente valorizamos** contribuições nos arquivos `references/` — material jurídico brasileiro curado e atualizado é o diferencial deste projeto.

## Quem usa

Desenvolvido pela [Autodev Tecnologia](https://autodev.com.br) para uso em projetos de automação jurídica com clientes de médio e grande porte.

## Licença

[MIT](LICENSE) — use, modifique, contribua.

---

<p align="center">
  Feito por <a href="https://github.com/autodevx">Autodev Tecnologia</a> ·
  <a href="https://autodev.com.br">site</a> ·
  <a href="https://linkedin.com/company/autodevx">linkedin</a>
</p>
