# Segurança e Boas Práticas

Como o CiênciaDados protege dados e garante a integridade das análises.

---

## Proteção de Dados

### Credenciais

As credenciais de banco de dados ficam em arquivo `.env` fora do controle de versão (`.gitignore`). O sistema usa **Trusted Connection** (autenticação Windows) por padrão, eliminando a necessidade de armazenar senhas.

### Dados Sensíveis

O sistema trabalha exclusivamente com **dados públicos** de portais governamentais abertos. Não armazena informações sigilosas ou restritas.

### Logs sem Dados Pessoais

Os logs de operação registram comandos executados, duração e status, mas não armazenam CPFs, CNPJs ou nomes das entidades analisadas.

---

## Integridade das Análises

### Achados são Indícios

Os achados gerados pelo sistema são **indícios** que devem ser investigados e validados antes de qualquer ação legal. O sistema não substitui:

- Investigação humana
- Auditoria formal
- Processo judicial

### Classificação Transparente

Cada achado inclui as evidências que suportam a conclusão, permitindo que o analista verifique e valide os resultados.

### Reprodutibilidade

Todas as análises são determinísticas: executando os mesmos comandos sobre os mesmos dados, os resultados são idênticos. Isso permite auditoria do próprio sistema.

---

## Boas Práticas de Operação

### Comece pelo Básico

Sempre execute análises de qualidade de dados (perfil, duplicidades, validação) antes de análises de fraude. Dados ruins geram achados ruins.

### Valide Mapeamentos

Antes de rodar análises, verifique se os campos estão corretamente mapeados com `cienciadados campos -f <id>`.

### Use Severidade como Filtro

- **Severidade 3+** para relatórios de auditoria
- **Severidade 4+** para alertas executivos
- **Todas as severidades** para investigação exploratória

### Mantenha Conjuntos Organizados

Use `conjunto-criar` e `conjunto-associar` para organizar fontes de dados por tema. Isso facilita rastrear a origem dos achados.

### Backup Regular

Faça backups regulares do banco de dados SQL Server. O comando `resetar` apaga todos os dados irreversivelmente.

---

## Cuidados com Interpretação

### Fuzzy Matching Não é Certeza

Matches por similaridade de nomes (fuzzy matching) com 85% de tolerância podem gerar falsos positivos. Sempre valide manualmente achados baseados apenas em similaridade de nomes.

### Coincidência ≠ Causalidade

Um achado de que um familiar de parlamentar tem o mesmo sobrenome que um fornecedor não prova irregularidade. É um **indício** que merece investigação adicional.

### Contexto Importa

Outliers em valores de contrato podem ser legítimos (obras de grande porte) ou suspeitos (superfaturamento). O sistema sinaliza — o analista investiga.

---

## Aviso Legal

Este sistema é uma ferramenta de análise de dados abertos e detecção de padrões. Os achados gerados são **indícios** que devem ser investigados e validados antes de qualquer ação legal. O sistema não substitui investigação humana, auditoria formal ou processo judicial.

---

[Voltar à página inicial](./README.md)
