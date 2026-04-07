# Fluxos Visuais

Diagramas visuais que ilustram como o sistema rastreia e analisa o uso de recursos públicos.

---

## Fluxo: Para onde vai o dinheiro do deputado?

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

## Fluxo: Familiares do deputado recebem recursos públicos?

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

[Voltar à página inicial](./README.md)
