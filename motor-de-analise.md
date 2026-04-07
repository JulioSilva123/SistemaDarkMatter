# Motor de Análise

Como o CiênciaDados detecta irregularidades, padrões suspeitos e inconsistências nos dados.

---

## Filosofia do Motor

O motor de análise opera sobre **dados já consolidados** no banco. Cada analisador é independente, focado em um tipo específico de irregularidade, e gera achados padronizados com severidade classificada.

Os analisadores utilizam os **mapeamentos de campos** para funcionar com qualquer fonte de dados, sem depender de nomes específicos de colunas.

---

## Categorias de Análise

### 1. Análises de Qualidade de Dados

Validam a integridade e consistência interna dos dados importados.

**Perfil Estatístico**
Gera estatísticas descritivas de cada coluna: tipo, taxa de preenchimento, nulos, únicos, distribuição por categorias. É o ponto de partida para entender qualquer base de dados.

**Duplicidades**
Detecta registros duplicados por hash completo (linhas 100% idênticas) e por campo-chave (mesmo CPF/CNPJ aparecendo múltiplas vezes).

**Validação de Documentos**
Verifica os dígitos verificadores de CPFs e CNPJs usando os algoritmos oficiais. Documentos com dígitos inválidos indicam possível fraude ou erro de digitação.

**Outliers**
Identifica valores numéricos fora do padrão usando o método IQR (Interquartile Range). Configurações moderadas (fator 1.5) ou extremas (fator 3.0).

**Análise de Texto**
Detecta blocos de texto longos duplicados (copy-paste). Útil para encontrar fundamentações legais idênticas, observações copiadas em contratos ou justificativas repetidas.

---

### 2. Análises de Consistência

Verificam contradições entre dados dentro da mesma base ou entre bases relacionadas.

**Análise Temporal**
Examina datas para encontrar: sanções vencidas que ainda aparecem como ativas, registros sem data final, datas inconsistentes (final antes do início), e picos temporais suspeitos.

**Cruzamento de Bases**
Cruza duas fontes por CPF/CNPJ (match exato) e/ou por nome (fuzzy matching). Configurações de tolerância ajustáveis para encontrar variações de grafia.

**Consistência Restritivo vs Benefício**
Detecta entidades que estão simultaneamente em uma base restritiva (CEIS, CNEP — empresas punidas) e em uma base de benefícios (Auxílio Brasil, CEPIM). Severidade **4 (crítica)**: entidade sancionada recebendo recurso público.

---

### 3. Análises de Fraude

Detectam padrões que indicam irregularidades intencionais.

**Fragmentação de Despesas (Fuga de Licitação)**
Baseado na Nova Lei de Licitações, detecta quando a mesma entidade (CPF/CNPJ) recebe múltiplos pagamentos fracionados abaixo do teto de dispensa, cujo acumulado numa janela rolante de 90 dias ultrapassa o limite legal.

**Sazonalidade Eleitoral**
Detecta picos atípicos de gastos públicos às vésperas de eleições. Compara anos eleitorais (pares) com a média dos anos não-eleitorais (ímpares) para cada fornecedor.

**Insider Trading Político**
Cruza parlamentares e candidatos com registros de negociação de valores mobiliários na CVM (Resolução CVM 44). Verifica se operações coincidem com tramitação de leis que impactam as empresas negociadas.

**Parlamentar Sancionado**
Cruza nomes de deputados e senadores com bases de sanções (CEIS/CNEP) usando fuzzy matching. Identifica se um político aparece como empresa ou pessoa sancionada.

**Fornecedor Sancionado**
Verifica se os fornecedores das despesas de cota parlamentar (CEAP/CEAPS) estão em listas de impedidos (CEIS, CEPIM). Detecta quando dinheiro público vai para empresas punidas.

**Rede de Influência**
Constrói grafos de co-ocorrência de CPFs e CNPJs para detectar: cartéis por revezamento em licitações, empresas de fachada (mesmo endereço/telefone/domínio), operadores centrais de redes de laranjas.

---

## Técnicas Utilizadas

### Fuzzy Matching
Algoritmo de similaridade de strings (Levenshtein) para encontrar nomes parecidos mesmo com variações de grafia, abreviações ou erros de digitação. Tolerância configurável (padrão: 85%).

### Índice HHI (Herfindahl-Hirschman)
Mede concentração de mercado. Aplicado para detectar quando um deputado concentra gastos em poucos fornecedores (HHI > 2500 = alta concentração).

### IQR (Interquartile Range)
Método estatístico robusto para detecção de outliers. Valores fora de [Q1 - 1.5×IQR, Q3 + 1.5×IQR] são considerados anômalos.

### Janela Rolante (Rolling Window)
Usada na análise de fragmentação para somar valores acumulados em períodos deslizantes de N dias, detectando padrões que não são visíveis em análises estáticas.

### Grafos de Co-ocorrência
Entidades que aparecem juntas nos mesmos registros formam arestas em um grafo. Clusters densos indicam redes de relacionamento suspeito.

---

## Resultado das Análises

Cada análise gera **achados** no banco de dados com:
- Tipo da irregularidade detectada
- Severidade (1 a 5)
- Descrição detalhada do que foi encontrado
- Evidências (dados concretos que suportam a conclusão)
- Entidades envolvidas (CPF/CNPJ/nome)
- Fonte de dados de origem
- Data e hora da detecção

---

[Voltar à página inicial](./index.md) | [Ver lista completa](./analises-disponiveis.md)
