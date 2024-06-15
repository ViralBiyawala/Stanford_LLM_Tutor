# Stanford LLM Tutor | AI Bot

## Project Overview

This repository contains the implementation of an AI bot built using a transformer model (`gpt2`) from Hugging Face. The chatbot leverages FAISS for vector database storage to efficiently match user queries with relevant data. The data used for training and response generation was scraped from the official Stanford LLM course.

## Features

- **Language Model**: Utilizes the `gpt2` model from Hugging Face.
- **Vector Database**: Implements FAISS to store and retrieve keys efficiently.
- **Data Sources**: Scraped from various lectures of the Stanford LLM course.
- **Content Types**: Handles various content types including paragraphs, tables, equations, links, ordered lists, and unordered lists.
- **Query Matching**: Matches user queries to the top 2 closest keys using FAISS and constructs a prompt with the retrieved data.

## How It Works

1. **Data Scraping**: The data is scraped from various lectures of the Stanford LLM course. The `h2`, `h3`, and `<strong>` tags serve as keys, and the corresponding content is categorized into paragraphs, tables, links, equations, ordered lists, and unordered lists.

2. **Vector Database (FAISS)**: The keys are stored in a FAISS vector database using L2 distance for efficient retrieval. When a user query is received, FAISS finds the top 2 closest matching keys based on vector similarity.

3. **Prompt Generation**: The chatbot constructs a structured prompt using the data retrieved from FAISS. This prompt includes paragraphs, tables, equations, links, ordered lists, and unordered lists as relevant to the matched keys.

4. **Response Generation**: The constructed prompt is fed into the GPT-2 model to generate a coherent and relevant response to the user query.

## Data Schema

The data scraped from the Stanford LLM course lectures have the following schema:

```
key1:{
  {
      'paragraphs': [],
      'tables': [],
      'links': [],
      'equations': [],
      'ordered_lists': [],
      'unordered_lists': []
  } }
key2:{
  {
      'paragraphs': [],
      'tables': [],
      'links': [],
      'equations': [],
      'ordered_lists': [],
      'unordered_lists': []
  } }
```

Each key corresponds to `h2`, `h3`, or `<strong>` tags from the lecture pages. The data associated with each key includes paragraphs, tables, links, equations, ordered lists, and unordered lists if they exist.

## Example Usage

1. **User Query**: "What are Benefits and Harms?"

2. **FAISS Retrieval**: The query is matched to the top 2 closest keys in the vector database using L2 distance.

3. **Prompt Construction**:
    ```
    # Create a structured prompt
    prompt = f"**Question:** {query}\n\n"
    
    # Add top 2 matched sections
    prompt += f"**Sections:**\n- {result_key1}\n- {result_key2}\n\n"
    
    # Add content to the prompt
    for result_key, result_content in [(result_key1, result_content1), (result_key2, result_content2)]:
        if result_content.get('paragraphs'):
            prompt += "**Paragraphs:**\n" + "\n".join(result_content['paragraphs']) + "\n\n"
        if result_content.get('ordered_lists'):
            prompt += "**Ordered Lists:**\n" + "\n".join(["\n".join(ol) for ol in result_content['ordered_lists']]) + "\n\n"
        if result_content.get('unordered_lists'):
            prompt += "**Unordered Lists:**\n" + "\n".join(["\n".join(ul) for ul in result_content['unordered_lists']]) + "\n\n"
        if result_content.get('tables'):
            prompt += "**Tables:**\n" + "\n".join(["\n".join(table) for table in result_content['tables']]) + "\n\n"
        if result_content.get('links'):
            prompt += "**Links:**\n" + "\n".join(result_content['links']) + "\n\n"
        if result_content.get('equations'):
            prompt += "**Equations:**\n" + "\n".join(result_content['equations']) + "\n\n"
    
    # Add a closing statement
    prompt += "Answer is :"
    
    # Define max_length
    max_length = min(len(prompt) + 100, 750)
    
    # Generate response
    response = generator(prompt[:750], max_length=max_length, num_return_sequences=1, truncation=True, pad_token_id=50256)
    ```

4. **Generated Response**: The GPT-2 model uses the prompt to generate a detailed response.

## Example Screenshot

![Example Screenshot](example_screenshotv2.png)

This screenshot shows an example interaction where the chatbot responds to a user query about the basics of LLMs.

## Training Details

- **Platform**: Kaggle
- **Hardware**: CPU

The model was trained on Kaggle using CPU resources.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or new features.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgements

- [Hugging Face](https://huggingface.co/)
- [FAISS](https://github.com/facebookresearch/faiss)
- [Stanford LLM Course](https://stanford-cs324.github.io/winter2022/)
