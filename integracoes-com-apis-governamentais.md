# Integrações com APIs Governamentais

Fontes externas de dados que o CiênciaDados consome automaticamente.

---

## Portal da Transparência

**URL:** `https://portaltransparencia.gov.br`

O Portal da Transparência do Governo Federal é a principal fonte de dados sobre gastos públicos e sanções. O CiênciaDados baixa datasets mensais organizados por tema.

### Datasets Disponíveis

| Dataset | Conteúdo | Periodicidade |
|---|---|---|
| **CEIS** | Cadastro de Empresas Inidôneas e Suspensas | Mensal |
| **CNEP** | Cadastro Nacional de Empresas Punidas (Lei Anticorrupção) | Mensal |
| **CEAF** | Cadastro de Entidades e Agentes Públicos Expulsos | Mensal |
| **CEPIM** | Cadastro de Entidades Privadas Impedidas | Mensal |
| **Bolsa Família** | Beneficiários do programa de transferência de renda | Mensal |
| **Servidores Federais** | Quadro de servidores da administração federal | Mensal |
| **Licitações** | Processos licitatórios do governo federal | Mensal |
| **Despesas** | Gastos e empenhos do governo | Mensal |
| **Cartão Corporativo** | Despesas com cartão corporativo | Mensal |

### Formato de Download

Os datasets são disponibilizados como arquivos ZIP contendo CSVs. O sistema faz download, descompactação e importação automática com o parâmetro `-i`.

---

## Câmara dos Deputados

**API:** `https://dadosabertos.camara.leg.br`

API oficial de dados abertos da Câmara dos Deputados, com endpoints REST paginados.

### Dados Disponíveis

| Recurso | Conteúdo |
|---|---|
| **Deputados** | Cadastro completo dos 513 deputados (nome, partido, UF, foto, gabinete) |
| **Despesas (CEAP)** | Todas as despesas de cota parlamentar (Cota para Exercício da Atividade Parlamentar) |
| **Proposições** | PLs, PECs, MPs, emendas e outros tipos de proposições legislativas |
| **Votações** | Votações nominais (como cada deputado votou) |
| **Órgãos** | Estrutura de comissões, mesas diretoras e frentes parlamentares |

### Paginação

A API da Câmara utiliza paginação automática. O sistema itera por todas as páginas até coletar o conjunto completo de dados.

---

## Senado Federal

**API:** `https://dadosabertos.senado.leg.br`

API de dados abertos do Senado Federal, com dados sobre senadores e suas atividades.

### Dados Disponíveis

| Recurso | Conteúdo |
|---|---|
| **Senadores** | Cadastro completo dos 81 senadores |
| **Despesas (CEAPS)** | Cota para Exercício da Atividade Parlamentar dos Senadores |
| **Proposições** | Projetos de lei, emendas e matérias legislativas |
| **Comissões** | Estrutura e composição das comissões do Senado |

---

## TSE — Tribunal Superior Eleitoral

**Portal:** `https://odsele.tse.jus.br`

Portal ODS ELE do TSE com dados eleitorais em formato aberto.

### Dados Disponíveis

| Dataset | Conteúdo |
|---|---|
| **Candidatos** | Cadastro de candidatos por eleição (cargo, partido, UF, bens declarados) |
| **Prestação de Contas** | Receitas e despesas de campanha por candidato |
| **Eleições** | Resultados e dados gerais das eleições |

### Formato

Os dados do TSE são disponibilizados em arquivos de grande porte (centenas de MB a vários GB). O sistema gerencia download e descompactação automaticamente.

---

## CVM — Comissão de Valores Mobiliários

**API:** Dados abertos da CVM sobre mercado de capitais.

### Dados Disponíveis

| Dataset | Conteúdo |
|---|---|
| **Insider Trading** | Negociações de valores mobiliários por administradores de companhias abertas (ICVM 358 e CVM 44) |
| **Companhias Abertas** | Cadastro e informações de empresas listadas na B3 |

### Uso na Detecção de Insider Trading

Os dados de insider trading são cruzados com parlamentares e candidatos para detectar se políticos eleitos ou em campanha operam ações de empresas impactadas por legislação em tramitação.

---

## Receita Federal

**Fonte:** Cadastro Nacional da Pessoa Jurídica (CNPJ) e Cadastro de Pessoas Físicas (CPF).

### Dados Disponíveis

| Base | Conteúdo |
|---|---|
| **Empresas** | Razão social, capital social, situação cadastral, natureza jurídica |
| **Estabelecimentos** | Endereço, atividade econômica (CNAE), telefone, email |
| **Quadro Societário** | Sócios, qualificações, faixas etárias, representantes legais |

### Uso no Sistema

Os dados da Receita Federal alimentam os **catálogos mestres** de pessoas e empresas, permitindo cruzar fornecedores de recursos públicos com informações societárias para detectar conflitos de interesse.

---

## Resiliência de Conexão

Todas as integrações com APIs externas possuem:
- **Retry automático** em caso de falha de rede
- **Paginação transparente** para APIs com limites por página
- **Log de operações** para rastrear downloads bem-sucedidos ou falhos
- **Importação pós-download** opcional com flag `-i`

---

[Voltar à página inicial](./README.md) | [Ver visão geral de datasets](./datasets.md)
