# JSON Viewer → Tabela

Visualizador de JSON em formato de tabela. Página HTML única, sem build, sem dependências e sem backend — os dados nunca saem da máquina.

Feito para o caso do dia a dia: colar a resposta de uma API (`GET /v1/...`) e conseguir ler, filtrar e exportar sem abrir planilha nem escrever script.

**▶ Acesse:** https://andrewsmartins.github.io/jsonViewer/

## Como usar

1. Abra https://andrewsmartins.github.io/jsonViewer/ — ou baixe o `index.html` e abra direto no navegador (duplo clique já resolve).
2. Cole o JSON no painel da esquerda.
3. Clique em **Gerar tabela →** (ou `Ctrl + Enter`).

Não há instalação, servidor local nem `npm install`. Mesmo pela versão publicada, o processamento é 100% no navegador: o JSON colado não é enviado a lugar nenhum.

## Funcionalidades

- **Detecção automática do array** — encontra sozinho o array de registros em estruturas como `[...]`, `{ "results": [...] }` ou `{ "data": { "items": [...] } }`. Se a heurística não acertar, informe o caminho manualmente no campo **Caminho** (ex: `data.items`).
- **Achatamento de objetos aninhados** — `{ "cliente": { "nome": "..." } }` vira a coluna `cliente.nome` (até 3 níveis; abaixo disso o valor é mostrado como JSON). Arrays viram lista separada por vírgula.
- **Colunas dinâmicas** — a união de todas as chaves dos registros, ordenada por frequência. As 12 mais comuns aparecem por padrão; o restante é ligado/desligado no menu **Colunas**.
- **Busca global** com destaque do trecho encontrado em qualquer campo.
- **Ordenação** por qualquer coluna (numérica quando o valor é número, alfabética `pt-BR` no resto).
- **Tipagem visual** — números, booleanos, `null` e datas ganham cor e formatação próprias. Datas ISO (`2026-08-11T14:30:00Z`) e timestamps Unix (10 ou 13 dígitos) são formatados em `dd/mm/aaaa`, com o valor original no `title` da célula.
- **Exportação CSV** do que está na tela — respeita a busca, a ordenação e as colunas visíveis. Sai com BOM UTF-8, então abre acentuado no Excel.
- **Botão Exemplo** carrega um JSON de amostra para testar a ferramenta.

## Formatos aceitos

```json
[ { "id": 1, "nome": "..." } ]
```
```json
{ "results": [ { "id": 1, "nome": "..." } ] }
```
```json
{ "data": { "items": [ { "id": 1, "nome": "..." } ] } }
```

Qualquer outro formato funciona desde que exista um array de objetos em algum ponto — basta apontar o caminho.

## Limitações conhecidas

- Renderiza a tabela inteira de uma vez, sem virtualização: JSONs muito grandes (dezenas de milhares de linhas) deixam a página lenta.
- O achatamento para em 3 níveis de profundidade; estruturas mais fundas viram string JSON na célula.
- Valores que representam um **instante** (ISO com `Z`/offset, ou timestamp Unix) são convertidos para o fuso do navegador — que é o comportamento desejado, mas significa que a mesma linha mostra horas diferentes em máquinas de fusos diferentes. O fuso usado aparece no `title` da célula. Datas puras (`2026-08-11`) não sofrem conversão nenhuma.
- Na exportação CSV, um valor de texto começando com `=`, `+`, `-`, `@`, tab ou CR recebe um apóstrofo (`'`) na frente, para que o Excel não o execute como fórmula. Números negativos são preservados como número.

## Stack

HTML + CSS + JavaScript puro, num arquivo só. A única requisição externa é a fonte (Google Fonts) — sem ela o layout continua funcionando, só muda a tipografia.
