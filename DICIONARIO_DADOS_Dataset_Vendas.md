# Dicionário de Dados - Dataset_Vendas_2023_2025

Documentação completa do modelo semântico Power BI com todas as tabelas, colunas, relacionamentos e medidas DAX.

## 📋 Índice

1. [Estrutura Geral](#estrutura-geral)
2. [Tabelas de Dimensão](#tabelas-de-dimensão)
3. [Tabela de Fatos](#tabela-de-fatos)
4. [Tabela de Medidas](#tabela-de-medidas)
5. [Medidas DAX Detalhadas](#medidas-dax-detalhadas)
6. [Relacionamentos](#relacionamentos)
7. [Padrões de Nomenclatura](#padrões-de-nomenclatura)

---

## Estrutura Geral

**Total de Tabelas:** 9
- Dimensões: 7
- Fatos: 1
- Auxiliares: 1

**Total de Colunas:** 64
**Total de Medidas DAX:** 32
**Total de Relacionamentos:** 7

---

## Tabelas de Dimensão

### 1. dim_canal

**Descrição:** Dimensão de canais de vendas (presencial, online, telefone, etc.)

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Canal | Int | Chave primária |
| Nome_Canal | Text | Nome do canal de vendas |
| Tipo_Canal | Text | Tipo: Presencial, Online, Telefone |
| Status | Text | Ativo/Inativo |
| Data_Inicio | Date | Data de início |
| Comissao_Padrao | Decimal | Comissão padrão (%) |
| Gerenciador | Text | Responsável |

**Relacionamento:** fato_vendas → dim_canal (1:N)

---

### 2. dim_cliente

**Descrição:** Dimensão de clientes com informações demográficas e comerciais

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Cliente | Int | Chave primária |
| Nome_Cliente | Text | Nome completo |
| CPF_CNPJ | Text | CPF ou CNPJ |
| Segmento | Text | PJ, PF, Governo |
| Cidade | Text | Cidade |
| Estado | Text | UF (SP, RJ, MG, etc.) |
| Pais | Text | País |
| Data_Cadastro | Date | Data de primeiro cadastro |
| Status_Cliente | Text | Ativo/Inativo |
| Limite_Credito | Decimal | Limite de crédito |
| Categoria_Risco | Text | Alto, Médio, Baixo |
| Telefone | Text | Telefone para contato |
| Email | Text | Email |

**Relacionamento:** fato_vendas → dim_cliente (1:N)

---

### 3. dim_data

**Descrição:** Dimensão temporal para análise de séries temporais

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Data | Int | Chave primária (YYYYMMDD) |
| Data | Date | Data completa |
| Ano | Int | Ano (2023, 2024, 2025) |
| Trimestre | Text | Q1, Q2, Q3, Q4 |
| Mes | Int | Mês (1-12) |
| Nome_Mes | Text | Janeiro, Fevereiro... |
| Semana | Int | Semana do ano |
| Dia | Int | Dia do mês |
| Dia_Semana | Int | 1-7 |
| Nome_Dia_Semana | Text | Segunda-feira... Domingo |
| Eh_Fim_Semana | Boolean | Sim/Não |
| Eh_Feriado | Boolean | Sim/Não |
| Nome_Feriado | Text | Nome do feriado (se houver) |
| Dias_Uteis_Mes | Int | Dias úteis no mês |

**Relacionamento:** fato_vendas → dim_data (1:N)

---

### 4. dim_fornecedor

**Descrição:** Dimensão de fornecedores de produtos

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Fornecedor | Int | Chave primária |
| Nome_Fornecedor | Text | Razão social |
| CNPJ | Text | CNPJ do fornecedor |
| Cidade | Text | Cidade |
| Estado | Text | UF |
| Pais | Text | País |
| Telefone | Text | Telefone |
| Email | Text | Email |
| Data_Cadastro | Date | Quando começou a fornecer |
| Status | Text | Ativo/Inativo |
| Classificacao | Text | A, B, C (volume de negócios) |
| Prazo_Pagamento | Int | Prazo médio (dias) |
| Desconto_Padrao | Decimal | Desconto padrão (%) |

**Relacionamento:** dim_produto → dim_fornecedor (1:N)

---

### 5. dim_loja

**Descrição:** Dimensão de pontos de venda físicos

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Loja | Int | Chave primária |
| Nome_Loja | Text | Nome da loja |
| Numero_Loja | Text | Código da loja |
| Cidade | Text | Cidade |
| Estado | Text | UF |
| Regiao | Text | Sul, Sudeste, Nordeste, etc. |
| Gerente | Text | Nome do gerente |
| Data_Inauguracao | Date | Data de abertura |
| Status | Text | Aberta/Fechada |
| Area_Comercial | Decimal | Metragem (m²) |
| Telefone | Text | Telefone |
| Email | Text | Email da loja |
| Categoria | Text | Matriz, Filial, Quiosque |

**Relacionamento:** fato_vendas → dim_loja (1:N)

---

### 6. dim_produto

**Descrição:** Dimensão de produtos vendidos

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Produto | Int | Chave primária |
| SKU | Text | Stock Keeping Unit |
| Nome_Produto | Text | Nome do produto |
| Categoria | Text | Eletrônicos, Vestuário, etc. |
| Subcategoria | Text | Subcategoria de produto |
| Marca | Text | Marca |
| Preco_Custo | Decimal | Custo unitário |
| Preco_Varejo | Decimal | Preço de venda sugerido |
| Preco_Fornecedor | Decimal | Preço de custo do fornecedor |
| ID_Fornecedor | Int | FK → dim_fornecedor |
| Margem_Padrao | Decimal | Margem de lucro esperada (%) |
| Estoque_Minimo | Decimal | Quantidade mínima |
| Estoque_Maximo | Decimal | Quantidade máxima |
| Peso | Decimal | Peso em kg |
| Data_Lancamento | Date | Quando foi lançado |
| Status | Text | Ativo/Descontinuado |
| Promocao_Vigente | Boolean | Sim/Não |
| Percentual_Desconto | Decimal | Desconto em promoção (%) |

**Relacionamento:** fato_vendas → dim_produto (1:N)

---

### 7. dim_vendedor

**Descrição:** Dimensão de vendedores

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Vendedor | Int | Chave primária |
| Nome_Vendedor | Text | Nome completo |
| CPF | Text | CPF |
| Data_Admissao | Date | Data de entrada |
| Status | Text | Ativo/Inativo/Afastado |
| Cargo | Text | Gerente, Consultor, Trainee |
| ID_Loja | Int | FK → dim_loja (loja principal) |
| Telefone | Text | Telefone corporativo |
| Email | Text | Email corporativo |
| Meta_Anual | Decimal | Meta anual em reais |
| Comissao_Variavel | Decimal | Comissão variável (%) |
| Data_Nascimento | Date | Data de nascimento |
| Regiao_Atuacao | Text | Região onde atua |
| Supervisor | Text | Nome do supervisor |

**Relacionamento:** fato_vendas → dim_vendedor (1:N)

---

## Tabela de Fatos

### fato_vendas

**Descrição:** Tabela de fatos contendo todas as transações de vendas

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| ID_Venda | Int | Chave primária |
| ID_Data | Int | FK → dim_data |
| ID_Cliente | Int | FK → dim_cliente |
| ID_Produto | Int | FK → dim_produto |
| ID_Vendedor | Int | FK → dim_vendedor |
| ID_Loja | Int | FK → dim_loja |
| ID_Canal | Int | FK → dim_canal |
| Quantidade | Decimal | Quantidade vendida |
| Preco_Unitario | Decimal | Preço unitário (R$) |
| Valor_Bruto | Decimal | Quantidade × Preço Unitário |
| Percentual_Desconto | Decimal | Desconto concedido (%) |
| Valor_Desconto | Decimal | Valor do desconto (R$) |
| Valor_Liquido | Decimal | Valor Bruto - Desconto |
| Valor_Imposto | Decimal | Impostos (ICMS, PIS, COFINS) |
| Valor_Frete | Decimal | Custo de frete |
| Valor_Final | Decimal | Valor total da venda |
| Custo_Produto | Decimal | Custo do produto vendido |
| Lucro_Bruto | Decimal | Valor Liquido - Custo |
| Lucro_Liquido | Decimal | Lucro após todos os custos |
| Comissao_Vendedor | Decimal | Comissão do vendedor |
| Numero_Parcelas | Int | Número de parcelas (1+) |
| Tipo_Pagamento | Text | Dinheiro, Cartão, Cheque, Débito |
| Status_Venda | Text | Concluída, Cancelada, Devolvida |
| Data_Entrega | Date | Data da entrega |
| Dias_Atraso | Int | Dias de atraso na entrega |
| Observacoes | Text | Observações da venda |

**Total de Linhas (período):** ~2.5M registros (2023-2025)

---

## Tabela de Medidas

### tblMedidas

**Descrição:** Tabela que armazena todas as 32 medidas DAX organizadas por complexidade

| Campo | Descrição |
|-------|-----------|
| Pasta | Categoria da medida (Base, Derivada, Temporal, Performance, Avançada) |
| Nome_Medida | Nome único da medida |
| Descricao | Descrição detalhada |
| Formula_DAX | Fórmula DAX completa |
| Tipo_Dado | Tipo de dado (Número, Moeda, %) |
| Ultima_Atualizacao | Quando foi criada/alterada |

---

## Medidas DAX Detalhadas

### Pasta 1: Medidas Base (4 medidas)

#### 1. Qtd Total
```dax
Qtd Total = SUMX(fato_vendas, fato_vendas[Quantidade])
```
**Descrição:** Soma total de quantidade de produtos vendidos
**Tipo:** Número
**Uso:** Dashboard de vendas

#### 2. Receita Total
```dax
Receita Total = SUMX(fato_vendas, fato_vendas[Valor_Final])
```
**Descrição:** Soma total de receita (valor final de todas as vendas)
**Tipo:** Moeda
**Uso:** KPI principal de faturamento

#### 3. Num Vendas
```dax
Num Vendas = COUNTA(fato_vendas[ID_Venda])
```
**Descrição:** Número total de transações de vendas
**Tipo:** Número
**Uso:** Análise de transações

#### 4. Dias Operacionais
```dax
Dias Operacionais = 
CALCULATE(
    DISTINCTCOUNT(dim_data[ID_Data]),
    dim_data[Eh_Feriado] = FALSE
)
```
**Descrição:** Dias úteis com vendas no período
**Tipo:** Número
**Uso:** Análise de eficiência

---

### Pasta 2: Medidas Derivadas (3 medidas)

#### 5. Ticket Médio
```dax
Ticket Médio = 
DIVIDE(
    [Receita Total],
    [Num Vendas],
    0
)
```
**Descrição:** Valor médio por transação
**Tipo:** Moeda
**Uso:** Análise de performance comercial

#### 6. Quantidade Média
```dax
Quantidade Média = 
DIVIDE(
    [Qtd Total],
    [Num Vendas],
    0
)
```
**Descrição:** Média de produtos por transação
**Tipo:** Número
**Uso:** Análise comportamental

#### 7. Receita Média/Dia
```dax
Receita Média Dia = 
DIVIDE(
    [Receita Total],
    [Dias Operacionais],
    0
)
```
**Descrição:** Receita média por dia útil
**Tipo:** Moeda
**Uso:** Análise de produtividade diária

---

### Pasta 3: Medidas Temporais (5 medidas)

#### 8. Receita MTD
```dax
Receita MTD = 
CALCULATE(
    [Receita Total],
    DATESMTD(dim_data[Data])
)
```
**Descrição:** Receita mês até hoje (Month-To-Date)
**Tipo:** Moeda
**Uso:** Acompanhamento mensal

#### 9. Receita YTD
```dax
Receita YTD = 
CALCULATE(
    [Receita Total],
    DATESYTD(dim_data[Data])
)
```
**Descrição:** Receita acumulada no ano (Year-To-Date)
**Tipo:** Moeda
**Uso:** Análise anual

#### 10. Quantidade MTD
```dax
Quantidade MTD = 
CALCULATE(
    [Qtd Total],
    DATESMTD(dim_data[Data])
)
```
**Descrição:** Quantidade vendida mês até hoje
**Tipo:** Número
**Uso:** Análise de volume mensal

#### 11. Quantidade YTD
```dax
Quantidade YTD = 
CALCULATE(
    [Qtd Total],
    DATESYTD(dim_data[Data])
)
```
**Descrição:** Quantidade acumulada no ano
**Tipo:** Número
**Uso:** Análise de volume anual

#### 12. Vendas MTD
```dax
Vendas MTD = 
CALCULATE(
    [Num Vendas],
    DATESMTD(dim_data[Data])
)
```
**Descrição:** Número de transações mês até hoje
**Tipo:** Número
**Uso:** Análise transacional

---

### Pasta 4: Medidas de Performance (15 medidas)

#### 13. Custo Total
```dax
Custo Total = SUMX(fato_vendas, fato_vendas[Custo_Produto])
```
**Descrição:** Soma total de custos de produtos
**Tipo:** Moeda

#### 14. Lucro Bruto Total
```dax
Lucro Bruto Total = 
[Receita Total] - [Custo Total]
```
**Descrição:** Receita menos custo
**Tipo:** Moeda

#### 15. Margem Bruta %
```dax
Margem Bruta % = 
DIVIDE(
    [Lucro Bruto Total],
    [Receita Total],
    0
)
```
**Descrição:** Margem de lucro bruto
**Tipo:** Percentual

#### 16. Lucro Líquido Total
```dax
Lucro Líquido Total = 
SUMX(fato_vendas, fato_vendas[Lucro_Liquido])
```
**Descrição:** Lucro após todos os custos
**Tipo:** Moeda

#### 17. Margem Líquida %
```dax
Margem Líquida % = 
DIVIDE(
    [Lucro Líquido Total],
    [Receita Total],
    0
)
```
**Descrição:** Margem de lucro líquido
**Tipo:** Percentual

#### 18. Total Impostos
```dax
Total Impostos = SUMX(fato_vendas, fato_vendas[Valor_Imposto])
```
**Descrição:** Soma de todos os impostos
**Tipo:** Moeda

#### 19. Receita Anterior
```dax
Receita Anterior = 
CALCULATE(
    [Receita Total],
    DATEADD(dim_data[Data], -1, MONTH)
)
```
**Descrição:** Receita do mês anterior
**Tipo:** Moeda

#### 20. Variação Receita %
```dax
Variação Receita % = 
DIVIDE(
    [Receita Total] - [Receita Anterior],
    [Receita Anterior],
    0
)
```
**Descrição:** Variação percentual vs mês anterior
**Tipo:** Percentual

#### 21. Meta Atingida
```dax
Meta Atingida = 
CALCULATE(
    SUMX(dim_vendedor, dim_vendedor[Meta_Anual]),
    ALLEXCEPT(dim_vendedor, dim_vendedor[ID_Vendedor])
)
```
**Descrição:** Meta planejada
**Tipo:** Moeda

#### 22. % Meta Alcançado
```dax
% Meta Alcançado = 
DIVIDE(
    [Receita Total],
    [Meta Atingida],
    0
)
```
**Descrição:** Percentual de meta atingido
**Tipo:** Percentual

#### 23. Comissão Total
```dax
Comissão Total = 
SUMX(fato_vendas, fato_vendas[Comissao_Vendedor])
```
**Descrição:** Total de comissões pagas
**Tipo:** Moeda

#### 24. Desconto Concedido
```dax
Desconto Concedido = 
SUMX(fato_vendas, fato_vendas[Valor_Desconto])
```
**Descrição:** Total de descontos concedidos
**Tipo:** Moeda

#### 25. % Desconto
```dax
% Desconto = 
DIVIDE(
    [Desconto Concedido],
    [Receita Total] + [Desconto Concedido],
    0
)
```
**Descrição:** Percentual de desconto sobre vendas
**Tipo:** Percentual

#### 26. Frete Total
```dax
Frete Total = SUMX(fato_vendas, fato_vendas[Valor_Frete])
```
**Descrição:** Custo total de frete
**Tipo:** Moeda

#### 27. Devolução %
```dax
Devolução % = 
CALCULATE(
    [Num Vendas],
    fato_vendas[Status_Venda] = "Devolvida"
) / [Num Vendas]
```
**Descrição:** Percentual de devoluções
**Tipo:** Percentual

---

### Pasta 5: Medidas Avançadas (5 medidas)

#### 28. Crescimento MoM
```dax
Crescimento MoM = 
DIVIDE(
    [Receita Total] - [Receita Anterior],
    [Receita Anterior],
    BLANK()
)
```
**Descrição:** Crescimento mês a mês
**Tipo:** Percentual

#### 29. Tendência Receita
```dax
Tendência Receita = 
AVERAGE(
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -6, MONTH)
    ),
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -5, MONTH)
    ),
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -4, MONTH)
    ),
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -3, MONTH)
    ),
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -2, MONTH)
    ),
    CALCULATE(
        [Receita Total],
        DATEADD(dim_data[Data], -1, MONTH)
    )
)
```
**Descrição:** Tendência de 6 meses
**Tipo:** Moeda

#### 30. Média Móvel 12M
```dax
Média Móvel 12M = 
AVERAGEX(
    CALCULATETABLE(
        VALUES(dim_data[Data]),
        DATESBETWEEN(
            dim_data[Data],
            DATEADD(MAX(dim_data[Data]), -12, MONTH),
            MAX(dim_data[Data])
        )
    ),
    [Receita Total]
)
```
**Descrição:** Média móvel de 12 meses
**Tipo:** Moeda

#### 31. Concentração Cliente
```dax
Concentração Cliente = 
DIVIDE(
    CALCULATE(
        [Receita Total],
        TOPN(
            1,
            ALL(dim_cliente),
            [Receita Total]
        )
    ),
    [Receita Total],
    0
)
```
**Descrição:** % receita do maior cliente
**Tipo:** Percentual

#### 32. ABC Produto
```dax
ABC Produto = 
VAR RankProduto =
    RANKX(
        ALL(dim_produto),
        [Receita Total]
    )
VAR TotalProdutos =
    CALCULATE(
        DISTINCTCOUNT(dim_produto[ID_Produto]),
        ALL(dim_produto)
    )
RETURN
    IF(
        RankProduto <= TotalProdutos * 0.2, "A",
        IF(
            RankProduto <= TotalProdutos * 0.5, "B",
            "C"
        )
    )
```
**Descrição:** Classificação ABC de produtos
**Tipo:** Texto

---

## Relacionamentos

| De | Para | Tipo | Cardinalidade | Ativa |
|----|----|------|---|---|
| fato_vendas | dim_data | FK | N:1 | Sim |
| fato_vendas | dim_cliente | FK | N:1 | Sim |
| fato_vendas | dim_produto | FK | N:1 | Sim |
| fato_vendas | dim_vendedor | FK | N:1 | Sim |
| fato_vendas | dim_loja | FK | N:1 | Sim |
| fato_vendas | dim_canal | FK | N:1 | Sim |
| dim_produto | dim_fornecedor | FK | N:1 | Sim |

---

## Padrões de Nomenclatura

### Prefixos de Tabelas
- `dim_` = Tabelas de dimensão
- `fato_` = Tabelas de fatos
- `tbl` = Tabelas auxiliares

### Convenções de Colunas
- Chaves primárias: `ID_[TablaName]`
- Chaves estrangeiras: `ID_[ReferencedTable]`
- Datas: sufixo `Data` ou `Data_[Tipo]`
- Valores monetários: prefixo `Valor_` ou `Preco_`
- Booleanos: prefixo `Eh_` ou sufixo `Flag`
- Textos descritivos: `Nome_`, `Tipo_`, `Status`, `Descricao`

### Convenções de Medidas
- Base: nome simples (ex: "Receita Total")
- Derivadas: operações sobre base (ex: "Ticket Médio")
- Temporais: sufixo MTD/YTD (ex: "Receita YTD")
- Performance: descritiva de contexto (ex: "% Meta Alcançado")
- Avançadas: descritiva analítica (ex: "Média Móvel 12M")

---

## Notas Técnicas

- **Atualização:** Diária às 02:00 AM
- **Retenção:** 3 anos (2023-2025)
- **Modo de Armazenamento:** Import (otimizado para query)
- **Particionamento:** Por ano (dim_data)
- **Cache:** Habilitado para todas as tabelas
- **Compactação:** Ativa

---

*Documento gerado em: 14/12/2025*
*Versão: 1.0*
*Responsável: Jefferson Desenvolvedor Master*
