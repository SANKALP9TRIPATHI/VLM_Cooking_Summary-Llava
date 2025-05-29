# 🍲 Recipe Summarizer

⚡ Objective: 
Given a food image and a noisy or vague dish title, the model generates a concise 2–3 step summary of the cooking instructions that captures the essence of the dish.

🌐 Overview:
Use of the Llava v1.6 Mistral-7b vision-language model (VLM) to generate concise 2–3 step cooking summaries from detailed recipe instructions, supported by an image and a vague or noisy title.
The system implements a few-shot prompting strategy by feeding the model:
•	Several annotated examples (training_data)
•	A new test recipe (test_data) and asking it to summarize the essential steps in a short, clear format.

💡 Pipeline Abstract:
Input:
•	Recipe title (often vague, e.g., “round meat things” for meatballs)
•	Full detailed cooking instructions and manual summary.
•	Corresponding image.
•	The dataset was collected by web scraping recipes from the AllRecipes website, gathering detailed cooking instructions, actual titles, and accompanying food images.
Model:
•	llava-hf/llava-v1.6-mistral-7b-hf
A multimodal large language model capable of visual and text reasoning.
Prompting Approach:
•	Uses few-shot examples embedded in a system prompt to guide the summarization style.
Output:
•	A clear, concise, 2–3 step cooking summary.


💬 Approach:
Model: Llava v1.6 Mistral-7b:
•	Llava is explicitly designed for joint vision and language reasoning. It can process both the image and text together, allowing it to disambiguate vague titles (like “round meat things”) by grounding them in the visual context. This is something text only models or weaker VLMs struggle with.
•	At 7 billion parameters, the Mistral-based Llava is smaller and faster than giant models but still delivers high-quality multimodal outputs. This makes it computationally efficient for testing and, avoiding the cost and slowness of heavier alternatives.
•	Compared to earlier models (including BLIP, which I initially tested), Llava showed a clearer grasp of prompt patterns and could reliably follow a summarization style when given proper prompting. BLIP, while decent for image captioning or retrieval, often produced generic or incomplete summaries when asked to condense full recipes it lacked the refined instruction-following and multitask capabilities that Llava offers.
•	Llava’s open-source nature gave flexibility and transparency, without needing fine-tuning. It allowed a plug and play setup using few-shot prompting, steering outputs by example rather than retraining.
Few-shot Prompting:
•	Few-shot prompting needs zero additional training, just carefully crafted prompts with inlined examples.
•	The summarization task has a specific target: short, action-focused, clear steps. By providing annotated examples in the prompt, I could guide the model to mimic the desired output format (concise 2–3 step summaries) without any weight updates.
•	Few-shot setups let me swap in new examples or adjust the prompt on the fly, testing how the model adapts to different types of recipes (soups, drinks, baked goods) without waiting for re-training cycles.
•	I could experiment across models (including BLIP and Llava) using similar prompt frameworks. This ultimately revealed Llava’s superior performance, especially when BLIP’s outputs remained shallow, vague, or inconsistent even under carefully curated prompts.


📊 Analysis of Performance:
✅ Rights:
•	For recipes like grilled chicken or smoothie, the model accurately condensed the core preparation steps (marinate + grill, blend + adjust).
•	With the given visual context, Llava could correctly map vague titles like “round meat things” to meatballs, using the image to ground its reasoning.
•	The generated summaries usually maintained clear, action-ready steps rather than abstract descriptions or verbose answers..
❌ Wrongs:
•	In some cases, the model compressed too aggressively, omitting key sub-steps like preheating ovens or letting dishes rest.
•	And in some cases where the tile was very vague like “creamy rice”  the model relied more on the full instructions and comparably lengthy summaries were observed.
•	For less vague titles like “warm bowl stuff”, visual cues were underused.

📁 Project Structure
/images/                      → Folder containing all recipe and food images
README.md                     → Project overview and documentation
VLM_Cooking_Summary_v13.ipynb → Main Jupyter Notebook with code, experiments, and results
dataset.json                  → Collected recipe dataset (web scraped) in JSON format
recipe_summary_results.json   → Model-generated cooking summaries (output results)
requirements.txt              → Python dependencies list for setting up the environment


