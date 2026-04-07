# Pipeline de Dados (ETL)

Como os dados fluem desde a origem até a análise no CiênciaDados.

---

## Visão Geral do Pipeline

O pipeline do CiênciaDados segue três etapas principais: **Extração**, **Transformação** e **Carga** (ETL), com uma etapa adicional de **Consolidação de Entidades** antes da análise.

```
Extração ──► Transformação ──► Carga ──► Consolidação ──► Análise
```

---

## 1. Extração

### Download de APIs Governamentais
O sistema se conecta diretamente às APIs e portais de dados abertos do governo brasileiro para baixar datasets atualizados:

- **Portal da Transparência** — datasets mensais de sanções, despesas, servidores e benefícios
- **Câmara dos Deputados** — API de dados abertos com paginação automática para deputados, despesas e proposições
- **Senado Federal** — API de dados abertos para senadores, despesas CEAPS e proposições
- **TSE** — portal ODS ELE para dados eleitorais e prestações de contas
- **CVM** — dados de insider trading e companhias abertas

### Upload Manual
Arquivos CSV obtidos por outros meios podem ser colocados na pasta `dados/brutos/` para importação manual.

---

## 2. Transformação

### Auto-detecção
Antes de importar, o sistema analisa automaticamente cada arquivo:

- **Encoding** — detecta se o arquivo é UTF-8, Latin-1, Windows-1252, etc. (usando chardet)
- **Separador** — identifica se usa vírgula, ponto-e-vírgula, tabulação ou pipe
- **Nome da fonte** — derivado automaticamente do nome do arquivo

### Detecção de Schema
Para cada coluna, o sistema detecta:
- Tipo de dado (string, inteiro, decimal, data)
- Taxa de preenchimento (nulos vs preenchidos)
- Cardinalidade (valores únicos)

---

## 3. Carga

### Formato Agnóstico
Os dados são carregados no SQL Server em formato agnóstico:
- Cada linha vira um registro JSON na tabela de registros
- Metadados de colunas vão para a tabela de layouts
- Não é necessário criar tabelas específicas para cada fonte

### Resiliência
A importação é **resumível**: se interrompida (Ctrl+C ou queda de energia), retoma automaticamente de onde parou ao reexecutar o comando. Uma barra de progresso mostra o andamento em tempo real.

### Pós-importação
Após importar com sucesso, o arquivo CSV é movido automaticamente de `dados/brutos/` para `dados/processados/`.

---

## 4. Consolidação de Entidades

Esta etapa transforma dados brutos em conhecimento estruturado:

### Extração de CPFs e CNPJs
O sistema varre as colunas mapeadas de cada arquivo importado e extrai todos os documentos encontrados.

### Catálogo de Pessoas
Consolida todas as pessoas físicas por CPF, reunindo nomes, datas de nascimento e nomes de mãe encontrados em diferentes fontes.

### Catálogo de Empresas
Consolida todas as pessoas jurídicas por CNPJ, reunindo razão social, situação cadastral, endereços e capital social.

### Enriquecimento OSINT
Quando disponível, o sistema enriquece os catálogos com dados obtidos de fontes OSINT (Open Source Intelligence).

---

## 5. Análise

Com os dados carregados e as entidades consolidadas, os analisadores entram em ação:

- **Análises básicas** — perfil estatístico, duplicidades, validação de documentos, outliers
- **Análises de consistência** — sanções vencidas, datas inconsistentes, textos duplicados
- **Análises de fraude** — fragmentação de despesas, sazonalidade eleitoral, insider trading, redes de influência
- **Cruzamentos** — fuzzy matching entre bases diferentes (ex: parlamentares x sanções)

Cada análise gera **achados** classificados por severidade, prontos para consulta e geração de relatórios.

---

## Monitoramento

O comando `status` fornece visão completa do pipeline:
- Quais arquivos foram importados
- Quantas linhas e colunas cada um possui
- Status de cada importação (OK, ERRO, PARCIAL, PENDENTE)

---

[Voltar à página inicial](./README.md) | [Ver arquitetura](./arquitetura-do-sistema.md)
