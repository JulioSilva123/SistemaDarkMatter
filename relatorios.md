# Relatórios

O CiênciaDados gera relatórios de auditoria profissionais em formato Markdown.

---

## Geração de Relatórios

O comando `relatorio` extrai, consolida e converte todo o conhecimento de fraudes do Banco de Dados para um documento limpo e legível em Markdown (MD), pronto para entrega de auditorias ou reportagens.

```bash
# Exportar tudo com Severidade 3 ou maior
cienciadados relatorio -o resultado.md -s 3

# Direcionar apenas a uma análise específica
cienciadados relatorio -o insider_investigacao.md -s 4 -i 12
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--saida` | `-o` | Sim | Nome do arquivo a ser criado / caminho final |
| `--severidade` | `-s` | Não | Oculta alertas abaixo da severidade min. (default: 3) |
| `--id-arquivo` | `-i` | Não | Filtra a apuração por uma fonte específica |

---

## Conteúdo do Relatório

O relatório gerado inclui:

- **Sumário Executivo** — visão geral dos achados por severidade
- **Achados por Categoria** — agrupados por tipo de irregularidade
- **Detalhamento** — evidências, entidades envolvidas e gravidade
- **Recomendações** — próximos passos sugeridos para investigação

---

## Boas Práticas

- Use severidade mínima 3 para relatórios de auditoria formal
- Filtre por arquivo específico para investigações direcionadas
- O formato Markdown permite fácil conversão para PDF ou Word
- Inclua a data da análise e versão do sistema no nome do arquivo

---

[Voltar à página inicial](./README.md)
