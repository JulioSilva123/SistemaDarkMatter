# jft.cienciadados.consolecli

> **Motor de investigação automatizada de dados públicos para detecção de fraudes, conflitos de interesses e irregularidades na administração pública brasileira.**

---

## O que é

Um sistema que importa, cruza e analisa dados abertos do governo brasileiro para detectar padrões suspeitos, redes de influência, fraudes em licitações e crimes contra o patrimônio público.

Opera com mais de 20 analisadores especializados que varrem bilhões de combinações entre dados de parlamentares, empresas, sócios, sanções e contratos, gerando achados classificados por severidade de 1 a 5.

---

## O que faz

**Rastreia recursos públicos** — mapeia para onde vai o dinheiro de cada deputado, senador, governador e prefeito. Identifica fornecedores que recebem de múltiplos agentes públicos simultaneamente. Detecta fragmentação de despesas para evasão de licitação. Flagra sazonalidade eleitoral em gastos públicos.

**Detecta conflitos de interesses** — descobre se políticos contratam empresas onde são sócios ou onde familiares são sócios. Identifica se parlamentares votam a favor de leis que beneficiam seus próprios fornecedores. Detecta nepotismo direto e cruzado. Flagra porta giratória entre reguladores e setor regulado.

**Mapeia redes ocultas** — conecta entidades por co-ocorrência de CPFs e CNPJs. Detecta empresas de fachada por endereço, telefone ou domínio web compartilhado. Identifica operadores centrais que ligam múltiplas redes de influência. Constrói grafos de relacionamentos com busca por graus de separação.

**Investiga fraudes em licitações** — detecta cartel por revezamento. Identifica obras fantasmas, inacabadas ou superfaturadas. Flagra aditivos abusivos que multiplicam o valor original. Encontra empresas com padrão suspeito de vitórias.

**Detecta insider trading** — cruza nomes de políticos com registros de negociação de ações da CVM. Identifica parlamentares que negociam ações de empresas impactadas por legislação em tramitação.

**Cruza bases de dados** — encontra entidades sancionadas que continuam recebendo benefícios públicos. Detecta inconsistências temporais em sanções e contratos.

**Consolida entidades** — reúne todas as pessoas, empresas e órgãos públicos a partir de dados da Receita Federal. Classifica automaticamente por tipo. Enriquece com dados OSINT.

---

## Perguntas que o sistema responde

- Para onde vai o dinheiro da cota parlamentar de cada deputado?
- O deputado é sócio de alguma empresa que recebe recursos dele?
- Familiares do deputado recebem recursos públicos dele?
- O deputado vota a favor de leis que beneficiam seus fornecedores?
- Existem fornecedores que recebem de múltiplos deputados do mesmo partido?
- O deputado fragmenta despesas para evitar licitação?
- O senador tem rede de influência com governadores e prefeitos?
- O governador tem empresas em seu nome ou de familiares?
- O juiz tem decisões que beneficiam empresas de aliados?
- A empresa que venceu a licitação é fachada ou laranja de político?
- Existem ciclos de dinheiro onde o recurso volta ao parlamentar?
- Quem é o operador central que conecta múltiplas redes de influência?
- Existem empresas de fachada que compartilham endereço, telefone ou domínio web?
- O político tem padrão de gastos anômalo comparado à média da casa?
- Existem obras públicas que receberam pagamento mas não existem fisicamente?
- Empresas sancionadas no CEIS continuam recebendo contratos públicos?
- Políticos aparecem como negociantes de ações na CVM?

---

## Datasets utilizados

**Portal da Transparência** — CEIS (empresas inidôneas), CNEP (empresas punidas pela Lei Anticorrupção), CEAF (servidores expulsos), CEPIM (entidades impedidas), Bolsa Família, Servidores Federais, Licitações, Despesas Públicas e Cartão Corporativo do Governo.

**Câmara dos Deputados** — dados cadastrais de 513 deputados, todas as despesas de cota parlamentar, proposições legislativas (PLs, PECs, MPs), votações nominais, estrutura de comissões e frentes parlamentares.

**Senado Federal** — dados cadastrais de 81 senadores, despesas CEAPS, proposições, comissões e votações.

**Receita Federal** — cadastro completo de todas as empresas do Brasil (razão social, capital social, situação cadastral), dados de todos os estabelecimentos (endereço, atividade econômica) e quadro societário completo (sócios, qualificações, faixa etária).

**TSE** — prestação de contas de campanhas eleitorais e cadastro de candidatos.

**CVM** — registros de insider trading e dados de companhias abertas.

---

## Fluxos de investigação

**Do dado bruto ao relatório** — o sistema baixa dados de portais governamentais, importa automaticamente com detecção de encoding e separador, consolida CPFs e CNPJs em catálogos mestres, executa todos os analisadores e gera um relatório de auditoria em Markdown com achados agrupados por severidade.

**Investigação parlamentar completa** — coleta dados de deputados e senadores, importa despesas de gabinete de todos os 513 deputados, cruza com listas de sanção (CEIS/CNEP), detecta fornecedores irregulares, analisa fragmentação de despesas, mapeia redes de influência e gera relatório focado em conflitos de interesse.

**Detecção de redes de laranjas** — extrai todos os CPFs e CNPJs dos dados importados, constrói um grafo de co-ocorrência (entidades que aparecem juntas nos mesmos registros), detecta clusters por dados compartilhados (email, telefone, endereço), cruza com domínios web via OSINT e identifica operadores centrais da rede.

**Auditoria de licitações** — importa dados de contratos e licitações, valida CNPJs dos fornecedores, detecta empresas fantasmas ou inativas, identifica cartel por revezamento de vitórias, flagra aditivos abusivos e fragmentação de despesas, cruza com listas de sanção para encontrar fornecedores impedidos.

**Insider trading político** — importa dados de parlamentares e registros de negociação da CVM, cruza nomes por fuzzy matching, verifica se negociações coincidem com votações de leis relevantes e gera achados de severidade máxima quando detecta político negociando ações suspeitas.

---

## Fluxo visual: Para onde vai o dinheiro do deputado?

```
┌─────────────────────────────────────────────────────────────────────┐
│                     FLUXO: DESTINO DA COTA PARLAMENTAR             │
└─────────────────────────────────────────────────────────────────────┘

  ┌──────────────────┐
  │   🔍 DEPUTADO    │  Nome, CPF, Partido, UF
  │  ID: 12345       │  Câmara dos Deputados
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  📊 DESPESAS PARLAMENTARES (base completa)               │
  │  ├─ Agrupar por fornecedor (CNPJ / CPF)                  │
  │  ├─ Somar valores por fornecedor                         │
  │  ├─ Contar nº de pagamentos                              │
  │  └─ Primeiro e último pagamento                          │
  └────────┬─────────────────────────────────────────────────┘
           │
           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  🏢 IDENTIFICAR QUEM RECEBEU                            │
  │                                                          │
  │  CNPJ ──────────────────────┐  CPF ──────────────────┐  │
  │  ▼                          │  ▼                     │  │
  │  ┌──────────────────┐       │  ┌──────────────────┐  │  │
  │  │ EMPRESAS (RFB)   │       │  │ PESSOAS          │  │  │
  │  │ ├─ Razão social  │       │  │ ├─ Nome          │  │  │
  │  │ ├─ Situação      │       │  │ ├─ Nascimento    │  │  │
  │  │ ├─ Endereço      │       │  │ └─ Nome da mãe   │  │  │
  │  │ └─ Capital social│       │  └──────────────────┘  │  │
  │  └────────┬─────────┘       └──────────┬─────────────┘  │
  │           │                            │                │
  │           ▼                            ▼                │
  │  ┌──────────────────┐       ┌──────────────────┐        │
  │  │ SÓCIOS (RFB)     │       │                  │        │
  │  │ ├─ CPF do sócio  │       │                  │        │
  │  │ ├─ Qualificação  │       │                  │        │
  │  │ └─ Faixa etária  │       │                  │        │
  │  └──────────────────┘       └──────────────────┘        │
  └────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  📈 ANÁLISE                                              │
  │                                                          │
  │  🏷️  Classificar por setor econômico                    │
  │     (consultoria, transporte, gráfica, tecnologia...)    │
  │                                                          │
  │  📊 Calcular concentração (HHI)                          │
  │     HHI > 2500 → ⚠️ Alta concentração                   │
  │                                                          │
  │  📅 Evolução temporal                                    │
  │     Picos? Mudança de padrão? Sazonalidade?              │
  └────────┬─────────────────────────────────────────────────┘
           │
           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  🎯 RESULTADO                                            │
  │                                                          │
  │  🏆 Top 10 fornecedores:                                 │
  │  ┌────────────────┬──────────┬────────┬─────────┐        │
  │  │ Fornecedor     │ Setor    │ Total  │ Pgto    │        │
  │  ├────────────────┼──────────┼────────┼─────────┤        │
  │  │ Empresa Alpha  │ Consult. │ R$ 50K │ 12      │        │
  │  │ Empresa Beta   │ Transp.  │ R$ 35K │ 8       │        │
  │  │ Empresa Gamma  │ Gráfica  │ R$ 28K │ 15      │        │
  │  └────────────────┴──────────┴────────┴─────────┘        │
  │                                                          │
  │  ⚠️  Achados gerados: severidade 2 a 4                  │
  └──────────────────────────────────────────────────────────┘
```

---

## Fluxo visual: Familiares do deputado recebem recursos públicos?

```
┌─────────────────────────────────────────────────────────────────────┐
│              FLUXO: FAMÍLIA DO DEPUTADO E RECURSOS PÚBLICOS        │
└─────────────────────────────────────────────────────────────────────┘

  ┌──────────────────┐
  │   🔍 DEPUTADO    │  Nome completo + CPF
  │  "João Silva"    │  Câmara dos Deputados
  └────────┬─────────┘
           │
           ├────────────────────────────────────────────┐
           │                                            │
           ▼                                            ▼
  ┌────────────────────┐                     ┌────────────────────┐
  │  👨‍👩‍👧 FAMILIARES   │                     │  💰 DESPESAS       │
  │                    │                     │                    │
  │  Extrair sobrenomes│                     │  Buscar todos os   │
  │  "Silva", "Santos" │                     │  fornecedores      │
  │                    │                     │  (CNPJ/CPF)        │
  │  Buscar na base de │                     │                    │
  │  PESSOAS:          │                     │  Lista de quem     │
  │  ├─ Maria Silva    │                     │  recebeu dinheiro  │
  │  ├─ Carlos Santos  │                     │  do deputado       │
  │  └─ Ana Silva      │                     │                    │
  │                    │                     └─────────┬──────────┘
  │  Score parentesco: │                               │
  │  🟢 Alto: mesmo    │                               │
  │     endereço       │                               │
  │  🟡 Médio: mesma   │                               │
  │     UF/município   │                               │
  │  🔴 Baixo: apenas  │                               │
  │     sobrenome      │                               │
  └────────┬───────────┘                               │
           │                                           │
           ▼                                           │
  ┌────────────────────┐                               │
  │  🏢 EMPRESAS DOS   │                               │
  │     FAMILIARES     │                               │
  │                    │                               │
  │  Buscar em SÓCIOS  │                               │
  │  (Receita Federal) │                               │
  │                    │                               │
  │  Maria Silva ──►   │                               │
  │    ├─ Empresa A    │                               │
  │    └─ Empresa B    │                               │
  │                    │                               │
  │  Carlos Santos ──► │                               │
  │    └─ Empresa C    │                               │
  │                    │                               │
  │  Ana Silva ──►     │                               │
  │    └─ Empresa D    │                               │
  └────────┬───────────┘                               │
           │                                           │
           └───────────────┬───────────────────────────┘
                           │
                           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  🔗 CRUZAMENTO                                           │
  │                                                          │
  │  Empresas dos familiares ∩ Fornecedores do deputado      │
  │                                                          │
  │  Empresa A (Maria Silva) ◄──► Empresa A (fornecedor)     │
  │  ✅ MATCH!                                               │
  │                                                          │
  │  Empresa C (Carlos Santos) ◄──► Empresa C (fornecedor)   │
  │  ✅ MATCH!                                               │
  │                                                          │
  │  Empresa B ──► ❌ Sem match                              │
  │  Empresa D ──► ❌ Sem match                              │
  └────────┬─────────────────────────────────────────────────┘
           │
           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  🔎 VALIDAÇÃO ADICIONAL                                  │
  │                                                          │
  │  📛 Fuzzy matching: nome do fornecedor vs nome familiar  │
  │     Score > 85% → possível match adicional               │
  │                                                          │
  │  📍 Endereço: mesmo endereço ou mesmo bairro?            │
  │     ✅ Mesmo endereço → score sobe                       │
  │     ⚠️ Mesmo bairro → score moderado                     │
  │                                                          │
  │  🌐 Domínio web: compartilham domínio?                   │
  │     ✅ Mesmo domínio → ⚠️ possível fachada               │
  └────────┬─────────────────────────────────────────────────┘
           │
           ▼
  ┌──────────────────────────────────────────────────────────┐
  │  🎯 RESULTADO                                            │
  │                                                          │
  │  ⚠️  ACHADO: Familiar recebe recursos do deputado         │
  │                                                          │
  │  ┌──────────────┬────────────┬────────┬─────────┬──────┐ │
  │  │ Familiar     │ Empresa    │ Score  │ Total   │ Sev  │ │
  │  ├──────────────┼────────────┼────────┼─────────┼──────┤ │
  │  │ Maria Silva  │ Empresa A  │ 🟢 Alto│ R$ 45K  │  🔴4 │ │
  │  │ Carlos Santos│ Empresa C  │ 🟡 Médio│ R$ 12K  │  🟡3 │ │
  │  └──────────────┴────────────┴────────┴─────────┴──────┘ │
  │                                                          │
  │  Severidade:                                             │
  │  🔴 4 = sobrenome + endereço + pagamentos               │
  │  🟡 3 = sobrenome + co-ocorrência                       │
  │  🟢 2 = apenas sobrenome                                │
  └──────────────────────────────────────────────────────────┘
```

---

## Aviso Legal

Este sistema é uma ferramenta de análise de dados abertos e detecção de padrões. Os achados gerados são **indícios** que devem ser investigados e validados antes de qualquer ação legal. O sistema não substitui investigação humana, auditoria formal ou processo judicial.
