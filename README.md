# Q&A Chatbot README
 **Club**: Northwestern AI Club | Fall 2025<br>
 **Author**: Raymond Gu

## Project Structure
This project is meant to be a light introduction to important NLP concepts and resources for Northwestern's AI Club's Onboarding Process. In this project, students can build local LLM-powered RAG chatbots using Hugging Face tools. To guide students through this project, I wrote modules to help students connect model behavior to underlying concepts like sampling strategies and how fine-tuning alters a model's reasoning style. The general structure for this project is shown below:<br>

<table cellpadding="8" border="1">
  <tr>
    <th>Modules</th>
    <th>Topics That’ll Be Covered</th>
  </tr>
  <tr>
    <td><b>Module 2</b></td>
    <td>
      • RAG (Retrieval-Augmented Generation)<br>
      • Search Engine Setup
    </td>
  </tr>
  <tr>
    <td><b>Module 3</b></td>
    <td>
      • Hugging Face Account Setup<br>
      • Pre-Training and Fine-Tuning Models<br>
      • Autoencoders (Embedding Models)<br>
      • Cosine Similarity<br>
      • Webscraping
    </td>
  </tr>
  <tr>
    <td><b>Module 4</b></td>
    <td>
      • Instruction-Tuning Models<br>
      • Observing LLM Generation Behavior<br>
      • Coding Stopping Conditions
    </td>
  </tr>
</table>

## Retrieval Pipeline
For this project, students build a Q&A chatbot to answer questions about a specific topic of their choice (a sports team, favorite anime, etc.) by retrieving relevant information from the web and generating answer using a local LLM.<br>

### <u>Search Engine Setup</u>
For this project, students will be using `DuckDuckGo` as their search engine. When using DuckDuckGo's API, there are many quality-of-life features that services like Google typically have. To account for this, there are several techniques used to get more consistent results.<br>

`Context Prefix`: By putting a context prefix at the beginning of the query, we can push the search engine towards more relevant results.<br>

* **Before**: What happened to the comet? What happened to mitsuha and itomori?
* **After**: *[Your Name anime]* What happened to the comet? What happened to mitsuha and itomori?

`Clarifiers`: DuckDuckGo's pattern matching can be problematic if a term is ambiguous. For example, "Apple" can refer to the fruit or the company. To help clarify, we can expand ambiguous terms like "Apple" in the query to "Apple fruit" to be more specific.<br>

* **Before**: What happened to the comet? What happened to mitsuha and itomori?
* **After**: What happened to the comet *Tiamat*? What happened to Mitsuha *Miyamizu* and Itomori *town*?

`Trusted Domains`: Even after refining the query, DuckDuckGo can still produce irrelevant results. To make sure we use the best information, we can prioritize trusted domains and then look at other sources.<br>

### <u>Scraping & Formatting Data</u>
After retrieving links to the most relevant webpages from DuckDuckGo's API, we need to extract the content from those webpages to feed into the LLM. Code is provided for students to use, it is recommended for students to understand how this code works incase they need to modify it (since every website is formatted differently).<br>

* **Note**: The function `scrape_webpages` uses 2 methods to scrape content from webpages. It first attempts to scrape content using `newspaper3k`. If that doesn't work, it falls back to using `Requests` + `BeautifulSoup`.

After scaping information from the most relevant webpages, we need a way to extract the most useful information for answering the user's question from those webpages. This is done in 2 steps:

* **Separate By Paragraphs**: First, the content from each webpage is separate by paragraph.
* **Rank By Cosine Similarity**: Then, the cosine similarity between the question and each paragraph is calculated to determine which paragraphs are most relevant.

## Generation
As stated previously, this project uses local LLMs from Hugging Face to generate answers for user questions. Since we aren't just calling an API, there are several factors that students need to consider.

### <u>Model Selection</u>
Students are advised to do research on different models provided by Hugging Face in order to gain a richer understanding of how models work. The starter code uses `Qwen2.5-1.5B-Instruct` by default. Here are some general guidelines to consider when choosing a model:<br>

`Model Size`: Each model on Hugging Face generally has multiple versions of varying sizes. While larger models are generally more capable than smaller models, they are also much more expensive to run. Since this project uses Colab, I'd generally recommend using models that have around 3 billion parameters.<br>

`Type Of Model`: Not all models on Hugging Face are designed for human dialogue, meaning they might not act like ChatGPT and be optimized for interactive conversations.<br>

`Instruction-Tuned Models`: Instruction-tuned models are trained on datasets that look like instructions + responses. This help models follow user instructions and generate more useful responses. This is the difference between a model that just writes text and a model that acts like a useful assistant.<br>

### <u>Model Behavior</u>
When working with Hugging Face models, there are several parameters that students should play around with:

* `Max_New_Tokens`: As the name suggests, this is the maximum number of tokens the model can generate. If this number is too large, the model may not know when to stop and might end up taking 20 minutes to generate a single response.
* `Temperature`: When determining the next token, the model rolls a dice using the probability distribution. This means a high-probability token is still more likely, but lower-probability ones can still be chosen. Different values of temperature affect the probability distribution to adjust consistency and variety.


Even after tweaking some of the model's parameters, it might not output perfect responses. Perhaps it's constantly repeating the same thing over or rambling about unrelated topics after it's answered the quesiton. While there is no set protocol for observing LLM behavior, I've provided what I did to debug model behavior in `Module 4`.


