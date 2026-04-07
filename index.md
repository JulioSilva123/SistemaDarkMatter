# CiênciaDados

> **Motor de investigação automatizada de dados públicos para detecção de fraudes, conflitos de interesses e irregularidades na administração pública brasileira.**

---

## Visão Geral

O **CiênciaDados** é um sistema que importa, cruza e analisa dados abertos do governo brasileiro para detectar padrões suspeitos, redes de influência, fraudes em licitações e crimes contra o patrimônio público.

Opera com mais de 20 analisadores especializados que varrem bilhões de combinações entre dados de parlamentares, empresas, sócios, sanções e contratos, gerando achados classificados por severidade de 1 a 5.

---

## O que faz

### Rastreia recursos públicos
Mapeia para onde vai o dinheiro de cada deputado, senador, governador e prefeito. Identifica fornecedores que recebem de múltiplos agentes públicos simultaneamente. Detecta fragmentação de despesas para evasão de licitação. Flagra sazonalidade eleitoral em gastos públicos.

### Detecta conflitos de interesses
Descobre se políticos contratam empresas onde são sócios ou onde familiares são sócios. Identifica se parlamentares votam a favor de leis que beneficiam seus próprios fornecedores. Detecta nepotismo direto e cruzado. Flagra porta giratória entre reguladores e setor regulado.

### Mapeia redes ocultas
Conecta entidades por co-ocorrência de CPFs e CNPJs. Detecta empresas de fachada por endereço, telefone ou domínio web compartilhado. Identifica operadores centrais que ligam múltiplas redes de influência. Constrói grafos de relacionamentos com busca por graus de separação.

### Investiga fraudes em licitações
Detecta cartel por revezamento. Identifica obras fantasmas, inacabadas ou superfaturadas. Flagra aditivos abusivos que multiplicam o valor original. Encontra empresas com padrão suspeito de vitórias.

### Detecta insider trading
Cruza nomes de políticos com registros de negociação de ações da CVM. Identifica parlamentares que negociam ações de empresas impactadas por legislação em tramitação.

### Cruza bases de dados
Encontra entidades sancionadas que continuam recebendo benefícios públicos. Detecta inconsistências temporais em sanções e contratos.

### Consolida entidades
Reúne todas as pessoas, empresas e órgãos públicos a partir de dados da Receita Federal. Classifica automaticamente por tipo. Enriquece com dados OSINT.

---

## Componentes do Sistema

| Componente | Descrição |
|---|---|
| **Console CLI** | Interface de linha de comando para importação, análise e geração de relatórios |
| **Console UI** | Interface gráfica desktop para operação visual |
| **Web Admin** | Painel web de administração do sistema |
| **Web App** | Aplicação web para consulta e visualização de resultados |

---

## Páginas da Wiki

### Sobre o Sistema

| Página | Conteúdo |
|---|---|
| [Arquitetura do Sistema](./arquitetura-do-sistema.md) | Visão macro, camadas e componentes |
| [Modelo de Dados](./modelo-de-dados.md) | Formato agnóstico, catálogos e entidades |
| [Pipeline de Dados](./pipeline-de-dados.md) | ETL: extração, transformação, carga e consolidação |
| [Motor de Análise](./motor-de-analise.md) | Como funcionam os 20+ analisadores |
| [Sistema de Severidade](./sistema-de-severidade.md) | Classificação de achados por gravidade |
| [Integrações com APIs Governamentais](./integracoes-com-apis-governamentais.md) | Fontes externas e formatos de dados |
| [Padrões de Projeto](./padroes-de-projeto.md) | Decisões arquiteturais e princípios de design |
| [Segurança e Boas Práticas](./seguranca-e-boas-praticas.md) | Proteção de dados e cuidados de operação |

### Uso e Operação

| Página | Conteúdo |
|---|---|
| [Perguntas que o Sistema Responde](./perguntas-que-o-sistema-responde.md) | Lista completa de perguntas investigativas |
| [Datasets](./datasets.md) | Fontes de dados utilizadas |
| [Fluxos de Investigação](./fluxos-de-investigacao.md) | Do dado bruto ao relatório |
| [Fluxos Visuais](./fluxos-visuais.md) | Diagramas visuais dos fluxos de análise |
| [Guia de Comandos CLI](./guia-de-comandos-cli.md) | Referência completa de todos os comandos |
| [Análises Disponíveis](./analises-disponiveis.md) | Tipos de análise e mineração de dados |
| [Relatórios](./relatorios.md) | Geração de relatórios de auditoria |
| [Configuração](./configuracao.md) | Setup e configuração do ambiente |
| [Fluxo de Uso](./fluxo-de-uso.md) | Guia passo a passo para novos usuários |

---

## Aviso Legal

Este sistema é uma ferramenta de análise de dados abertos e detecção de padrões. Os achados gerados são **indícios** que devem ser investigados e validados antes de qualquer ação legal. O sistema não substitui investigação humana, auditoria formal ou processo judicial.
