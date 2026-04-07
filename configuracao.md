# Configuração

Guia de setup e configuração do ambiente do CiênciaDados.

---

## Pré-requisitos

- **SQL Server** (2017 ou superior)
- **.NET SDK** (versão compatível com o projeto)
- **ODBC Driver 17 for SQL Server** (ou superior)
- **Conexão com internet** para download de dados das APIs governamentais

---

## Arquivo de Configuração

O sistema usa um arquivo `.env` na raiz do projeto:

```env
DB_SERVER=localhost
DB_NAME=db_ciencia_dados
DB_DRIVER=ODBC Driver 17 for SQL Server
```

Conexão padrão: **Trusted Connection** (autenticação Windows).

---

## Estrutura de Diretórios

```
CienciaDados/
├── dados/
│   ├── brutos/          # Arquivos CSV baixados aguardando importação
│   └── processados/     # Arquivos já importados (movidos automaticamente)
├── db_ciencia_dados/    # Scripts de banco de dados
├── jft.cienciadados.consolecli/   # Interface de linha de comando
├── jft.cienciadados.consoleui/    # Interface gráfica desktop
├── jft.cienciadados.webadmin/     # Painel web de administração
└── jft.cienciadados.webapp/       # Aplicação web
```

---

## Componentes do Sistema

| Componente | Descrição |
|---|---|
| **Console CLI** | Interface de linha de comando para importação, análise e geração de relatórios |
| **Console UI** | Interface gráfica desktop para operação visual |
| **Web Admin** | Painel web de administração do sistema |
| **Web App** | Aplicação web para consulta e visualização de resultados |

---

## Primeiro Acesso

1. Configure o arquivo `.env` com as credenciais do SQL Server
2. Execute `cienciadados testar-conexao` para validar a conexão
3. Crie seu primeiro conjunto de dados com `cienciadados conjunto-criar`
4. Baixe dados com os comandos de ingestão ou importe CSVs manualmente

---

[Voltar à página inicial](./index.md)
