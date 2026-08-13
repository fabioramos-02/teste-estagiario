# Roteiro do Desafio

## Objetivo

Construir uma aplicação (web, desktop, notebook — sua escolha) que apresente
métricas e visualizações úteis sobre a base de contas do MS Digital.

Não existe "resposta certa". A avaliação foca em **como você pensa sobre os
dados** e **como justifica suas decisões técnicas**.

---

## Etapa 1 — Explorar os dados (1 a 2 horas)

Antes de escrever qualquer linha de código de aplicação, abra os CSVs e
responda para si mesmo:

- Quantas linhas cada arquivo tem?
- Qual é a chave primária de cada tabela?
- Quais tabelas se conectam entre si? Por qual campo?
- Existe alguma tabela que **não** se conecta com as outras? Por quê?
- Que porcentagem dos usuários tem endereço cadastrado?
- Os campos `cidade` e `uf` em `endereco.csv` são texto ou código? Como você resolveria isso?
- Alguma coluna tem muitos valores nulos? Isso muda o que você pode analisar?

**Entregável desta etapa:** um arquivo `analise-inicial.md` (1 página, no
próprio repositório do seu projeto) com **3 a 5 observações críticas** sobre a
base. Não precisa ser bonito. Precisa mostrar que você olhou os dados com
atenção antes de sair codando.

---

## Etapa 2 — Desenhar a aplicação (30 min a 1 hora)

Escreva uma nota curta (`decisao-tecnica.md`, meia página é suficiente)
respondendo:

- Qual stack você vai usar? Por quê?
- Vai ter backend ou é só frontend consumindo CSV?
- Onde os dados vão morar (arquivo local, SQLite, banco, memória)?
- Como você vai lidar com performance? (367 mil linhas não é grande, mas não
  é trivial de renderizar direto no DOM)

**Não use tecnologia que você não sabe justificar.** "É o que eu conheço" é
uma resposta válida. "Vi no LinkedIn que estava em alta" não é.

---

## Etapa 3 — Implementar o dashboard

Métricas mínimas que a aplicação deve mostrar:

1. **KPIs no topo:**
   - Total de contas
   - Contas ativas (`ativo = true`)
   - Total de matrículas (registros em `carteira_funcional.csv`)

2. **Distribuição geográfica:**
   - Contas por UF e/ou por município (mapa OU tabela — sua escolha)
   - Cuidado: `endereco.csv` cobre uma fração pequena dos usuários. Deixe
     isso claro na interface.

3. **Contas criadas por ano:**
   - Gráfico de barras (ou linha) com total de contas criadas por ano
   - Se conseguir, mostre também quantas dessas continuam ativas hoje

4. **Faixa etária dos usuários:**
   - Distribuição por faixas de idade (calcule a partir de `dataNascimento`
     em `usuario.csv`)
   - Escolha faixas razoáveis (ex: 0-17, 18-24, 25-34, 35-44, 45-59, 60+)

## Etapa 4 — Rodar / publicar

Deixe funcionando. Pode ser:

- Local (`npm start`, `streamlit run`, `python app.py` — o que for)
- Deploy simples (Vercel, Netlify, Streamlit Cloud, Render — todos com free tier)

Documente **como rodar** no seu `README.md`.

---

## Etapa 5 — Apresentar

Ao final, você vai me mostrar (15-20 min):

1. As observações da Etapa 1
2. A decisão técnica da Etapa 2
3. O dashboard rodando
4. O que você deixou de fora e por quê
5. O que faria diferente se tivesse mais tempo

---

## Critérios de avaliação

Em ordem de importância:

1. **Leitura crítica dos dados** — você percebeu as pegadinhas antes de codar?
2. **Decisões justificadas** — você sabe explicar por que escolheu cada coisa?
3. **Funciona** — o dashboard abre e mostra os números certos
4. **Código legível** — nomes claros, sem esperteza desnecessária
5. **Estética** — importa, mas menos que os quatro acima

O que **não** conta pontos extras:

- Stack "moderninha" que ninguém do time usa
- Testes automatizados (não é escopo)
- Docker/CI/CD (não é escopo)
- Autenticação, cadastro de usuário, admin panel (não pediu)

---

## Regras de ouro

- **Pergunte quando estiver em dúvida.** Não perder 4 horas fazendo a coisa errada é uma habilidade profissional.
- **Prazo maior que o necessário é sinal de escopo mal definido.** Se a etapa está demorando muito, para e pergunta.
- **Commits pequenos e frequentes.** Não me entregue um único commit "primeira versão" no último dia.
- Os dados são **anonimizados mas ainda são dados de gente**. Não publique num repositório público sem checar antes.
