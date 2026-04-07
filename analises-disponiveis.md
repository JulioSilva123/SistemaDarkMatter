# Análises Disponíveis

O CiênciaDados oferece mais de 20 tipos de análise para detecção de irregularidades.

---

## Análises Básicas

### Perfil Estatístico
Gera um perfil completo de cada coluna: tipo de dado, taxa de preenchimento, nulos, valores únicos e distribuição por UF/categoria/esfera.

### Duplicidades
Detecta registros duplicados por hash (100% iguais) e por campo-chave (CPF/CNPJ).

### Validação de Documentos
Valida dígitos verificadores de CPFs e CNPJs. Detecta documentos inválidos ou fraudados.

### Análise Temporal
Analisa datas: sanções vencidas, ativas, sem data final, datas inconsistentes e picos temporais.

### Cruzamento de Bases
Cruza duas bases por CPF/CNPJ (exato) e/ou Nome (fuzzy matching).

### Outliers
Detecta valores numéricos fora do padrão usando IQR (Interquartile Range).

### Análise de Texto
Detecta textos longos duplicados (copy-paste). Útil para encontrar fundamentações legais idênticas, observações copiadas, etc.

---

## Análises de Fraude

### Consistência (Sancionado Recebendo Benefício)
Detecta entidades presentes simultaneamente em base restritiva (CEIS, CNEP) e base de benefícios (Auxílio Brasil, CEPIM). Severidade **4 (crítica)**.

### Fragmentação de Despesas
Detector rigoroso de Fuga de Licitação. Baseado na Nova Lei de Licitações, varre contratos ou tabelas financeiras procurando a mesma entidade (CPF/CNPJ) faturando valores fracionados abaixo da margem de lei, cujo montante acumulado numa janela rolante de 90 dias fure o teto sem concorrência pública.

### Sazonalidade Eleitoral
Avalia arquivos com valores declarados e datas e detecta picos atípicos de gastos ou repasses ocorridos às vésperas de campanhas políticas (compara Anos Pares com a média dos anos comuns/ímpares).

### Insider Trading Político
Cruza políticos eleitos ou candidatos com relatórios de negociações de valores mobiliários na B3 (Res. CVM 44). Inferência de *Insider Trading* comparando datas de operações contra tramitações legais.

### Parlamentar Sancionado
Cruza nomes de Deputados Federais ou Senadores diretamente com as bases de sanções do governo (CEIS ou CNEP), utilizando algoritmo de *Fuzzy Match*.

### Fornecedor Sancionado
Mapeia se os gastos de Despesas de Gabinete (cota parlamentar) foram destinados a empresas ou entidades oficialmente impedidas/punidas. Cruza a Nota Fiscal do deputado com CEIS e CEPIM.

### Rede de Influência
Monitor de cartéis e laranjas. Constrói grafos de co-ocorrência de CPFs e CNPJs, detecta clusters por dados compartilhados e identifica operadores centrais.

---

## Classificação de Severidade

| Severidade | Nível | Descrição |
|---|---|---|
| 5 | Máxima | Crime evidente, ação imediata |
| 4 | Crítica | Forte indício de irregularidade grave |
| 3 | Alta | Padrão suspeito que requer investigação |
| 2 | Média | Anomalia que merece atenção |
| 1 | Baixa | Inconsistência leve ou informativa |

---

[Voltar à página inicial](./README.md)
