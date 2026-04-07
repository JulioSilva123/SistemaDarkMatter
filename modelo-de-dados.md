# Modelo de Dados

Como o CiênciaDados organiza e estrutura os dados internamente.

---

## Filosofia: Formato Agnóstico

O sistema adota uma abordagem de **armazenamento agnóstico**: cada linha de um CSV importado é armazenada como um registro JSON na tabela de registros. Isso permite que qualquer arquivo seja importado sem necessidade de criar tabelas específicas para cada fonte.

Os metadados (colunas, tipos, nulos, únicos) ficam na tabela de layouts. O vínculo com campos padrão do sistema fica na tabela de mapeamentos.

---

## Catálogos e Referências

### Campos Padrão
Campos semânticos que o sistema reconhece independentemente da fonte de dados. Exemplos: `cpf_cnpj`, `nome`, `data_inicio`, `data_final`, `uf`, `orgao`. Cada campo padrão tem um identificador único e uma descrição.

### Tipos de Achado
Classificação das inconsistências detectadas. Cada tipo tem nome, descrição e severidade padrão associada. Exemplos: duplicidade, valor inválido, conflito de dados, sanção ativa, servidor expulso, entidade impedida, outlier, padrão suspeito.

### Tipos de Entidade
Classificação de pessoas e empresas nos catálogos mestres: pessoa física, pessoa jurídica, órgão público, etc.

---

## Fontes de Dados

Cada arquivo importado gera um registro na tabela de fontes de dados com:
- Nome da fonte (derivado do nome do arquivo)
- Sigla identificadora
- Data de importação
- Status (OK, ERRO, PARCIAL, PENDENTE)
- Total de linhas e colunas

---

## Registros e Layouts

### Registros
Cada linha do CSV importado vira um registro com:
- ID do arquivo de origem
- Hash da linha (para detecção de duplicatas)
- Dados completos em formato JSON

### Layouts
Metadados de cada coluna detectada:
- Nome original da coluna no CSV
- Tipo de dado detectado (string, inteiro, decimal, data)
- Campo padrão mapeado (se houver)
- Contagem de nulos
- Contagem de valores únicos

---

## Mapeamentos

Vínculo entre um campo padrão do sistema e uma coluna real em uma fonte específica:
- ID da fonte de dados
- Campo padrão (ex: `cpf_cnpj`)
- Nome real da coluna no CSV (ex: "CPF OU CNPJ DO SANCIONADO")
- Descrição opcional

Uma mesma coluna pode ser mapeada para múltiplos campos padrão em fontes diferentes.

---

## Catálogos Mestres

### Pessoas
Consolidação de todas as pessoas físicas encontradas nos dados importados, identificadas pelo CPF:
- CPF
- Nome completo
- Data de nascimento (quando disponível)
- Nome da mãe (quando disponível)
- Fontes de origem

### Empresas
Consolidação de todas as pessoas jurídicas encontradas, identificadas pelo CNPJ:
- CNPJ
- Razão social
- Situação cadastral
- Endereço
- Capital social
- Fontes de origem

### Órgãos
Catálogo de órgãos e entidades públicas identificados nos dados.

---

## Achados

Cada inconsistência ou irregularidade detectada gera um achado com:
- Tipo de achado
- Severidade (1 a 5)
- Descrição do que foi encontrado
- Evidências (dados que suportam a conclusão)
- Entidades envolvidas (CPF/CNPJ)
- Fonte de dados de origem
- Data de detecção

---

## Conjuntos de Dados

Grupos lógicos para organizar fontes de dados relacionadas. Um conjunto pode conter múltiplas fontes e uma fonte pode pertencer a múltiplos conjuntos.

Exemplo: conjunto "Sanções do Governo Federal" contendo CEIS, CNEP, CEAF e CEPIM.

---

## Logs

Registro de todas as operações executadas pelo sistema:
- Comando executado
- Data e hora
- Duração
- Status (sucesso, erro)
- Mensagens de erro (se aplicável)

---

[Voltar à página inicial](./README.md) | [Ver arquitetura](./arquitetura-do-sistema.md)
