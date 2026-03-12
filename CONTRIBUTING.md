# Guia de Contribuição

Obrigado por considerar contribuir para o repositório legal-skills! Este guia fornece instruções para contribuir com novas habilidades e melhorias.

## Como Contribuir uma Nova Habilidade

### Estrutura de Diretórios

Cada habilidade deve seguir a seguinte estrutura:

```
skills/
└── nome-habilidade/
    ├── SKILL.md
    ├── references/
    │   ├── lei-123-2024.md
    │   ├── resolucao-456.md
    │   └── artigos-relevantes.md
    ├── scripts/
    │   └── main.py
    └── tests/
        └── test_main.py
```

### Formato do SKILL.md

O arquivo SKILL.md deve conter:

```markdown
---
name: Nome da Habilidade
description: Descrição clara da funcionalidade
license: MIT
author: Seu Nome
version: 1.0.0
tags: [tag1, tag2, tag3]
---

# Nome da Habilidade

## Descrição
Explicação detalhada do que a habilidade faz.

## Entrada
Especifique o formato esperado de entrada.

## Saída
Especifique o formato esperado de saída.

## Exemplos de Uso
Forneça exemplos práticos e específicos.

## Referências Legais
Liste as leis, resoluções e normas aplicáveis.

## Limitações
Descreva limitações conhecidas.
```

## Padrões de Código

### Python

- Use **type hints** em todas as funções:
  ```python
  def processar_documento(caminho: str) -> dict[str, Any]:
      """Processa um documento legal."""
      pass
  ```

- Escreva **docstrings** detalhadas (formato Google):
  ```python
  def validar_cnj(numero: str) -> bool:
      """Valida um número de processo CNJ.

      Args:
          numero: String contendo o número do processo.

      Returns:
          True se o número é válido, False caso contrário.

      Raises:
          ValueError: Se o número está vazio.
      """
      pass
  ```

- Siga o padrão **PEP 8** para estilo de código
- Limite linhas a 100 caracteres
- Use nomes descritivos em português para variáveis

### Testes

- Escreva testes com **pytest** para todas as funções
- Mínimo de 80% de cobertura de código
- Nomes de testes devem ser descritivos:
  ```python
  def test_validar_cnj_com_numero_valido_retorna_verdadeiro():
      assert validar_cnj("0000001-95.2016.8.26.0100") is True
  ```

- Organize testes em `tests/test_modulo.py`

## Processo de Pull Request

1. **Fork** o repositório
2. **Clone** para seu ambiente local
3. **Crie uma branch** descritiva:
   ```bash
   git checkout -b feat/nova-habilidade
   git checkout -b fix/corrige-validacao
   ```
4. **Commite** suas mudanças com mensagens claras:
   ```bash
   git commit -m "feat: adiciona habilidade de análise de contratos"
   ```
5. **Push** para sua branch
6. **Abra um Pull Request** com descrição detalhada

No PR, inclua:
- Descrição clara da mudança
- Referência para issues relacionadas (se houver)
- Verificação de testes passando
- Screenshots ou exemplos de saída (se relevante)

## Reportar Issues

### Antes de Reportar

1. Verifique se o problema já foi reportado
2. Teste se o problema se reproduz na versão mais recente
3. Colete informações sobre seu ambiente

### Como Reportar

Use o template de bug report fornecido, incluindo:
- Título descritivo
- Descrição clara do problema
- Passos para reproduzir
- Comportamento esperado vs atual
- Nome da habilidade afetada
- Informações de ambiente (Python, SO)

## Convenções de Nomenclatura

### Nomes de Habilidades

- Use **kebab-case** (palavras em minúsculas separadas por hífen)
- Seja **descritivo** e **específico**
- Exemplos válidos:
  - `cnj-parser` - Análise de números de processo
  - `contrato-review` - Revisão de contratos
  - `lgpd-validator` - Validação de conformidade LGPD
  - `jurisprudencia-search` - Busca de jurisprudência

- Exemplos inválidos:
  - `skill1` - Muito genérico
  - `validador` - Muito vago (use `validador-xxx`)
  - `ValidadorNomes` - Use kebab-case

## Padrões de Qualidade

### Referências Legais

- Todas as afirmações sobre lei devem ser **citadas** em `references/`
- Use fontes confiáveis:
  - Legislação oficial (planalto.gov.br)
  - Resoluções CNJ
  - Jurisprudência verificada
  - Artigos de especialistas publicados

- Documente com arquivos Markdown:
  ```markdown
  # Lei 13.709/2018 - LGPD

  ## Disposição Relevante

  Art. 5º - Definições...

  Fonte: http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm
  ```

- **Aviso de Disclaimers**: Adicione avisos sobre limitações legais

### Testes de Avaliação

- Crie arquivo `evals/nome-habilidade.evals.json`
- Mínimo de 8-10 casos de teste
- Inclua casos válidos, inválidos e edge cases
- Descreva o propósito de cada teste

Formato:
```json
{
  "skill": "nome-habilidade",
  "version": "1.0.0",
  "evals": [
    {
      "id": "test-001",
      "input": "entrada do usuário",
      "expected_output": {
        "contains": ["strings esperadas"],
        "valid": true
      },
      "tags": ["categoria", "edge-case"]
    }
  ]
}
```

## Padrões de Conteúdo

- Respeite a **precisão jurídica** - a imprecisão pode prejudicar usuários
- Cite sempre as **fontes legais** aplicáveis
- Valide against **jurisprudência atual**
- Considere a **jurisprudência dominante** dos tribunais brasileiros
- Teste com casos **reais** quando possível

## Código de Conduta Básico

### Princípios

- **Profissionalismo**: Mantenha discussões respeitosas e construtivas
- **Transparência**: Comunique claramente problemas e limitações
- **Qualidade**: Priorize código bem testado e documentado
- **Inclusão**: Respeite diferentes perspectivas e experiências

### Esperado

- Fornecer feedback construtivo
- Reconhecer contribuições de outros
- Comunicar mudanças significativas antecipadamente
- Responsabilizar-se por suas contribuições

### Não Tolerado

- Assédio de qualquer tipo
- Discriminação por qualquer motivo
- Ataques pessoais
- Spam ou promoção não solicitada

## Perguntas?

Abra uma discussão no repositório ou entre em contato com os mantenedores. Aprecia-se toda contribuição!
