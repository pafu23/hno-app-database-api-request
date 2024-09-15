# hno-app-database-api-request
The code within the Jupyter notebooks consolidates single or multiple-choice questions and their corresponding answer options from designated Excel columns, dispatching them as prompts to various AI models through an API. Subsequently, it documents the prompts and responses across six predetermined columns and assesses answers from A to D, annotating them with a '1' when recognized.
We utilized this code to conduct a research study. Below is a slightly modified excerpt from the methods section of our paper, which is presently submitted for peer review:
# Methods
# Question Database
We utilized the question bank from an online education platform (https://hno.keelearning.de/), which provides quiz-based questions in German to help prepare for the German otolaryngology board certification. We omitted any questions that relied on images. The study included a total of 2,576 questions, which were divided into multiple-choice (479 questions) and single-choice (2,097 questions). Official permission was secured from the copyright owner before beginning the study to use the questions for research purposes.	
# AI models
28,336 questions were entered to all the AI models by interacting with the APIs. The companies offering these APIs for the LLMs  —OpenAI, Google, and Anthropic— provide a range of models, each with distinct capabilities and varying price points.  We utilized the most recent stable versions of the following ChatGPT models: GPT-3.5 Turbo, GPT-4, GPT-4 Turbo, GPT-4o, and GPT-4o mini (https://platform.openai.com/docs/models). We also engaged the latest Gemini models from Google: Gemini 1.0 Pro, Gemini 1.5 Flash, and Gemini 1.5 Pro (https://ai.google.dev/gemini-api/docs/models/gemini). Finally, we incorporated the most recent Claude models: Claude 3 Haiku, Claude 3.5 Sonnet, and Claude 3 Opus (https://docs.anthropic.com/en/docs/about-claude/models).
For each model the default API settings were maintained throughout the study, with the exception of the Gemini models. For these, we adjusted the safety settings to block none for all four harm categories (Hate Speech, Harassment, Sexually Explicit, and Dangerous Content). This adjustment was necessary to ensure the models did not withhold responses to medical questions that might be erroneously classified as potentially harmful. It is important to note that, to our knowledge, the APIs for ChatGPT and Claude do not offer the option to modify harm categories.
# Prompt Design for LLM Interaction
To facilitate a new analysis step using Python code, we tailored the wording of our prompts to explicitly direct the LLMs to refrain from offering additional explanations. This modification was essential as the selected LLMs often attempt to justify why alternative answers were not chosen, which could have complicated our data extraction process. 
We submitted each question to the API only once. To accommodate differences in question formats, we employed two distinct prompts in German when soliciting responses from the corresponding AI model to quiz-style questions with four options. The original German prompts, along with two examples each of multiple-choice and single-choice questions, can be accessed in the Excel sheet or Jupyter notebooks available on this GitHub page.
When addressing single-choice questions, the following prompt was utilized:
(A) “Please answer the following question. Only one answer option is correct. Enter only the correct answer letter. Avoid additional explanations or information:
Single choice question 
(a) Option A
(b) Option B
(c) Option C
(d) Option D”
We used the following prompt for multiple-choice questions:
(B) “Please answer the following question. Several answer options could be correct. Enter only the correct answer letters. Avoid additional explanations or information:
Multiple choice question
(a) Option A
(b) Option B
(c) Option C
(d) Option D”	
# Data Collection and Processing
Using Anaconda, a Python data science platform (Anaconda, Inc., Austin, Texas, USA), we executed our Python code within a Jupyter notebook. Jupyter notebook is an open-source web application that enables users to create and share documents containing live code, equations, visualizations, and narrative text. We operated this application on a Python 3 kernel, an execution environment that interprets Python code. Our Python scripts performed the following tasks sequentially:
i.	Initially, the code imports the necessary Python libraries, which are collections of modules that enhance Python's capabilities without requiring additional code. For example, the library openpyxl allows for handling Excel sheets.
ii.	The script loads the prepared Excel table. Column B indicates whether the entry is a multiple-choice question; if not, it's classified as a single-choice question. Column C contains the question itself, while Columns D through G list the possible answers. Columns H to BU document the created prompt, the response received from the LLM, and the analysis of the answer, as detailed in steps v and vi.
iii.	We defined a configuration variable (model_config) in the script to facilitate switching between different AI sub-models and to specify where data should be stored in the Excel file based on the LLM used.
iv.	The Jupyter notebook features a function that sends prompts to the respective AI model's API and receives responses.
v.	In our Python code, we used the update_response_columns function along with a regular expression (regex) pattern to precisely identify which letters (A to D) the LLMs selected as correct answers. A regex pattern is a sequence of characters that forms a search pattern, often used for string matching or manipulation. By crafting prompts that elicited responses containing only the correct letters, our code could accurately detect and record these responses as binary values (0 for incorrect, 1 for correct).
vi.	The code iterates through rows in the Excel sheet, each representing a question. It determines whether the question is multiple-choice or single-choice, selects the appropriate prompt format, and combines it with the question and answer options. This compiled prompt is then logged in the Excel sheet to facilitate manual validation, sent to the LLM's API, and the raw API response is also stored for potential review.
vii.	Additional functions, such as an API request counter and intermediate data saving, are implemented to prevent errors related to API rate limits and potential data loss. Ultimately, the updated Excel file is saved by overwriting the original file, thereby securing the collected data.
The accuracy of each LLM was evaluated based on its ability to match the answers from the online study platform. For multiple-choice questions, a response was considered correct only if the LLM accurately identified all the correct answers.
Statistical Analysis
We utilized Pearson’s chi-square test to assess differences in question format and categories. The statistical analysis was carried out using SPSS Statistics 25 (IBM, Armonk, NY, USA), and a two-tailed p-value of ≤ 0.05 was used to determine statistical significance.
