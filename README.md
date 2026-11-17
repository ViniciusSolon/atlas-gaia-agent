# Atlas - an agent for the GAIA benchmark

Final project for the Hugging Face [AI Agents Course](https://huggingface.co/learn/agents-course).

I built an agent that answers questions from [GAIA](https://huggingface.co/papers/2311.12983), a benchmark with questions that are easy for people but hard for AI models on their own. To answer them, the agent has to search the web, read files (spreadsheets, audio, images) and go through a few steps of reasoning.

## How it works

The agent repeats a loop until it finds the answer:

1. **Think** about the next step
2. **Act**: write Python code that uses a tool
3. **Observe** the result

Tools it uses:

- web search (DuckDuckGo)
- opening web pages
- Wikipedia search
- audio transcription (Whisper)
- image analysis (GPT-4o)
- Python, for math and spreadsheets

## Tech stack

- Python
- [smolagents](https://github.com/huggingface/smolagents)
- OpenAI (GPT-4o and Whisper)
- pandas

## Problems I solved

- Wikipedia was blocking the agent, so I wrote my own tool to open pages.
- The course API didn't deliver some files, so I added a fallback download from the GAIA dataset.
- The agent made up answers when information was missing, so I adjusted the prompt.
- The answer has to match the expected answer exactly, so I wrote a function to clean up the format.

## How to run

```bash
pip install -r requirements.txt
jupyter notebook atlas_gaia_agent.ipynb
```

You need an OpenAI key (`OPENAI_API_KEY`). The `HF_TOKEN` is optional.
