# Dicionário de Dados

Descrição de cada arquivo em `data/`. Leia isto antes de tocar no dashboard.

---

## conta.csv

Representa uma **conta de acesso** no aplicativo MS Digital. Uma conta
corresponde a um cidadão (identificado pelo CPF).

**367.028 linhas.**

| Coluna | Tipo | Descrição |
|---|---|---|
| `cpf_hash` | string (12 chars hex) | Identificador anonimizado do titular. **Chave para JOIN** com `usuario.csv` e `endereco.csv`. Não é CPF real — é hash SHA-256 truncado. |
| `ativo` | bool | Conta ativa (não excluída/bloqueada). ~343k são `True`. |
| `ultimoLogin` | datetime (UTC) | Última autenticação no app. Pode ser nulo (conta criada e nunca acessada). |
| `createdAt` | datetime (UTC) | Quando a conta foi criada. Base para o gráfico "contas criadas por ano". |
| `updatedAt` | datetime (UTC) | Última alteração no registro. |
| `contaGovBr` | int? | Marca se a conta veio via integração gov.br. Investigue os valores possíveis (0, 1, 2?) — o significado exato não está documentado. |

---

## usuario.csv

Dados pessoais do titular da conta. Relação **1:1** com `conta.csv`.

**367.021 linhas** (7 contas sem usuário — anomalia real do banco, não erro
de extração).

| Coluna | Tipo | Descrição |
|---|---|---|
| `cpf_hash` | string (12) | Mesmo hash de `conta.csv`. Use para JOIN. |
| `nome` | string | **FICTÍCIO** — gerado com `Faker`. Não é o nome real. Serve para você ter algo realista para mostrar na UI. |
| `nomeSocial` | string? | Fictício quando presente. Mesma lógica de `nome`. |
| `email` | string | Fictício no formato `user_<hash8>@exemplo.gov.br`. |
| `dataNascimento` | datetime? | Data real. Use para calcular idade / faixa etária. |
| `escolaridade` | string? | Grau de instrução autodeclarado. Muitos nulos. |
| `createdAt` | datetime (UTC) | Quando o cadastro foi feito. |
| `updatedAt` | datetime (UTC) | Última alteração. |

---

## endereco.csv

Endereço do titular. **Apenas ~6.725 linhas** — a grande maioria dos usuários
**não tem endereço cadastrado**. Trate isso como uma amostra, não como o
universo.

Colunas sensíveis removidas por LGPD: `rua`, `numero`, `complemento`, `cep`.

| Coluna | Tipo | Descrição |
|---|---|---|
| `cpf_hash` | string (12) | JOIN com as outras tabelas. |
| `bairro` | string? | Texto livre. Bagunça esperada (typos, "Centro" com variações etc). |
| `cidade` | int? | **Código IBGE do município** (7 dígitos). Ex: `5002704` = Campo Grande/MS. Use `municipios_ibge.csv` para traduzir. |
| `uf` | int | **Código IBGE da UF** (2 dígitos). Ex: `50` = MS. |
| `createdAt` | datetime (UTC) | Quando o endereço foi cadastrado. |
| `updatedAt` | datetime (UTC) | Última alteração. |

---

## carteira_funcional.csv

Registro de matrículas de servidores estaduais. **20.564 linhas** — bate com
o número total de matrículas no sistema.

**IMPORTANTE:** esta tabela **não tem relação direta** com `conta.csv`. O
banco de origem não guarda o vínculo matrícula ↔ CPF. Trate como uma
dimensão isolada.

Colunas sensíveis removidas: `autenticacao`, `token`.

| Coluna | Tipo | Descrição |
|---|---|---|
| `matricula_hash` | string (10 chars hex) | Identificador anonimizado da matrícula do servidor. Não é matrícula real. |
| `createdAt` | datetime (UTC) | Quando a carteira funcional foi emitida. |
| `updatedAt` | datetime (UTC) | Última alteração. |

---

## municipios_ibge.csv

Lookup externo baixado do IBGE. Serve para traduzir os códigos IBGE em
`endereco.csv` para nomes legíveis.

**5.571 linhas** — todos os municípios brasileiros.

| Coluna | Tipo | Descrição |
|---|---|---|
| `codigo_municipio` | int | Código IBGE do município (7 dígitos). JOIN com `endereco.cidade`. |
| `nome_municipio` | string | Nome do município. |
| `codigo_uf` | int | Código IBGE da UF (2 dígitos). JOIN com `endereco.uf`. |
| `sigla_uf` | string(2) | Sigla da UF ("MS", "SP" etc). |
| `nome_uf` | string | Nome da UF ("Mato Grosso do Sul" etc). |

---

## Termos do domínio

- **Matrícula** — identificação do servidor público estadual. Uma pessoa pode
  ser cidadã (tem `Conta`) e servidora (tem `CarteiraFuncional`) ao mesmo
  tempo — mas o banco não guarda esse vínculo.
- **MS Digital** — aplicativo do cidadão do governo de Mato Grosso do Sul,
  mantido pela SETDIG/SGD. Reúne serviços públicos digitais.
- **gov.br** — plataforma federal de identidade única. `contaGovBr` indica
  contas integradas.

---

## Observações críticas para análise

Anote isto na cabeça antes de codar:

1. **Matrícula é ilha.** `carteira_funcional.csv` não tem `cpf_hash`. Você
   pode contar matrículas, mostrar quando foram criadas — mas não pode
   cruzar com `usuario.csv` para dizer "quantos servidores usam o app".
   Se quiser esse cruzamento no futuro, o banco precisa ganhar uma coluna
   de relacionamento.

2. **Endereço é amostra.** ~2% dos usuários. Um "mapa do Brasil" com esses
   dados representa **quem informou endereço**, não o universo de usuários.
   Deixe isso claro na UI (rótulo, tooltip, nota de rodapé).

3. **Cidade e UF são códigos IBGE.** Sem o JOIN com `municipios_ibge.csv`,
   seu mapa vai mostrar "5002704" em vez de "Campo Grande". Faça o JOIN.

4. **Datas em UTC com timezone.** `createdAt` e afins vêm no formato
   `2024-08-02 19:37:16.3850000 +00:00`. Se agrupar por ano/mês, converta
   para o fuso do usuário (America/Campo_Grande é UTC-4) para não jogar
   registros de 21h de 31/dez para o ano seguinte.

5. **Dados são anonimizados mas ainda são pessoas.** Não publique dump
   completo em repo público sem verificar antes.
