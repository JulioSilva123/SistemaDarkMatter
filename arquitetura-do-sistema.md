# Arquitetura do Sistema

Visão geral da arquitetura do CiênciaDados.

---

## Visão Macro

O CiênciaDados é construído como um sistema **multi-camada** com separação clara entre:

- **Infraestrutura** — conexão com banco de dados, leitura de arquivos, variáveis de ambiente
- **Núcleo de negócio** — entidades, regras de validação, mapeamentos e enums
- **Serviços de aplicação** — ingestão de dados, importação de fontes governamentais, análises, geração de relatórios
- **Interfaces** — CLI (linha de comando), UI (desktop), Web Admin e Web App

---

## Princípios Arquiteturais

### Separação de Responsabilidades
Cada camada tem um papel bem definido. A infraestrutura não conhece regras de negócio. Os serviços de aplicação orquestram sem saber detalhes de conexão com banco. As interfaces não processam dados — apenas apresentam resultados.

### Agnosticismo de Dados
O sistema foi projetado para trabalhar com qualquer CSV, independente de origem, encoding ou separador. A auto-detecção permite importar dados sem configuração manual prévia.

### Mapeamento Flexível
Colunas de arquivos CSV são vinculadas a campos padrão do sistema através de mapeamentos configuráveis. Isso permite que o mesmo analisador funcione com fontes de dados diferentes.

### Análise Extensível
Novos tipos de análise podem ser adicionados sem modificar os existentes. Cada analisador opera de forma independente e gera achados com severidade padronizada.

---

## Camadas do Sistema

```
┌─────────────────────────────────────────────────────┐
│                 INTERFACES                          │
│  ┌─────────┐ ┌─────────┐ ┌──────────┐ ┌──────────┐ │
│  │  CLI    │ │  UI     │ │  Web     │ │  Web     │ │
│  │ (Console│ │(Desktop)│ │  Admin   │ │  App     │ │
│  │  Python)│ │  (.NET) │ │ (FastAPI)│ │ (FastAPI)│ │
│  └────┬────┘ └────┬────┘ └────┬─────┘ └────┬─────┘ │
└───────┼───────────┼───────────┼────────────┼───────┘
        │           │           │            │
        ▼           ▼           ▼            ▼
┌─────────────────────────────────────────────────────┐
│           SERVIÇOS DE APLICAÇÃO                     │
│                                                     │
│  Ingestão  │  Importação  │  Análise  │  Relatórios │
│  de Dados  │  de Fontes   │  e Mineração│  e Achados │
│            │  Governamentais│           │            │
└──────────────────────────┬──────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────┐
│              NÚCLEO DE NEGÓCIO                      │
│                                                     │
│   Entidades  │  Mapeamentos  │  Validações  │ Enums │
│   (Pessoa,   │  (Campo       │  (CPF, CNPJ) │ (Tipo │
│   Empresa,   │   padrão ↔    │              │  de   │
│   Achado)    │   Coluna CSV) │              │Achado)│
└──────────────────────────┬──────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────┐
│               INFRAESTRUTURA                        │
│                                                     │
│   Conexão SQL Server  │  Leitor CSV  │  Configuração│
│   (pyodbc/SQLAlchemy) │  (chardet)   │  (.env)      │
└─────────────────────────────────────────────────────┘
```

---

## Componentes de Interface

### Console CLI (Python)
Interface principal de linha de comando. Operações de importação, análise e geração de relatórios são executadas via comandos textuais. Usa Click como framework de CLI e Rich para formatação visual no terminal.

### Console UI (.NET)
Interface gráfica desktop para usuários que preferem operação visual. Espelha as funcionalidades do CLI em formulários e painéis interativos.

### Web Admin (Python/FastAPI)
Painel web de administração. Permite gerenciar conjuntos de dados, mapeamentos e acompanhar status de importações através do navegador.

### Web App (Python/FastAPI)
Aplicação web para consulta e visualização de resultados. Exibe achados, relatórios e dashboards de forma acessível remotamente.

---

## Banco de Dados

O sistema utiliza **SQL Server** como banco de dados principal, com as seguintes categorias de tabelas:

- **Catálogos** — tabelas de referência (campos padrão, tipos de achado, tipos de entidade)
- **Fontes de dados** — registro de cada arquivo importado com metadados
- **Registros** — dados brutos importados em formato agnóstico (JSON por linha)
- **Layouts** — schema detectado de cada arquivo importado
- **Mapeamentos** — vínculo entre campos padrão e colunas reais
- **Catálogos mestres** — entidades consolidadas (pessoas, empresas, órgãos)
- **Achados** — inconsistências e irregularidades detectadas pelas análises
- **Conjuntos** — agrupamentos lógicos de fontes de dados
- **Logs** — registro de operações executadas

---

## Fluxo de Dados

```
Fontes Externas ──► Download/Upload ──► CSVs Brutos
                                              │
                                              ▼
                                       Auto-detecção
                                    (encoding, separador)
                                              │
                                              ▼
                                      Importação SQL Server
                                    (formato agnóstico JSON)
                                              │
                                              ▼
                                    Consolidação de Entidades
                                   (catálogos mestres de pessoas
                                    e empresas por CPF/CNPJ)
                                              │
                                              ▼
                                       Análises e Cruzamentos
                                    (20+ tipos de analisadores)
                                              │
                                              ▼
                                        Achados no Banco
                                   (classificados por severidade)
                                              │
                                              ▼
                                      Relatório de Auditoria
                                         (Markdown/PDF)
```

---

[Voltar à página inicial](./README.md)
