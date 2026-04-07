# Fluxos de Investigação

O CiênciaDados transforma dados brutos em relatórios de auditoria através de fluxos automatizados.

---

## Do Dado Bruto ao Relatório

O sistema baixa dados de portais governamentais, importa automaticamente com detecção de encoding e separador, consolida CPFs e CNPJs em catálogos mestres, executa todos os analisadores e gera um relatório de auditoria em Markdown com achados agrupados por severidade.

## Investigação Parlamentar Completa

Coleta dados de deputados e senadores, importa despesas de gabinete de todos os 513 deputados, cruza com listas de sanção (CEIS/CNEP), detecta fornecedores irregulares, analisa fragmentação de despesas, mapeia redes de influência e gera relatório focado em conflitos de interesse.

## Detecção de Redes de Laranjas

Extrai todos os CPFs e CNPJs dos dados importados, constrói um grafo de co-ocorrência (entidades que aparecem juntas nos mesmos registros), detecta clusters por dados compartilhados (email, telefone, endereço), cruza com domínios web via OSINT e identifica operadores centrais da rede.

## Auditoria de Licitações

Importa dados de contratos e licitações, valida CNPJs dos fornecedores, detecta empresas fantasmas ou inativas, identifica cartel por revezamento de vitórias, flagra aditivos abusivos e fragmentação de despesas, cruza com listas de sanção para encontrar fornecedores impedidos.

## Insider Trading Político

Importa dados de parlamentares e registros de negociação da CVM, cruza nomes por fuzzy matching, verifica se negociações coincidem com votações de leis relevantes e gera achados de severidade máxima quando detecta político negociando ações suspeitas.

---

## Classificação de Severidade

| Severidade | Nível | Descrição |
|---|---|---|
| 5 | Máxima | Crime evidente, ação imediata |
| 4 | Crítica | Forte indício de irregularidade grave |
| 3 | Alta | Padrão suspeito que requer investigação |
| 2 | Média | Anomalia que merece atenção |
| 1 | Baixa | Inconsistência leve ou informativa |

---

[Voltar à página inicial](./README.md)
