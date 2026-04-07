# Fluxo de Uso

Guia passo a passo para novos usuários do CiênciaDados.

---

## Fluxo Recomendado

```
1. testar-conexao
        ↓
2. conjunto-criar
        ↓
3. baixar e importar dados da API
        ↓
4. status
        ↓
5. layout e mapear campos
        ↓
6. analisar perfil
        ↓
7. analisar duplicidades
        ↓
8. Análises Básicas
        ↓
9. Análises de Fraude e Consistência
        ↓
10. achados (Terminal)
        ↓
11. relatorio (Exportação)
```

---

## Exemplo Prático Completo

```bash
# 1. Testar conexão
cienciadados testar-conexao

# 2. Criar grupo
cienciadados conjunto-criar -n "Análise Ocupantes de Cargos Públicos"

# 3. Baixar dados das APIs (Câmara, Senado, CVM e TSE)
#    A flag '-i' já faz as importações automaticamente
cienciadados parlamentar -d deputados -i
cienciadados parlamentar -d senadores -i
cienciadados baixar -d ceis -p 202401 -i
cienciadados cvm -d insider -a 2024 -i

# 4. Ver o que foi importado e associar os IDs ao grupo
cienciadados status
cienciadados conjunto-associar -c 1 -f 1

# 5. Ver layout e mapear campos para as bases
cienciadados layout -i 1
cienciadados mapear -f 1 -p cpf_cnpj -c "CPF OU CNPJ DO SANCIONADO"
cienciadados mapear -f 1 -p nome -c "NOME DO SANCIONADO"

# 6. Análises Básicas
cienciadados analisar perfil -i 1
cienciadados analisar duplicidades -i 1
cienciadados analisar validacao -i 1

# 7. Análises de Investigação, Fraude e Insider Trading
cienciadados analisar parlamentar-sancao -p 1 -s 3 -t 85
cienciadados analisar parlamentar-fornecedor -d 4 -s 3
cienciadados analisar insider -p 1 -c 9 -t 85

# 8. Ver resultados e irregularidades descobertas
cienciadados achados -s 3

# 9. Gerar Entregável de Auditoria (Markdown)
cienciadados relatorio -o "auditoria_ocupantes_cargos.md" -s 3
```

---

## Dicas para Iniciantes

- **Sempre comece com `testar-conexao`** para validar o ambiente
- **Use `status` frequentemente** para acompanhar o progresso das importações
- **Comece com análises básicas** (perfil, duplicidades) antes de ir para as de fraude
- **Severidade 3+** é o ponto de partida recomendado para filtrar achados relevantes
- **O sistema é resumível** — se interromper uma importação, basta executar o mesmo comando novamente

---

[Voltar à página inicial](./index.md)
