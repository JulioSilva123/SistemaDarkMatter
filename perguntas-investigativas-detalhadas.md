# Perguntas Investigativas Detalhadas

Cada pergunta abaixo representa uma capacidade de investigação do sistema. Para cada uma, explicamos **o que** o sistema faz e **como** chega à resposta.

---

## Cota Parlamentar

### Para onde vai o dinheiro da cota parlamentar de cada deputado?

**O que o sistema faz:** Rastreia cada centavo gasto com a Cota para Exercício da Atividade Parlamentar (CEAP) de um deputado específico, identificando todos os fornecedores, valores, datas e categorias de despesa.

**Como funciona:**

1. Coleta todas as despesas do deputado na base da Câmara dos Deputados
2. Agrupa por fornecedor (CNPJ ou CPF), somando valores e contando pagamentos
3. Identifica cada fornecedor nos catálogos mestres do sistema (Receita Federal)
4. Classifica por setor econômico (consultoria, transporte, gráfica, tecnologia, etc.)
5. Calcula indicadores de concentração (HHI) para detectar dependência de poucos fornecedores
6. Analisa evolução temporal para identificar picos, mudanças de padrão e sazonalidade

**Resultado:** Top 10 fornecedores do deputado com setor, total gasto, número de pagamentos e achados de severidade associados.

---

### O deputado fragmenta despesas para evitar licitação?

**O que o sistema faz:** Detecta quando um mesmo fornecedor recebe múltiplos pagamentos fracionados abaixo do teto de dispensa de licitação, cujo acumulado num período ultrapassa o limite legal.

**Como funciona:**

1. Varre todas as despesas agrupadas por fornecedor (CNPJ/CPF)
2. Ordena cronologicamente os pagamentos a cada fornecedor
3. Aplica uma janela rolante de 90 dias sobre os pagamentos
4. Soma os valores dentro de cada janela
5. Compara o acumulado com o teto legal de dispensa (R$ 59.900,00 pela Nova Lei de Licitações)
6. Quando o acumulado ultrapassa o teto, gera um achado de severidade **4 (crítica)**

**Resultado:** Lista de fornecedores que receberam valores fragmentados acima do teto legal, com datas, valores individuais e acumulado, e o deputado responsável.

---

### O deputado tem padrão de gastos anômalo comparado à média da casa?

**O que o sistema faz:** Compara o perfil de gastos de um deputado com a média de todos os 513 deputados, identificando desvios significativos.

**Como funciona:**

1. Calcula a média e mediana de gastos por deputado para cada categoria de despesa
2. Define faixas normais usando o método IQR (Interquartile Range)
3. Identifica deputados cujos gastos estão fora da faixa normal (outliers)
4. Analisa categorias específicas: passagens aéreas, hospedagem, consultorias, combustíveis
5. Detecta mudanças bruscas de padrão ao longo do tempo

**Resultado:** Deputados com gastos anômalos por categoria, com valor gasto vs. média da casa e fator de desvio.

---

### Existem fornecedores que recebem de múltiplos deputados do mesmo partido?

**O que o sistema faz:** Identifica fornecedores que recebem recursos de vários deputados de uma mesma legenda, o que pode indicar esquema centralizado de direcionamento.

**Como funciona:**

1. Agrupa todos os fornecedores por deputado e partido
2. Conta quantos deputados de cada partido pagam o mesmo fornecedor
3. Calcula o total recebido pelo fornecedor de cada partido
4. Sinaliza fornecedores com concentração alta em um único partido
5. Cruza com dados societários para verificar se há ligação entre os fornecedores

**Resultado:** Fornecedores com recebimentos de múltiplos deputados do mesmo partido, com lista de deputados, totais e severidade do achado.

---

## Conflitos de Interesse

### O deputado é sócio de alguma empresa que recebe recursos dele?

**O que o sistema faz:** Cruza o CPF do deputado com o quadro societário da Receita Federal para verificar se ele é sócio de empresas que aparecem como fornecedoras nas suas despesas parlamentares.

**Como funciona:**

1. Identifica o CPF do deputado nos dados da Câmara
2. Busca o CPF no quadro societário da Receita Federal
3. Lista todas as empresas onde o deputado é sócio
4. Cruza os CNPJs dessas empresas com os fornecedores das despesas do deputado
5. Quando encontra correspondência, gera achado de severidade **4 (crítica)**

**Resultado:** Empresas do deputado que receberam recursos da cota parlamentar, com valores, datas e número de pagamentos.

---

### Familiares do deputado recebem recursos públicos dele?

**O que o sistema faz:** Identifica se parentes do deputado (mesmo sobrenome + mesma UF) aparecem como pessoas físicas ou jurídicas recebendo recursos da cota parlamentar.

**Como funciona:**

1. Extrai sobrenomes incomuns do nome completo do deputado
2. Busca no catálogo de pessoas todos que compartilham esses sobrenomes e a mesma UF
3. Calcula score de parentesco: mesmo endereço (alto), mesmo município (médio), apenas sobrenome (baixo)
4. Identifica empresas onde essas pessoas são sócias
5. Cruza com fornecedores das despesas do deputado
6. Aplica validação adicional: fuzzy matching de nomes, verificação de endereço compartilhado, domínios web

**Resultado:** Familiares com recebimentos de recursos do deputado, com score de parentesco, valores totais e severidade composta.

---

### O deputado vota a favor de leis que beneficiam seus fornecedores?

**O que o sistema faz:** Cruza votações nominais do deputado com dados de fornecedores da sua cota parlamentar, identificando se ele votou a favor de proposições que impactam setores ou empresas específicas.

**Como funciona:**

1. Coleta todas as votações nominais do deputado na Câmara
2. Identifica os fornecedores mais relevantes das suas despesas
3. Classifica fornecedores por setor econômico e CNAE
4. Cruza com proposições legislativas que impactam esses setores
5. Verifica se o deputado votou a favor de proposições benéficas aos seus fornecedores

**Resultado:** Casos onde o deputado votou a favor de leis que beneficiam setores dos seus fornecedores, com detalhes da votação e do fornecedor.

---

### O governador tem empresas em seu nome ou de familiares?

**O que o sistema faz:** Busca no quadro societário da Receita Federal empresas vinculadas ao governador e seus familiares, cruzando com contratos públicos do estado.

**Como funciona:**

1. Identifica CPF e nome completo do governador
2. Busca no quadro societário todas as empresas vinculadas
3. Extrai sobrenomes e busca familiares no catálogo de pessoas
4. Identifica empresas de familiares
5. Cruza com contratos e licitações do governo estadual
6. Detecta se empresas do governador ou familiares receberam contratos públicos

**Resultado:** Empresas do governador ou familiares com contratos públicos, com valores, órgãos contratantes e severidade.

---

## Redes de Influência

### Quem é o operador central que conecta múltiplas redes de influência?

**O que o sistema faz:** Constrói grafos de relacionamento entre entidades (pessoas, empresas, órgãos) e identifica os nós centrais que conectam clusters distintos.

**Como funciona:**

1. Extrai todos os CPFs e CNPJs dos dados importados
2. Constrói um grafo onde cada entidade é um nó e cada co-ocorrência é uma aresta
3. Aplica algoritmos de centralidade para identificar os nós mais conectados
4. Detecta clusters (comunidades) dentro do grafo
5. Identifica entidades que aparecem em múltiplos clusters (operadores centrais)
6. Cruza com dados OSINT para enriquecer informações

**Resultado:** Operadores centrais identificados, com lista de redes que conectam, número de conexões e entidades envolvidas.

---

### Existem empresas de fachada que compartilham endereço, telefone ou domínio web?

**O que o sistema faz:** Detecta empresas distintas que compartilham dados cadastrais idênticos ou muito similares, indicando possível fachada ou laranja.

**Como funciona:**

1. Agrupa empresas por endereço completo
2. Agrupa por telefone
3. Agrupa por domínio web (extraído de emails ou sites)
4. Identifica grupos com 3+ empresas compartilhando o mesmo dado
5. Cruza com situação cadastral (empresas inativas com mesmo endereço de ativas)
6. Verifica se empresas do grupo receberam recursos públicos

**Resultado:** Clusters de empresas de fachada potenciais, com dados compartilhados, lista de empresas e recebimentos públicos.

---

### O senador tem rede de influência com governadores e prefeitos?

**O que o sistema faz:** Constrói o grafo de relacionamentos do senador buscando conexões com governadores e prefeitos através de fornecedores compartilhados, co-ocorrências e vínculos societários.

**Como funciona:**

1. Coleta dados do senador (despesas CEAPS, proposições, votações)
2. Identifica fornecedores das despesas do senador
3. Cruza com fornecedores de governadores e prefeitos (dados do TSE, Portal da Transparência)
4. Busca vínculos societários compartilhados
5. Constrói o grafo de conexões e calcula graus de separação
6. Identifica caminhos de influência entre o senador e agentes municipais/estaduais

**Resultado:** Rede de influência do senador com governadores e prefeitos, com caminhos de conexão e entidades intermediárias.

---

### Existem ciclos de dinheiro onde o recurso volta ao parlamentar?

**O que o sistema faz:** Detecta caminhos circulares onde dinheiro público sai do parlamentar como despesa e retorna a ele ou seus familiares através de empresas intermediárias.

**Como funciona:**

1. Mapeia todos os fornecedores do deputado
2. Identifica sócios de cada fornecedor
3. Cruza sócios com catálogo de pessoas buscando o deputado ou familiares
4. Constrói caminhos: Deputado → Fornecedor → Sócio → Deputado/Familiar
5. Calcula o valor total que circulou no ciclo
6. Gera achado de severidade **5 (máxima)** quando o ciclo é confirmado

**Resultado:** Ciclos de dinheiro detectados, com caminho completo, valores envolvidos e entidades em cada etapa.

---

## Licitações e Contratos

### A empresa que venceu a licitação é fachada ou laranja de político?

**O que o sistema faz:** Analisa empresas vencedoras de licitações buscando sinais de fachada: endereço compartilhado, telefone igual, sócios ocultos, capital social incompatível com o valor do contrato.

**Como funciona:**

1. Coleta dados de licitações e contratos do Portal da Transparência
2. Para cada empresa vencedora, verifica:
   - Endereço: compartilha com outras empresas?
   - Telefone: é o mesmo de outras empresas?
   - Capital social: é compatível com o valor do contrato?
   - Situação cadastral: está ativa, suspensa ou inapta?
   - Tempo de existência: foi criada pouco antes da licitação?
3. Cruza sócios com catálogo de políticos e familiares
4. Calcula score de risco baseado nos fatores encontrados

**Resultado:** Empresas vencedoras com sinais de fachada, com fatores de risco identificados, score e vínculos com políticos.

---

### Existem obras públicas que receberam pagamento mas não existem fisicamente?

**O que o sistema faz:** Identifica contratos de obras que receberam pagamentos mas apresentam sinais de irregularidade: aditivos abusivos, cronograma descumprido, valores acima de mercado.

**Como funciona:**

1. Coleta contratos de obras do Portal da Transparência
2. Verifica se houve pagamento e em quanto tempo
3. Compara valor do contrato com médias de mercado para o tipo de obra
4. Detecta aditivos que multiplicaram o valor original
5. Cruza com dados geográficos para verificar localização
6. Identifica obras com pagamento mas sem conclusão registrada

**Resultado:** Obras com sinais de irregularidade, com pagamentos realizados, aditivos, superfaturamento potencial e status.

---

### Empresas sancionadas no CEIS continuam recebendo contratos públicos?

**O que o sistema faz:** Cruza a lista de empresas inidôneas e suspensas (CEIS) com contratos e pagamentos ativos do governo federal.

**Como funciona:**

1. Coleta lista atualizada do CEIS (Cadastro de Empresas Inidôneas e Suspensas)
2. Coleta contratos e pagamentos ativos do Portal da Transparência
3. Cruza por CNPJ (match exato)
4. Para cada correspondência, verifica:
   - Data da sanção vs. data do contrato
   - Órgão que contratou vs. órgão que aplicou a sanção
   - Valor total recebido após a sanção
5. Gera achado de severidade **4 (crítica)**

**Resultado:** Empresas sancionadas com contratos ativos, com valores, órgãos contratantes e período de vigência.

---

### O juiz tem decisões que beneficiam empresas de aliados?

**O que o sistema faz:** Cruza dados de decisões judiciais com vínculos societários para identificar se juízes decidem repetidamente a favor de empresas ligadas a seus aliados políticos.

**Como funciona:**

1. Coleta dados de decisões judiciais (quando disponíveis)
2. Identifica empresas beneficiadas nas decisões
3. Busca vínculos societários das empresas
4. Cruza com dados de políticos aliados do juiz
5. Detecta padrões de decisões favoráveis a empresas de aliados
6. Calcula frequência e valor total beneficiado

**Resultado:** Padrões de decisões judiciais beneficiando empresas de aliados, com frequência, valores e vínculos.

---

## Insider Trading

### Políticos aparecem como negociantes de ações na CVM?

**O que o sistema faz:** Cruza nomes de políticos eleitos e candidatos com registros de negociação de valores mobiliários na CVM, detectando se operam ações de empresas listadas na B3.

**Como funciona:**

1. Coleta lista de parlamentares (deputados e senadores)
2. Coleta registros de insider trading da CVM (ICVM 358 e CVM 44)
3. Aplica fuzzy matching entre nomes de políticos e nomes de negociantes
4. Para cada correspondência, verifica:
   - Tipo de operação (compra ou venda)
   - Empresa negociada
   - Data da operação
   - Valor da operação
5. Cruza com votações e proposições relevantes para a empresa
6. Gera achado de severidade **5 (máxima)** quando político negocia ações de empresa impactada por legislação

**Resultado:** Políticos identificados como negociantes de ações, com operações detalhadas, empresas envolvidas e correlação com atividade legislativa.

---

### Existem parlamentares que negociam ações de empresas impactadas por legislação em tramitação?

**O que o sistema faz:** Detecta se operações de ações por parlamentares coincidem temporalmente com votações de leis que impactam diretamente as empresas negociadas.

**Como funciona:**

1. Identifica parlamentares que operam ações (da análise anterior)
2. Coleta proposições legislativas em tramitação no período das operações
3. Classifica proposições por setor econômico impactado
4. Cruza setor da proposição com setor da empresa negociada
5. Verifica se a operação ocorreu antes da votação (insider trading)
6. Calcula correlação temporal entre operação e votação

**Resultado:** Casos de possível insider trading político, com parlamentar, empresa, operação, proposição relacionada e correlação temporal.

---

[Voltar à página inicial](./README.md)
