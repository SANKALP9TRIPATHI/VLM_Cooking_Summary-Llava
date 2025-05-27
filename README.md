# Recipe Summarizer

Use of the Llava v1.6 Mistral-7b vision-language model (VLM) to generate
concise 2–3 step cooking summaries from detailed recipe instructions,
supported by an image and a vague or noisy title.
The system implements a few-shot prompting strategy by feeding the model:
 Several annotated examples (training_data)
 A new test recipe (test_data) and asking it to summarize the essential
steps in a short, clear format.

Input:
 Recipe title (often vague, e.g., “round meat things” for meatballs)
 Full detailed cooking instructions and manual summary.
 Corresponding image
Model:
 llava-hf/llava-v1.6-mistral-7b-hf
A multimodal large language model capable of visual and text
reasoning.
Prompting Approach:
 Uses few-shot examples embedded in a system prompt to guide the
summarization style.
Output:
 A clear, concise, 2–3 step cooking summary.
