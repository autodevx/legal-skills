---
name: audiencia-classifier
description: >
  Classifica publicações de audiência judicial por tipo e modalidade.
  Use quando receber textos de intimações, pautas de audiência ou
  publicações do diário de justiça e precisar categorizar automaticamente
  em INICIAL/INSTRUÇÃO/JULGAMENTO/CONCILIAÇÃO × PRESENCIAL/VIRTUAL/HÍBRIDA.
  Aplicável a publicações de qualquer tribunal brasileiro (TJs, TRTs, TRFs).
  Extrai também data, hora, vara, juiz e partes quando disponíveis.
license: MIT
metadata:
  author: autodev-tecnologia
  version: "1.0.0"
  domain: legal-ops
  personas: [analista, legal-ops, advogado]
  tags: [audiência, classificação, intimação, pauta, diário-justiça]
---

# Classificador de Audiências Judiciais

## Contexto

Publicações de audiência em diários oficiais e sistemas processuais (PJe, eSAJ, PROJUDI)
chegam em texto livre, sem estrutura padronizada. Esta skill classifica automaticamente
cada publicação extraindo tipo, modalidade e metadados relevantes.

## Taxonomia de classificação

### Eixo 1: Tipo de audiência

| Tipo | Sinônimos / Indicadores no texto | Finalidade |
|------|----------------------------------|------------|
| **INICIAL** | "audiência inicial", "audiência inaugural", "primeira audiência", "art. 334 CPC" | Tentativa de conciliação/mediação antes da instrução (CPC art. 334) |
| **CONCILIAÇÃO** | "audiência de conciliação", "audiência de mediação", "tentativa de acordo", "CEJUSC" | Sessão dedicada exclusivamente a acordo |
| **INSTRUÇÃO** | "audiência de instrução", "AIJ", "instrução e julgamento", "oitiva de testemunhas", "depoimento pessoal" | Coleta de provas orais (CPC art. 358+) |
| **JULGAMENTO** | "audiência de julgamento", "sessão de julgamento", "leitura de sentença" | Pronunciamento de decisão |
| **UNA** | "audiência una", "audiência de instrução e julgamento" | Instrução + julgamento na mesma sessão (comum em JECs e Justiça do Trabalho) |
| **JUSTIFICAÇÃO** | "audiência de justificação", "justificação prévia" | Produção antecipada de prova para tutela provisória |
| **CUSTÓDIA** | "audiência de custódia" | Criminal — apresentação do preso ao juiz (art. 310 CPP / Res. CNJ 213/2015) |
| **ADMOESTAÇÃO** | "audiência admonitória", "admoestação" | Intimação pessoal do devedor para cumprimento |

### Eixo 2: Modalidade

| Modalidade | Indicadores no texto |
|------------|---------------------|
| **PRESENCIAL** | "sala de audiência", "comparecer na vara", "sede do fórum", endereço físico mencionado |
| **VIRTUAL** | "videoconferência", "plataforma", "Microsoft Teams", "Zoom", "Google Meet", "Cisco Webex", "link de acesso", "sala virtual" |
| **HÍBRIDA** | menção simultânea de sala física E link virtual; "modalidade híbrida" |
| **NÃO IDENTIFICADA** | quando o texto não permite determinar a modalidade |

### Regra de prioridade
- Se o texto contém TANTO indicadores presenciais QUANTO virtuais → classificar como **HÍBRIDA**
- Se não há indicador claro → **NÃO IDENTIFICADA** (nunca inventar)

## Metadados a extrair

Além da classificação, extrair quando disponíveis:

| Campo | Formato | Obrigatório |
|-------|---------|-------------|
| data_audiencia | DD/MM/AAAA | Sim (se presente no texto) |
| hora | HH:MM | Sim (se presente no texto) |
| vara | texto livre | Desejável |
| comarca | texto livre | Desejável |
| juiz | nome do magistrado | Desejável |
| processo | número CNJ (usar cnj-parser se disponível) | Desejável |
| partes | autor vs. réu | Desejável |
| plataforma_virtual | nome da plataforma (se virtual) | Desejável |
| link_acesso | URL da sala virtual | Desejável |

## Output padrão

### Publicação única

```json
{
  "texto_original": "...",
  "classificacao": {
    "tipo": "INSTRUÇÃO",
    "modalidade": "VIRTUAL",
    "confianca": "ALTA"
  },
  "metadados": {
    "data_audiencia": "15/04/2025",
    "hora": "14:00",
    "vara": "2ª Vara Cível",
    "comarca": "Belo Horizonte/MG",
    "juiz": null,
    "processo": "0002150-34.2023.8.13.0024",
    "partes": "João da Silva vs. Empresa XYZ Ltda",
    "plataforma_virtual": "Microsoft Teams",
    "link_acesso": "https://teams.microsoft.com/..."
  },
  "indicadores_detectados": [
    "oitiva de testemunhas → INSTRUÇÃO",
    "plataforma Microsoft Teams → VIRTUAL"
  ]
}
```

### Lote de publicações

Retornar tabela consolidada:

```
| # | Processo | Tipo | Modalidade | Data | Hora | Vara | Confiança |
```

E estatísticas:
```
Total de audiências classificadas: N
Por tipo: INICIAL (X) | INSTRUÇÃO (Y) | CONCILIAÇÃO (Z) | ...
Por modalidade: PRESENCIAL (X) | VIRTUAL (Y) | HÍBRIDA (Z)
```

## Nível de confiança

| Nível | Critério |
|-------|----------|
| **ALTA** | Indicador explícito e inequívoco no texto (ex: "audiência de instrução") |
| **MÉDIA** | Indicador indireto mas razoável (ex: "oitiva" sem mencionar "instrução") |
| **BAIXA** | Classificação por exclusão ou contexto ambíguo |

## Particularidades por ramo

### Justiça do Trabalho (TRTs)
- "Audiência inaugural" = INICIAL (tentativa de conciliação obrigatória — CLT art. 846)
- "Audiência de prosseguimento" = continuação de INSTRUÇÃO
- Audiência UNA é muito comum em rito sumaríssimo
- CEJUSC trabalhista = CONCILIAÇÃO

### Juizados Especiais (JECs)
- "Audiência de conciliação, instrução e julgamento" = UNA
- Sempre tentar classificar como UNA se JEC

### Criminal
- "Audiência de custódia" = CUSTÓDIA
- "AIJ" em criminal = INSTRUÇÃO
- "Audiência do art. 28-A CPP" = acordo de não persecução

## Referências

Ler `references/tipos-audiencia.md` para taxonomia completa com exemplos de texto real.

## Limites

- Classificação baseada apenas no texto fornecido — não consulta sistemas processuais
- Textos muito curtos ou genéricos podem resultar em confiança BAIXA
- Não diferencia audiência designada de audiência redesignada (a menos que o texto explicite)
- Publicações em linguagem muito informal podem não ser classificadas corretamente
