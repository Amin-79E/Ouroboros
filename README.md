# Ouroboros: a local Python tutor

A single-file chat interface for a **fine-tuned Python debugging model**, running fully on your own machine through [Ollama](https://ollama.com). No server and no API key. Your conversations stay on your machine (the page only loads two fonts from Google Fonts).

<!-- Add a screenshot or short GIF here: ![Ouroboros UI](docs/screenshot.png) -->

## What it does

Paste your code and your traceback, and Ouroboros explains the cause of the problem before showing a fix.

- Streams answers token by token from a local model
- Syntax-highlighted code blocks with a **Copy** button
- Chat history saved in your browser (`localStorage`), with a history panel and new-chat button
- Model picker populated from your installed Ollama models
- Settings for Ollama address, temperature, context size and system prompt
- Tutor behavior set by the system prompt: explain the cause first, ask for the full code and traceback, say so when code is already correct, reply in the student's language

## Run it

1. Install [Ollama](https://ollama.com).
2. Download the model file (GGUF) from [Google Drive](https://drive.google.com/drive/folders/1EM6PbvfSRyj3IZQyU9VpYBPedHKP3cHi?usp=drive_link) into a new folder.
3. In that folder, create a file named `Modelfile` containing one line (use the exact name of the downloaded file):

   ```
   FROM ./your-model-file.Q4_K_M.gguf
   ```

   then register the model:

   ```
   ollama create Ouroboros -f Modelfile
   ```

   Other ChatML-format models (such as Qwen) also work; pick them from the dropdown.
4. Serve the page from localhost (opening the file directly will not reach Ollama):

   ```
   python -m http.server 8000
   ```

5. Open `http://localhost:8000/Interface.html`.

If the status dot is red, check that Ollama is running and that the address in Settings is `http://localhost:11434`.

## The model: fine-tuning experiment

The tutor runs on **Qwen2.5-Coder-7B-Instruct**, fine-tuned as a Python debugging assistant. The goal was to test whether fine-tuning improves the model's ability to:

- Identify Python bugs
- Generate corrected code
- Explain fixes
- Follow coding requirements
- Produce clean, copy-pasteable Python

### Evaluation

The fine-tuned model was compared with the original on a small custom benchmark covering syntax errors, logic errors, edge cases, Python-specific bugs, requirement-based fixes, and correct code that should **not** be modified.

### Results

- **Modest overall improvement**, not a dramatic jump.
- The biggest gain was on **requirement-based debugging**, such as finding the second-largest *distinct* number and handling edge cases.
- Weaknesses found: a tendency to assume correct code contains bugs, and poor initial code formatting.
- A stronger system prompt fixed most of the formatting and consistency problems **without extra training**. The interface includes an editable system prompt in Settings.

### Takeaway

Fine-tuning did not make the model dramatically better at basic Python debugging, but it **changed its behavior in useful ways** and showed where better training data could help next.

| | |
|---|---|
| **Base model** | Qwen2.5-Coder-7B-Instruct |
| **Quantization** | Q4_K_M (GGUF) |
| **Inference** | Ollama |
| **Use case** | Python debugging and tutoring |

## Limitations

- The model cannot run code. It reasons only from the code and output you give it.
- Code highlighting is Python-only, and the Markdown rendering is basic (paragraphs, lists, bold, inline code, code blocks).
- 7B models make mistakes. Treat answers as a tutor's hint, not ground truth.
- The benchmark is small and custom, so the results are indicative, not conclusive.

> This project is primarily an experiment in fine-tuning, evaluation, and analyzing LLM behavior.
