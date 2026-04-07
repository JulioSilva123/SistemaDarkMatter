# Padrões de Projeto e Decisões Arquiteturais

Princípios e decisões que guiaram o design do CiênciaDados.

---

## Decisões Arquiteturais

### Por que Formato Agnóstico?

**Problema:** Cada fonte de dados governamental tem um schema diferente. Criar tabelas específicas para cada CSV tornaria o sistema rígido e difícil de manter.

**Solução:** Armazenar dados brutos como JSON em uma tabela genérica de registros, com metadados de schema em uma tabela separada de layouts. Isso permite importar qualquer CSV sem modificar o banco.

**Trade-off:** Consultas são mais complexas (requer parsing de JSON), mas a flexibilidade compensa para um sistema que lida com fontes de dados em constante mudança.

### Por que SQL Server?

- **Trusted Connection** — autenticação Windows integrada, sem necessidade de gerenciar credenciais no código
- **JSON nativo** — suporte a consultas JSON dentro do SQL Server
- **Performance** — índices e otimizações para grandes volumes de dados
- **Integração corporativa** — compatível com ambientes governamentais e empresariais

### Por que Python no CLI?

- **Ecossistema de dados** — Pandas, NumPy e bibliotecas de análise são nativas do Python
- **Fuzzy matching** — bibliotecas maduras para similaridade de strings
- **Auto-detecção** — chardet e outras libs para detecção automática de encoding
- **Produtividade** — desenvolvimento rápido de scripts de ETL

---

## Padrões de Projeto

### Repository Pattern

O acesso a dados é abstraído por repositórios. Os serviços de aplicação não sabem se os dados vêm de SQL Server, arquivo ou API — apenas solicitam dados através de interfaces.

### Strategy Pattern

Cada tipo de análise é uma estratégia independente. Novos analisadores podem ser adicionados sem modificar os existentes, seguindo o **Open/Closed Principle**.

### Factory Pattern

A criação de entidades (pessoas, empresas, achados) segue um padrão de fábrica que garante consistência e validação no momento da criação.

### Observer Pattern

O sistema de logs observa todas as operações executadas e registra automaticamente comandos, duração e resultados.

---

## Princípios de Design

### Convention over Configuration

O sistema auto-detecta encoding, separador e nome da fonte. O usuário só precisa indicar o caminho do arquivo — o resto é automático.

### Fail-Safe

Importações interrompidas podem ser retomadas. O sistema detecta o ponto de parada e continua de onde parou, sem duplicar registros.

### Single Source of Truth

Os catálogos mestres de pessoas e empresas são a fonte definitiva de entidades. Todas as análises consultam os catálogos, não os dados brutos.

### Defense in Depth

Múltiplas camadas de validação: documentos são validados antes da importação, mapeamentos são verificados antes das análises, e achados são classificados por severidade antes da geração de relatórios.

---

## Organização em Camadas

```
src/
├── core/              # Regras de negócio puras (sem dependências externas)
│   ├── entidades/     # Modelos de domínio (Pessoa, Empresa, Achado)
│   ├── enums/         # Tipificações (TipoAchado, TipoEntidade, Severidade)
│   ├── interfaces/    # Contratos de repositórios e serviços
│   └── mapeamentos/   # Campos padrão e definições
│
├── aplicativo/        # Serviços de aplicação (orquestração)
│   └── servicos/      # Ingestão, importação, análise, relatórios
│
├── infra/             # Detalhes de implementação
│   ├── banco/         # Conexão SQL Server, repositórios
│   └── leitores/      # Leitores de CSV, detecção de encoding
│
└── interface/         # Camada de apresentação
    └── cli/           # Comandos Click, formatação Rich
```

---

## Mapeamento de Campos: A Chave da Flexibilidade

O sistema opera com **campos padrão** semânticos (`cpf_cnpj`, `nome`, `data_inicio`) que são mapeados para **colunas reais** em cada fonte de dados. Isso permite que:

- O mesmo analisador funcione com CEIS, CNEP ou qualquer outra base
- Novas fontes sejam adicionadas apenas configurando mapeamentos
- Análises cruzadas funcionem mesmo com schemas completamente diferentes

Exemplo:
- CEIS: coluna "CPF OU CNPJ DO SANCIONADO" → campo `cpf_cnpj`
- Despesas Câmara: coluna "cnpjCpfFornecedor" → campo `cpf_cnpj`
- Ambos são analisados pelo mesmo validador de documentos

---

[Voltar à página inicial](./README.md) | [Ver arquitetura](./arquitetura-do-sistema.md)
