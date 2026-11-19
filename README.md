# LangChain Chains

This repository is a small **learning project** for understanding how to
compose LangChain runnables into useful LLM workflows. Each Python file
demonstrates a different chain pattern using prompts, chat models, output
parsers, and runnable composition.

The main example, [`conditional_chain.py`](./conditional_chain.py), builds a
sentiment-aware feedback responder:

1. A feedback message is classified as `positive` or `negative`.
2. The classification is validated and parsed into a Pydantic model.
3. `RunnableBranch` selects a response prompt based on the sentiment.
4. The selected prompt is sent to the chat model and converted to text.

This project is intended for experimentation and learning rather than
production use.

## Examples

| File | Concept |
| --- | --- |
| [`simple_chain.py`](./simple_chain.py) | A prompt, model, and output parser connected with the pipe operator |
| [`sequential_chain.py`](./sequential_chain.py) | Passing the output of one LLM step into the next step |
| [`parallel_chain.py`](./parallel_chain.py) | Running independent chains in parallel and merging their results |
| [`conditional_chain.py`](./conditional_chain.py) | Classifying input and choosing a branch conditionally |

## Requirements

- Python 3.10 or newer
- An OpenAI API key
- The Python packages used by the examples:
  - `langchain-core`
  - `langchain-openai`
  - `langchain-anthropic` (used by `parallel_chain.py`)
  - `python-dotenv`
  - `pydantic`

## Setup

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell, activate it with:

```powershell
.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
pip install langchain-core langchain-openai langchain-anthropic python-dotenv pydantic
```

Create a `.env` file in the project root and add your OpenAI key:

```env
OPENAI_API_KEY=your_openai_api_key
```

If you run [`parallel_chain.py`](./parallel_chain.py), also configure an
Anthropic key:

```env
ANTHROPIC_API_KEY=your_anthropic_api_key
```

Do not commit `.env` or expose API keys in source control.

## Running the conditional example

With the virtual environment activated, run:

```bash
python conditional_chain.py
```

The script sends the sample feedback `"This is a beautiful phone"` through
the classifier and prints a generated response for positive feedback. It also
prints an ASCII representation of the chain graph to make the workflow easier
to inspect.

To experiment with the behavior, change the value passed to `feedback` near
the bottom of the file and try both positive and negative messages.

## What this project demonstrates

- Prompt templates with input variables
- LCEL pipe composition (`|`)
- Structured output with `PydanticOutputParser`
- Runtime branching with `RunnableBranch`
- Fallback behavior with `RunnableLambda`
- Visualizing a runnable graph with `get_graph().print_ascii()`

## Learning notes

The examples intentionally keep the code straightforward so that each chain
pattern is easy to isolate. Model responses can vary between runs, and API
usage may incur costs. As a next step, the examples could be extended with
tests, explicit error handling, configurable model names, and command-line
input.
