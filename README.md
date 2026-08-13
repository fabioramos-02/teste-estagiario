# Desafio Estagiário — Dashboard MS Digital

Bem-vindo, Antonio! Este repositório contém sua primeira demanda na SETDIG/SGD.

## O que você recebeu

Um extrato **anonimizado** de dados reais do sistema MS Digital (aplicativo do
cidadão sul-mato-grossense). Sua missão é construir uma aplicação que
apresente esses dados de forma útil para o time.

## Estrutura do repositório

```
teste-estagiario/
├── README.md                    ← este arquivo
├── ROTEIRO.md                   ← o que você precisa entregar
├── DICIONARIO_DADOS.md          ← descrição de cada arquivo e coluna
└── data/
    ├── conta.csv                ← 367k contas
    ├── usuario.csv              ← 367k usuários
    ├── endereco.csv             ← 6,7k endereços
    ├── carteira_funcional.csv   ← 20,5k matrículas
    └── municipios_ibge.csv      ← lookup código IBGE → nome/UF
```

## Como abrir os dados

Todos os arquivos são CSV UTF-8. Escolha a ferramenta que preferir:

- **Excel/LibreOffice**: abrir direto (importar como UTF-8)
- **Python**: `pandas.read_csv("data/conta.csv")`
- **JavaScript/Node**: `papaparse`, `csv-parse`
- **SQL local**: importar em SQLite/DuckDB

## Próximos passos

1. Leia `DICIONARIO_DADOS.md` para entender o que cada campo representa
2. Leia `ROTEIRO.md` para ver o que precisa entregar e como será avaliado
3. Comece explorando os dados antes de escrever qualquer código

## Sobre os dados

- Foram **anonimizados** conforme LGPD (CPFs viraram hash, nomes são fictícios, dados sensíveis removidos)
- O hash de CPF é consistente entre as tabelas — você pode fazer JOIN entre `conta.csv`, `usuario.csv` e `endereco.csv` pela coluna `cpf_hash`
- Números batem com o banco de produção no momento da extração

## Prazo e entrega

*(a definir com Fabio)*

## Dúvidas

Fale com o Fabio (SGD/SETDIG). Não hesite em perguntar — parte do exercício é
saber quando pedir contexto.
