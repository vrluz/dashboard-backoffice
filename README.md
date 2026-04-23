# dashboard-backoffice

Dashboard gerencial single-file (HTML) para acompanhamento de demandas de Backoffice & Suporte. Processa planilhas exportadas do Sienge ou qualquer sistema que gere CSV/XLSX com colunas de chamados.

---

## Como funciona

O dashboard é um único arquivo `index.html` — sem backend, sem build, sem dependências locais. Todas as bibliotecas são carregadas via CDN.

Ao abrir o arquivo, ele tenta buscar automaticamente o arquivo `dados.csv` deste repositório. Se não encontrar (sem conexão, arquivo ausente ou URL incorreta), exibe o botão de upload manual.

---

## Estrutura do repositório

```
dashboard-backoffice/
├── index.html   # Dashboard completo
├── dados.csv    # Planilha de dados (atualizada conforme necessário)
└── README.md
```

---

## Configurando a URL do CSV

Dentro do `index.html`, localize a constante no início do bloco `<script>`:

```js
const GITHUB_CSV_URL = 'https://raw.githubusercontent.com/SEU_USUARIO/SEU_REPOSITORIO/main/dados.csv';
```

Substitua pelo caminho raw do seu repositório. Exemplo:

```js
const GITHUB_CSV_URL = 'https://raw.githubusercontent.com/empresa/dashboard-backoffice/main/dados.csv';
```

> **Atenção:** use sempre o link `raw.githubusercontent.com`, não a URL da página do arquivo no GitHub.

---

## Atualizando os dados

1. Exporte o relatório do Sienge (ou sistema de origem) como `.csv`
2. Renomeie o arquivo para `dados.csv`
3. Faça commit/push na branch `main` substituindo o arquivo anterior
4. Recarregue o dashboard no navegador — o botão **Atualizar** no canto superior direito rebusca o arquivo sem precisar reabrir a página

---

## Formato esperado do CSV

O dashboard detecta colunas automaticamente por similaridade de nome. As colunas abaixo são reconhecidas:

| Campo | Nomes aceitos na coluna |
|---|---|
| Assunto / Título | `assunto`, `titulo`, `title`, `demanda` |
| Responsável | `agente`, `responsavel`, `atribuido`, `assignee` |
| Área / Grupo | `area`, `grupo` |
| Prioridade | `prioridade`, `priority` |
| Status | `status`, `estado`, `fase` |
| Solicitante | `solicitante`, `requester`, `autor` |
| Data de abertura | `created`, `abertura`, `criacao`, `criado` |
| Data de conclusão | `resolved`, `conclusao`, `fechamento`, `resolvido` |

Formatos de data aceitos: `DD/MM/YYYY`, `YYYY-MM-DD`, `DD/MM/YYYY HH:MM:SS`.

O delimitador pode ser `,` ou `;` — o PapaParse detecta automaticamente.

---

## Regras de negócio

- **SLA e tempo de ciclo** calculados exclusivamente em **dias úteis (Seg–Sex)**, considerando jornada de **10h/dia**
- Fins de semana são ignorados em todos os cálculos de prazo
- A exportação de backlog gera CSV com delimitador `;` e BOM UTF-8 para leitura nativa no Excel PT-BR

---

## Editando o Roadmap

O conteúdo da aba **Roadmap & Projetos** não vem do CSV — é configurado diretamente no código, no objeto `configRoadmap` no início do `<script>`:

```js
const configRoadmap = {
    destaquesIA: [ ... ],
    equipeEPlanejamento: [ ... ],
    governanca: [ ... ],
    roadmapQuarters: {
        "Q1": [ ... ],
        "Q2": [ ... ],
        ...
    }
};
```

Basta editar os textos e status dos itens (`done`, `active`, `pending`, `backlog`) e salvar o arquivo.

---

## Dependências (CDN)

| Biblioteca | Versão | Uso |
|---|---|---|
| Chart.js | latest | Gráficos |
| chartjs-plugin-datalabels | 2.2.0 | Labels nos gráficos |
| PapaParse | 5.4.1 | Parse de CSV |
| SheetJS (XLSX) | 0.18.5 | Leitura de `.xlsx` |
| Google Fonts (DM Sans / DM Mono) | — | Tipografia |
