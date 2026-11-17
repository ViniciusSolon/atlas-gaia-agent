# 🧭 Atlas: agente de IA para o benchmark GAIA

> Agente autônomo que resolve perguntas do mundo real combinando o raciocínio de um LLM com ferramentas: busca na web, Wikipedia, execução de Python, transcrição de áudio e visão computacional.

Projeto final do [Hugging Face AI Agents Course](https://huggingface.co/learn/agents-course), avaliado em 20 perguntas de nível 1 do benchmark [GAIA](https://huggingface.co/papers/2311.12983).

**Resultado:** `XX%` de acerto (meta do curso: 30%)  <!-- troque XX pela sua nota -->

---

## O desafio

O GAIA tem perguntas **simples para humanos** (cerca de 92% de acerto), mas **difíceis para IA**: o GPT-4 com plugins acerta cerca de 15%. Para responder, é preciso:

- pesquisar e cruzar informações de várias fontes;
- ler anexos (planilhas, áudios, imagens, código);
- calcular e raciocinar em vários passos;
- responder **exatamente** no formato pedido, porque a correção é por *exact match*.

Exemplos de perguntas resolvidas:
- *"Quantos álbuns de estúdio a Mercedes Sosa lançou entre 2000 e 2009?"* (Wikipedia + contagem)
- *"Dada esta tabela de operação, quais elementos provam que \* não é comutativa?"* (Python)
- *".rewsna eht sa "tfel" drow eht fo etisoppo eht etirw"* (frase invertida, resolvida com `[::-1]`)

## Como funciona

```
Pergunta + anexo
      │
      ▼
┌─────────────────────────────── Atlas (smolagents CodeAgent) ──┐
│  Thought      → o gpt-4o raciocina sobre o próximo passo     │
│  Action       → escreve código Python que chama ferramentas   │
│  Observation  → o resultado volta para o modelo               │
│  ... repete até chamar final_answer()                         │
└───────────────────────────────────────────────────────────────┘
      │
      ▼
Resposta normalizada para exact match
```

| Ferramenta | Função |
|---|---|
| `web_search` | Busca no DuckDuckGo |
| `visit_webpage` | Abre páginas com `User-Agent` próprio e converte o HTML em markdown |
| `wikipedia_search` | Consulta direta à Wikipedia |
| `transcribe_audio` | Transcreve `.mp3` com o Whisper |
| `analyze_image` | Responde perguntas sobre imagens com a visão do GPT-4o |
| Sandbox Python | `pandas`, `numpy`, `re`, `math`... para cálculos e planilhas |

## Stack

- **[smolagents](https://github.com/huggingface/smolagents)**: framework de agentes da Hugging Face (`CodeAgent`)
- **OpenAI** `gpt-4o` (raciocínio e visão) e `whisper-1` (áudio)
- **huggingface_hub**: download de anexos do dataset oficial do GAIA
- **pandas / openpyxl**: leitura de planilhas
- **Gradio**: interface da versão publicada no Hugging Face Spaces

## Desafios resolvidos

| Problema | Solução |
|---|---|
| Wikipedia bloqueava o agente (HTTP 403) | Ferramenta de navegação própria com `User-Agent` identificado |
| API do curso devolvia 404 nos anexos | Plano B: download do dataset `gaia-benchmark/GAIA` |
| O agente inventava dados quando faltava o arquivo | Regras explícitas no prompt e aviso de arquivo indisponível |
| Respostas como `['a', 'b']` reprovavam no exact match | Normalização para `a, b` |
| Erros de *parsing* desperdiçavam passos | `code_block_tags="markdown"` |
| Requisições travavam | Timeout e retries no cliente da OpenAI |

## Como rodar

```bash
pip install -r requirements.txt
```

```bash
jupyter notebook atlas_gaia_agent.ipynb
```

Você vai precisar de:
- `OPENAI_API_KEY` (obrigatória). Uma rodada completa custa menos de US$ 1.
- `HF_TOKEN` (opcional), com os termos do dataset [GAIA](https://huggingface.co/datasets/gaia-benchmark/GAIA) aceitos.

As chaves são pedidas pelo notebook com `getpass` e nunca ficam salvas no arquivo.

## Conceitos aplicados

- **Agente = LLM + ferramentas**: o modelo decide, as ferramentas executam.
- **ReAct**: raciocínio e ação intercalados, com adaptação a cada observação.
- **Code actions vs. JSON**: código permite loops, variáveis e várias ferramentas num único passo.
- **Multimodalidade**: texto, imagem e áudio no mesmo fluxo.
- **Prompt engineering para avaliação automática**: saída rígida para exact match.

## Próximos passos

- [ ] Ferramenta para vídeos do YouTube
- [ ] Arquitetura multi-agente (gerente + pesquisador + leitor de arquivos)
- [ ] Motor de xadrez para perguntas de tabuleiro
- [ ] API de busca mais estável (Tavily/Serper)

---

Feito durante o [AI Agents Course](https://huggingface.co/learn/agents-course) da Hugging Face 🤗
