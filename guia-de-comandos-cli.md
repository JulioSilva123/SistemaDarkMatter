# Guia de Comandos CLI

Referência completa dos comandos disponíveis na interface de linha de comando do CiênciaDados.

---

## Visão Geral

```
cienciadados [COMANDO] [OPCOES]
```

| # | Comando | Descrição |
|---|---------|-----------|
| 1 | `testar-conexao` | Testa a conexão com o SQL Server |
| 2 | `conjunto-criar` | Cria um grupo de dados |
| 3 | `conjunto-associar` | Associa uma fonte a um grupo |
| 4 | `conjuntos` | Lista grupos e suas fontes |
| 5 | `importar` | Importa CSV (arquivo ou pasta) |
| 6 | `status` | Exibe status das importações |
| 7 | `layout` | Exibe colunas de um arquivo |
| 8 | `mapear` | Vincula coluna a um campo padrão |
| 9 | `campos` | Exibe mapeamentos cadastrados |
| 10 | `analisar perfil` | Perfil estatístico |
| 11 | `analisar duplicidades` | Detecta duplicatas |
| 12 | `analisar cruzamento` | Cruza duas bases |
| 13 | `analisar validacao` | Valida CPF/CNPJ |
| 14 | `analisar temporal` | Análise de datas |
| 15 | `analisar consistencia` | Sancionado recebendo benefício |
| 16 | `analisar outliers` | Valores numéricos anômalos |
| 17 | `analisar texto` | Textos duplicados/copy-paste |
| 18 | `achados` | Exibe inconsistências |
| 19 | `resetar` | Apaga todos os dados |
| 20 | `datasets` | Lista datasets da Transparência |
| 21 | `baixar` | Baixa datasets da Transparência |
| 22 | `parlamentar-datasets` | Lista dados base Câmara/Senado |
| 23 | `parlamentar` | Baixa dados Câmara/Senado |
| 24 | `parlamentar-despesas` | Baixa gastos de gabinete |
| 25 | `tse-datasets` | Lista datasets abertos do TSE |
| 26 | `tse` | Baixa dados de campanha (TSE) |
| 27 | `cvm-datasets` | Lista tabelas abertas da CVM |
| 28 | `cvm` | Baixa dados CVM (Insider Trading) |
| 29 | `analisar insider` | Achados CVM x Parlamentares |
| 30 | `analisar parlamentar-sancao` | Parlamentar sancionado (CEIS) |
| 31 | `analisar parlamentar-fornecedor` | Fornecedor Cota Parl. impedido |
| 32 | `analisar rede` | Monitor de cartéis e Laranjas |
| 33 | `relatorio` | Gera Relatório de Auditoria MD |

---

## 1. Conexão

### `testar-conexao`

Testa a conexão com o SQL Server usando as configurações do `.env`.

```bash
cienciadados testar-conexao
```

Sem opções. Exibe sucesso ou falha.

---

## 2. Conjuntos de Dados

### `conjunto-criar`

Cria um novo grupo para organizar fontes de dados.

```bash
cienciadados conjunto-criar -n "Sancoes Governo Federal" -d "Dados do Portal da Transparencia"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--nome` | `-n` | Sim | Nome do conjunto |
| `--descricao` | `-d` | Não | Descrição do conjunto |

---

### `conjunto-associar`

Associa uma fonte de dados existente a um conjunto.

```bash
cienciadados conjunto-associar -c 1 -f 1
cienciadados conjunto-associar -c 1 -f 2
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--conjunto` | `-c` | Sim | ID do conjunto |
| `--fonte` | `-f` | Sim | ID da fonte a associar |

> Use `status` para ver os IDs das fontes disponíveis.

---

### `conjuntos`

Lista todos os conjuntos com suas fontes associadas. Também mostra fontes órfãs (sem conjunto).

```bash
cienciadados conjuntos
```

Sem opções.

---

## 3. Download de Dados (APIs e Portais)

O sistema possui rotinas de ingestão automáticas (ETLs), permitindo baixar os arquivos e descompactá-los na pasta de dados brutos. Integrados com o parâmetro `--importar-apos` ou `-i`, o sistema baixa e importa para o SQL Server em uma única operação.

### Portal da Transparência

Baixa sanções e pagamentos federais diretamente do portal do governo.
- `datasets`: Lista os catálogos disponíveis para o mês atual.
- `baixar`: Realiza o download direto passando o parâmetro `-d`.
  ```bash
  cienciadados baixar -d despesas -p 202401 -i
  ```

### Dados Parlamentares (Câmara Federal e Senado)

Acessa as APIs de dados abertos para extrair Deputados, Senadores e Suas Despesas.
- `parlamentar-datasets`: Traz as opções (Ex: `deputados`, `senadores`, `orgaos`).
- `parlamentar`: Efetua a conexão e a paginação unificada dos ocupantes dos cargos.
  ```bash
  cienciadados parlamentar -d deputados -i
  cienciadados parlamentar -d senadores -i
  ```
- `parlamentar-despesas`: Lista gastos de gabinete (`-m` para max. deputados).

### Tribunal Superior Eleitoral (TSE)

Coleta planilhas pesadas (candidatos ou prestações eleitorais).
- `tse-datasets`: Lista o portal odsele do TSE para downloads.
- `tse`: Exige o ano de apuração `-a`.
  ```bash
  cienciadados tse -d prestacao_contas -a 2022 -i
  ```

### Insider Trading (CVM - B3)

Busca dados de administradores publicando compra/venda de ativos (ICVM 358 e CVM 44).
- `cvm-datasets`: Lê os relatórios (ex: `insider` e `empresas`).
- `cvm`: Consome a compilação do ano enviado.
  ```bash
  cienciadados cvm -d insider -a 2024 -i
  ```

---

## 4. Importação

### `importar`

Importa arquivos CSV para o SQL Server. Aceita um arquivo ou uma pasta inteira.

```bash
# Arquivo único
cienciadados importar -c "dados\brutos\CEIS.csv"

# Pasta inteira (importa todos os CSVs)
cienciadados importar -c "dados\brutos"

# Com opções
cienciadados importar -c "arquivo.csv" -f "CEIS" -s "CEIS" -e "utf-8" --separador ";"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--caminho` | `-c` | Sim | Caminho do CSV ou pasta |
| `--fonte` | `-f` | Não | Nome da fonte (auto-detecta do nome do arquivo) |
| `--sigla` | `-s` | Não | Sigla da fonte |
| `--encoding` | `-e` | Não | Encoding do CSV (auto-detecta) |
| `--separador` | — | Não | Separador do CSV (auto-detecta) |

**Funcionalidades:**
- **Auto-detecção**: encoding, separador e nome da fonte
- **Importação em lote**: passa uma pasta e importa todos os CSVs
- **Resumível**: se interromper com `Ctrl+C`, retoma de onde parou ao executar novamente
- **Barra de progresso**: mostra andamento em tempo real
- **Move para processados**: após importar, move o CSV de `dados/brutos/` para `dados/processados/`

> Se fechar a janela ou der `Ctrl+C`, basta executar o mesmo comando. O sistema detecta a importação parcial e continua automaticamente.

---

### `status`

Exibe o status de todos os arquivos importados: nome, fonte, linhas, colunas e status.

```bash
cienciadados status
```

Status possíveis: `OK` (concluído), `ERRO`, `PARCIAL` (interrompido), `PENDENTE`.

---

### `banco-entidades`

Varre um arquivo CSV já importado no formato agnóstico e popula os catálogos definitivos/mestres do negócio (`pessoas` e `empresas`), identificando a unicidade pelo CPF/CNPJ.

```bash
cienciadados banco-entidades -i 5 -c "cpf,cnpjCpfFornecedor" -n "nomeFornecedor"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo de origem a ser analisado |
| `--campos-documento` | `-c` | Sim | Nomes exatos das colunas JSON que contem CPFs/CNPJs |
| `--campo-nome` | `-n` | Sim | Nome da coluna JSON que armazena a Razão ou Nome |

---

### `layout`

Exibe as colunas (schema) de um arquivo importado, incluindo tipo detectado, nulos, únicos e campo mapeado.

```bash
cienciadados layout -i 1
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo |

---

## 5. Mapeamento de Campos

### `mapear`

Cadastra o vínculo entre um campo padrão do sistema e a coluna real no CSV. Ao mapear, os layouts existentes são atualizados automaticamente.

```bash
# Mapear CPF/CNPJ na fonte #1
cienciadados mapear -f 1 -p cpf_cnpj -c "CPF OU CNPJ DO SANCIONADO"

# Mapear nome
cienciadados mapear -f 1 -p nome -c "NOME DO SANCIONADO" -d "Nome completo do sancionado"

# Mapear datas
cienciadados mapear -f 1 -p data_inicio -c "DATA INÍCIO SANÇÃO"
cienciadados mapear -f 1 -p data_final -c "DATA FINAL SANÇÃO"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--fonte` | `-f` | Sim | ID da fonte de dados |
| `--papel` | `-p` | Sim | Campo padrão (ver tabela abaixo) |
| `--coluna` | `-c` | Sim | Nome real da coluna no CSV |
| `--descricao` | `-d` | Não | Descrição do mapeamento |

**Campos padrão disponíveis:**

| Campo | Descrição |
|-------|-----------|
| `cpf_cnpj` | Documento CPF ou CNPJ |
| `nome` | Nome da pessoa/empresa |
| `data_inicio` | Data de início |
| `data_final` | Data de término |
| `uf` | Unidade federativa |
| `orgao` | Órgão responsável |

> Se o campo já estiver mapeado para a mesma fonte, ele é **atualizado** (não duplica).

---

### `campos`

Exibe os campos padrão e todos os mapeamentos cadastrados.

```bash
# Todos os mapeamentos
cienciadados campos

# Filtrar por fonte
cienciadados campos -f 1
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--fonte` | `-f` | Não | Filtrar por fonte |

---

## 6. Análise e Mineração

### `analisar perfil`

Gera perfil estatístico completo de um arquivo. Mostra por coluna: tipo, preenchimento, nulos, únicos, distribuição por UF/categoria/esfera.

```bash
cienciadados analisar perfil -i 1
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo |

---

### `analisar duplicidades`

Detecta registros duplicados por hash (100% iguais) e por campo-chave.

```bash
# Auto-detecta campo CPF/CNPJ
cienciadados analisar duplicidades -i 1

# Informar campo manualmente
cienciadados analisar duplicidades -i 1 -c "NR_DOCUMENTO"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Não | ID do arquivo (todos se omitido) |
| `--campo` | `-c` | Não | Campo-chave (auto-detecta CPF/CNPJ) |

---

### `analisar cruzamento`

Cruza duas bases por CPF/CNPJ (exato) e/ou Nome (fuzzy matching).

```bash
# Cruzar CEIS x CNEP
cienciadados analisar cruzamento -a 1 -b 3

# Informar campo e tolerância
cienciadados analisar cruzamento -a 1 -b 3 -c "NR_DOCUMENTO" -t 90
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--arquivo-a` | `-a` | Sim | ID do primeiro arquivo |
| `--arquivo-b` | `-b` | Sim | ID do segundo arquivo |
| `--campo` | `-c` | Não | Campo para cruzar (auto-detecta) |
| `--tolerancia` | `-t` | Não | Tolerância fuzzy em % (default: 85) |

---

### `analisar validacao`

Valida dígitos verificadores de CPFs e CNPJs. Detecta documentos inválidos.

```bash
# Auto-detecta coluna de CPF/CNPJ
cienciadados analisar validacao -i 1

# Informar coluna manualmente
cienciadados analisar validacao -i 1 -c "NR_CPF"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Não | ID do arquivo (todos se omitido) |
| `--campo` | `-c` | Não | Coluna de CPF/CNPJ (auto-detecta) |

---

### `analisar temporal`

Analisa datas: sanções vencidas, ativas, sem data final, datas inconsistentes e picos temporais.

```bash
# Auto-detecta colunas de data
cienciadados analisar temporal -i 1

# Informar colunas manualmente
cienciadados analisar temporal -i 1 --campo-inicio "DT_ABERTURA" --campo-final "DT_ENCERRAMENTO"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Não | ID do arquivo (todos se omitido) |
| `--campo-inicio` | — | Não | Coluna de data início (auto-detecta) |
| `--campo-final` | — | Não | Coluna de data final (auto-detecta) |

---

### `analisar consistencia`

Detecta entidades presentes simultaneamente em base restritiva (CEIS, CNEP) e base de benefícios (Auxílio Brasil, CEPIM). Severidade **4 (crítica)**.

```bash
# CEIS (restritivo #1) vs Auxílio Brasil (benefício #5)
cienciadados analisar consistencia -r 1 -b 5

# Informar campo manualmente
cienciadados analisar consistencia -r 1 -b 5 --campo-a "CPF" --campo-b "NR_CPF"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--restritivo` | `-r` | Sim | ID do arquivo restritivo (sanções) |
| `--beneficio` | `-b` | Sim | ID do arquivo de benefício |
| `--campo-a` | — | Não | Coluna CPF/CNPJ no restritivo (auto-detecta) |
| `--campo-b` | — | Não | Coluna CPF/CNPJ no benefício (auto-detecta) |

> Achados desta análise indicam possível irregularidade: entidade sancionada recebendo recurso público.

---

### `analisar outliers`

Detecta valores numéricos fora do padrão usando IQR (Interquartile Range). Auto-detecta colunas numéricas.

```bash
# Auto-detecta colunas numéricas
cienciadados analisar outliers -i 1

# Coluna específica, fator extremo
cienciadados analisar outliers -i 1 -c "VALOR_CONTRATO" -f 3.0
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo |
| `--campo` | `-c` | Não | Coluna numérica (analisa todas se omitido) |
| `--fator` | `-f` | Não | Fator IQR: `1.5` moderado (default), `3.0` extremo |

Mostra para cada coluna: média, mediana, faixa normal e quantidade de outliers.

---

### `analisar fragmentacao`

Detector rigoroso de Fuga de Licitação. Baseado na Nova Lei de Licitações, ele varre contratos ou tabelas financeiras procurando a mesma entidade (CPF/CNPJ) faturando valores fracionados abaixo da margem de lei, cujo montante acumulado numa janela rolante de 90 dias fure o teto sem concorrência pública.

```bash
cienciadados analisar fragmentacao -i 8 -d "dt_emissao" -v "valor_rs" -a "cnpj" -l 60000 -j 80
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo de faturas ou empenhos |
| `--campo-data` | `-d` | Sim | Nome da coluna de Data da emissão do repasse |
| `--campo-valor` | `-v` | Sim | Nome da coluna monetária |
| `--agrupador` | `-a` | Sim | Coluna do Recebedor a ser somado (CNPJ/Nome) |
| `--categoria` | `-c` | Não | Opcional. Permite isolar o limite legal para um código de despesa |
| `--limite` | `-l` | Não | Teto Legal Licitatório de Dispensa. (R$ Padrão: 59900) |
| `--janela` | `-j` | Não | Quantidade de dias da Rolling Window. (Padrão: 90 dias) |

---

### `analisar sazonalidade`

Avalia arquivos com valores declarados e datas e detecta picos atípicos de gastos ou repasses ocorridos às vésperas de campanhas políticas (compara Anos Pares com a média imediatamente dos anos comuns/ímpares).

```bash
cienciadados analisar sazonalidade -i 8 -d "dt_emissao_nota" -v "vr_pago" -a "cnpjCpfFornecedor" -l 60
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo de repasses ou série histórica |
| `--campo-data` | `-d` | Sim | Nome da coluna de Data/Competência (texto ou int formatado) |
| `--campo-valor` | `-v` | Sim | Nome da coluna monetária (`R$` ou decimais) |
| `--agrupador` | `-a` | Sim | Coluna chave para agrupar (CPF, Nome Recebedor, CNPJ) |
| `--limiar` | `-l` | Não | % mínimo de anomalia pra considerar disparo (Padrão 50%) |

---

### `analisar texto`

Detecta textos longos duplicados (copy-paste). Útil para encontrar fundamentações legais idênticas, observações copiadas, etc.

```bash
# Auto-detecta colunas com texto longo
cienciadados analisar texto -i 1

# Coluna específica, mínimo 100 caracteres
cienciadados analisar texto -i 1 -c "FUNDAMENTACAO" --min-chars 100
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--id-arquivo` | `-i` | Sim | ID do arquivo |
| `--campo` | `-c` | Não | Coluna de texto (auto-detecta se omitido) |
| `--min-chars` | — | Não | Tamanho mínimo do texto (default: 50) |

Sinaliza grupos com 3+ textos idênticos como padrão suspeito.

---

### `analisar insider`

Cruza políticos eleitos ou candidatos com relatórios de negociações de valores mobiliários na B3 (Res. CVM 44). Usado para inferir *Insider Trading* comparando datas de operações contra tramitações legais.

```bash
cienciadados analisar insider -p 10 -c 12 -t 85
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--parlamentares` | `-p` | Sim | ID do arquivo de Políticos/Candidatos |
| `--cvm` | `-c` | Sim | ID do arquivo de Valores Mobiliários (CVM 44) |
| `--tolerancia` | `-t` | Não | Tolerância no cruzamento fuzzy % (default: 85) |

Gera um achado gravíssimo e evidência caso exista correspondência do político administrando ou comprando papéis na Bolsa.

---

### `analisar parlamentar-sancao`

Cruza nomes de Deputados Federais ou Senadores diretamente com as bases de sanções do governo (CEIS ou CNEP), utilizando algoritmo de *Fuzzy Match* para identificar se o político responsável se encontra na lista de empresas inidôneas e suspensas.

```bash
cienciadados analisar parlamentar-sancao -p 1 -s 3 -t 85
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--parlamentares` | `-p` | Sim | ID do arquivo de Legisladores |
| `--sancao` | `-s` | Sim | ID do Cadastro Restritivo (CEIS/CNEP) |
| `--tolerancia` | `-t` | Não | Tolerância fuzzy % (default: 85) |

---

### `analisar parlamentar-fornecedor`

Mapeia se os gastos de Despesas de Gabinete (cota parlamentar), CNPJ/CPF foram destinados a empresas ou entidades oficialmente impedidas/punidas. Cruza a Nota Fiscal do deputado com CEIS e CEPIM.

```bash
cienciadados analisar parlamentar-fornecedor -d 5 -s 4
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--despesas` | `-d` | Sim | ID do arquivo de Despesas de Deputados |
| `--sancao` | `-s` | Sim | ID da base restritiva com CNPJs suspensos |

Gera "Achado Crítico de Fornecedor Sancionado" especificando em nome de qual deputado aquele favorecido operou.

---

## 7. Resultados

### `achados`

Exibe inconsistências, anomalias e possíveis crimes detectados pelas análises.

```bash
# Últimos 20 achados
cienciadados achados

# Com filtros
cienciadados achados -l 50 -s 3 -t "duplicidade"
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--limite` | `-l` | Não | Máximo de achados (default: 20) |
| `--severidade` | `-s` | Não | Severidade mínima (1-5) |
| `--tipo` | `-t` | Não | Tipo de achado |

---

### `relatorio`

Extrai, consolida e converte todo o conhecimento de fraudes do Banco de Dados para um documento limpo e legível em Markdown (MD), pronto para entrega de auditorias ou reportagens.

```bash
# Exportar tudo com Severidade 3 ou maior
cienciadados relatorio -o resultado.md

# Direcionar apenas a uma analise (ex. Insider Trading cujo ID de arquivo base foi o 12)
cienciadados relatorio -o insider_investigacao.md -s 4 -i 12
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--saida` | `-o` | Sim | Nome do arquivo a ser criado / caminho final |
| `--severidade` | `-s` | Não | Oculta alertas abaixo da severidade min. (default: 3) |
| `--id-arquivo` | `-i` | Não | Filtra a apuração por uma fonte carregada e específica |

---

## 8. Manutenção

### `resetar`

Apaga **todos** os dados do banco e recomeça do zero. Reinsere campos padrão e tipos de achado.

```bash
cienciadados resetar --confirmar
```

| Opção | Atalho | Obrigatório | Descrição |
|-------|--------|-------------|-----------|
| `--confirmar` | — | Sim | Flag de segurança |

> Este comando apaga **todas** as tabelas: registros, layouts, arquivos, fontes_dados, campos_fonte, conjuntos_dados, achados e logs. Não pode ser desfeito.

---

[Voltar à página inicial](./index.md) | [Ver fluxo de uso recomendado](./fluxo-de-uso.md)
