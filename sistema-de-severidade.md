# Sistema de Severidade

Como os achados são classificados por gravidade no CiênciaDados.

---

## Escala de Severidade

Cada achado gerado pelo motor de análise recebe uma classificação de 1 a 5, indicando o nível de urgência e gravidade.

| Severidade | Nível | Cor | Ação Recomendada |
|---|---|---|---|
| **5** | Máxima | 🔴 | Crime evidente — ação imediata |
| **4** | Crítica | 🔴 | Forte indício de irregularidade grave |
| **3** | Alta | 🟡 | Padrão suspeito — requer investigação |
| **2** | Média | 🟢 | Anomalia — merece atenção |
| **1** | Baixa | 🔵 | Inconsistência leve ou informativa |

---

## Critérios de Classificação

### Severidade 5 — Máxima
Situações que configuram crime evidente ou violação grave da lei.

**Exemplos:**
- Político negociando ações de empresa impactada por legislação que votou (insider trading confirmado)
- Entidade com sanção ativa recebendo contratos milionários do governo
- Rede organizada de laranjas com documentos fraudulentos

### Severidade 4 — Crítica
Forte indício de irregularidade que requer apuração formal.

**Exemplos:**
- Entidade sancionada no CEIS recebendo qualquer benefício público
- Fornecedor de cota parlamentar em lista de impedidos (CEIS/CEPIM)
- Parlamentar aparecendo como sancionado em base do governo
- Fragmentação de despesas comprovada acima do teto legal

### Severidade 3 — Alta
Padrão suspeito que justifica investigação mais aprofundada.

**Exemplos:**
- Sazonalidade eleitoral significativa em gastos públicos
- Concentração alta de gastos em poucos fornecedores (HHI > 2500)
- Fornecedor compartilhando endereço com empresa de familiar de parlamentar
- Cartel por revezamento em licitações

### Severidade 2 — Média
Anomalia que merece atenção mas pode ter explicação legítima.

**Exemplos:**
- Sobrenome de familiar coincidindo com fornecedor (sem endereço compartilhado)
- Outliers moderados em valores de contratos
- Datas inconsistentes que podem ser erro de digitação

### Severidade 1 — Baixa
Inconsistência leve ou informação contextual para investigação futura.

**Exemplos:**
- Documentos com dígitos verificadores inválidos (pode ser erro de digitação)
- Registros duplicados que podem ser legítimos
- Textos longos similares que podem ser modelos padrão

---

## Uso nos Relatórios

Os relatórios de auditoria permitem filtrar por severidade mínima:

- **Severidade 3+** — recomendado para relatórios de auditoria formal
- **Severidade 4+** — para alertas executivos e denúncias
- **Severidade 5** — para ações imediatas e comunicados urgentes

---

## Composição de Severidade

Alguns achados têm severidade composta por múltiplos fatores. Por exemplo, na detecção de familiares recebendo recursos:

- **Severidade 4**: sobrenome + mesmo endereço + pagamentos confirmados
- **Severidade 3**: sobrenome + co-ocorrência na mesma UF/município
- **Severidade 2**: apenas coincidência de sobrenome

---

[Voltar à página inicial](./index.md) | [Ver motor de análise](./motor-de-analise.md)
