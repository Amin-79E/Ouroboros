# Python Debugging LLM Fine-Tuning Experiment

 A small experiment fine-tuning **Qwen2.5-Coder-7B-Instruct** to act as a Python debugging assistant.

 ## Goal

 Test whether fine-tuning could improve the model's ability to:

 - Identify Python bugs
- Generate corrected code
- Explain fixes
- Follow coding requirements
- Produce clean, copy-pasteable Python

 ## Evaluation

 The fine-tuned model was compared against the original model using a small custom benchmark covering:

 - Syntax errors
- Logic errors
- Edge cases
- Python-specific bugs
- Requirement-based fixes
- Correct code that should **not** be modified

 ### Results

 The fine-tuned model showed **modest overall improvement**, rather than a dramatic capability increase.

 The most notable improvement was on **requirement-based debugging**, where the fine-tuned model correctly handled requirements such as finding the second-largest **distinct** number and handling edge cases.

 The experiment also revealed weaknesses in the fine-tune, including a tendency to assume correct code contained bugs and initially poor code formatting.

 A stronger system prompt significantly improved the formatting and consistency without additional training.

 ## Takeaway

 Fine-tuning did not make the model dramatically better at basic Python debugging, but it **changed its behavior in useful ways** and highlighted where better training data could produce further improvements.

 ## Model

 - **Base:** Qwen2.5-Coder-7B-Instruct
- **Quantization:** Q4\_K\_M
- **Use case:** Python debugging
- **Inference:** Ollama / GGUF

 > This project is primarily an experiment in fine-tuning, evaluation, and analyzing LLM behavior.
