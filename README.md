# PowerBI - Dataset Vendas

Repositório contendo a documentação completa do modelo Power BI **Dataset_Vendas_2023_2025** com:

- 📊 9 tabelas (7 dimensões + 1 tabela fatos + 1 tabela de medidas)
- 📈 32 medidas DAX organizadas em 5 pastas por complexidade
- 📋 Dicionário de dados completo com descrições e fórmulas
- 🗂️ Estrutura TMDL para controle de versão

## Estrutura do Repositório

- README.md - Este arquivo
- DICIONARIO_DADOS_Dataset_Vendas.md - Documentação completa
- RESUMO_EXECUTIVO_Dataset_Vendas.txt - Guia rápido

## Resumo do Modelo

| Componente | Quantidade |
|-----------|-----------|
| Tabelas | 9 |
| Colunas | 64 |
| Medidas DAX | 32 |
| Relacionamentos | 7 |

## Tabelas

### Dimensões (7)
- dim_canal: Canais de Vendas
- dim_cliente: Clientes
- dim_data: Dimensão Temporal
- dim_fornecedor: Fornecedores
- dim_loja: Lojas
- dim_produto: Produtos
- dim_vendedor: Vendedores

### Fatos (1)
- fato_vendas: Transações de Vendas

### Auxiliares (1)
- tblMedidas: 32 Medidas DAX

## Medidas DAX (32)

Organizadas em 5 pastas:
1. Medidas Base (4): Qtd Total, Receita Total, Num Vendas, Dias Op
2. Medidas Derivadas (3): Ticket Médio, Qtd Média, Receita Média Dia
3. Medidas Temporais (5): MTD/YTD de Receita e Quantidade
4. Medidas de Performance (15): Variações, Metas, % Alcançado
5. Medidas Avançadas (5): Crescimento, Tendência, Média Móvel

## Como Usar

1. Consultar Fórmulas: Abra DICIONARIO_DADOS_Dataset_Vendas.md
2. Onboarding: Compartilhe RESUMO_EXECUTIVO_Dataset_Vendas.txt

Desenvolvido por: Jefferson Desenvolvedor Master
Repositório: https://github.com/jeffersondesenvolvedormaster/powerbi
