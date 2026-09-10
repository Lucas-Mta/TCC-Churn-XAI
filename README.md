# TCC II — Predição de Churn com Machine Learning e IA Explicável

Lucas de Jesus Mota Ferreira — FT/UNICAMP
Orientadora: Profa. Dra. Ana Estela Antunes da Silva

Implementação do pipeline descrito no Capítulo 3 da monografia.

---

## Estrutura do projeto

```
TCC_Churn_XAI/
├── data/                          # base de dados (bruta e processada)
│   ├── telco_churn.csv            # baixado automaticamente pelo Notebook 01
│   └── dados_processados.pkl      # gerado pelo Notebook 02
├── notebooks/
│   ├── 01_eda.ipynb               # Seção 3.3.1
│   ├── 02_preprocessamento.ipynb  # Seção 3.3
│   ├── 03_modelagem.ipynb         # Seção 3.4
│   └── 04_explicabilidade.ipynb   # Seção 3.5
├── outputs/
│   ├── figuras/                   # PNG 300 dpi, prontos para o LaTeX
│   ├── tabelas/                   # CSV, prontos para virar tabelas no TCC
│   └── modelos/                   # modelos treinados serializados
├── requirements.txt
└── README.md
```

---

## Configuração do ambiente (uma única vez)

### 1. Abrir o projeto no VS Code

`File > Open Folder` e selecione a pasta `TCC_Churn_XAI`.

### 2. Criar o ambiente virtual

Abra o terminal integrado (`Ctrl + '`) e execute:

**Windows (PowerShell)**
```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

**macOS / Linux**
```bash
python3 -m venv venv
source venv/bin/activate
```

O prompt deve passar a exibir `(venv)` no início da linha.

> Se o PowerShell bloquear a ativação, rode uma vez:
> `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`

### 3. Instalar as dependências

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Leva de 3 a 5 minutos.

### 4. Extensões do VS Code

Instale, pela aba de extensões (`Ctrl + Shift + X`):
- **Python** (Microsoft)
- **Jupyter** (Microsoft)

### 5. Selecionar o kernel

Abra qualquer notebook, clique em **Select Kernel** no canto superior direito e escolha o
interpretador do `venv`.

---

## Ordem de execução

Os notebooks são **sequenciais e dependentes**. Execute na ordem.

| # | Notebook | O que faz | Tempo aprox. |
|---|----------|-----------|--------------|
| 01 | `01_eda.ipynb` | Carrega a base, diagnostica, gera 7 figuras e as hipóteses iniciais | 2 min |
| 02 | `02_preprocessamento.ipynb` | Nulos, encoding, split 80/20, normalização, SMOTE | 1 min |
| 03 | `03_modelagem.ipynb` | Treina os 3 algoritmos, Grid Search, limiar, McNemar | 10–40 min |
| 04 | `04_explicabilidade.ipynb` | SHAP global + LIME local + síntese | 5–15 min |

Para rodar tudo de um notebook: `Run All` na barra superior.

---

## Dica importante sobre o Notebook 03

A variável `GRID_REDUZIDO = True` está ativa por padrão. Ela roda uma busca enxuta
(poucos minutos) para você validar que o fluxo funciona de ponta a ponta.

Quando estiver tudo certo, troque para `GRID_REDUZIDO = False` e rode a busca completa —
que é a que corresponde à Tabela 3.2 da monografia e cujos resultados devem ir para o
Capítulo 4.

---

## Rastreabilidade com a monografia

Cada notebook referencia explicitamente a seção correspondente do Capítulo 3. Se você
alterar alguma decisão metodológica no código, atualize também o texto — e vice-versa.
Essa consistência é um dos pontos que a banca verifica.

## Reprodutibilidade

`random_state = 42` está fixado em todas as etapas estocásticas: divisão treino/teste,
SMOTE, treinamento dos modelos e amostragem do LIME. Rodar duas vezes produz exatamente
os mesmos resultados.

## Aviso sobre vazamento de dados

A ordem do Notebook 02 é deliberada: **split → normalização → SMOTE**. Nunca normalize ou
aplique SMOTE antes de dividir treino e teste. Isso produziria métricas infladas e
invalidaria toda a avaliação — exatamente o erro que a Seção 3.3.6 se compromete a evitar.

---

## Aproveitando as saídas no LaTeX

**Figuras:** salvas em 300 dpi. Copie da pasta `outputs/figuras/` para a pasta `figuras/`
do Overleaf e insira com:

```latex
\begin{figure}[h!]
    \centering
    \includegraphics[width=0.85\textwidth]{shap_summary_plot.png}
    \caption[SHAP Summary Plot do modelo final]{Contribuição das variáveis para a predição
    de evasão. Fonte: elaborado pelo autor.}
    \label{fig:shapSummary}
\end{figure}
```

**Tabelas:** os CSVs em `outputs/tabelas/` podem ser convertidos para LaTeX com:

```python
print(df.to_latex(index=False, float_format='%.4f'))
```

Lembre-se de adicionar `Fonte: elaborado pelo autor.` em todas as tabelas — foi um dos
apontamentos da orientadora.
