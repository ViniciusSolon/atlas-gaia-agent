# Atlas - agente para o benchmark GAIA

Projeto final do [AI Agents Course](https://huggingface.co/learn/agents-course) da Hugging Face.

Criei um agente que responde perguntas do [GAIA](https://huggingface.co/papers/2311.12983), um benchmark com perguntas que são fáceis para pessoas, mas difíceis para modelos de IA sozinhos. Para responder, o agente precisa pesquisar na internet, ler arquivos (planilhas, áudios, imagens) e fazer alguns passos de raciocínio.

## Como funciona

O agente repete um ciclo até achar a resposta:

1. **Pensa** no próximo passo
2. **Age**: escreve um código Python que usa uma ferramenta
3. **Observa** o resultado

Ferramentas que ele usa:

- busca na web (DuckDuckGo)
- abrir páginas da internet
- busca na Wikipedia
- transcrição de áudio (Whisper)
- análise de imagens (GPT-4o)
- Python, para contas e planilhas

## Tecnologias

- Python
- [smolagents](https://github.com/huggingface/smolagents)
- OpenAI (GPT-4o e Whisper)
- pandas

## Problemas que resolvi

- A Wikipedia bloqueava o agente, então criei minha própria ferramenta para abrir páginas.
- A API do curso não entregava alguns arquivos, então adicionei um download alternativo pelo dataset do GAIA.
- O agente inventava respostas quando faltava informação, então ajustei o prompt.
- A resposta precisa ser exatamente igual ao gabarito, então criei uma função para limpar o formato.

## Como rodar

```bash
pip install -r requirements.txt
jupyter notebook atlas_gaia_agent.ipynb
```

Você vai precisar de uma chave da OpenAI (`OPENAI_API_KEY`). O `HF_TOKEN` é opcional.
