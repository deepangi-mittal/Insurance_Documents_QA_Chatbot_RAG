# Insurance_Documents_QA_Chatbot_RAG

✨ About the Project<br>
RAG Insurance Assistant is an innovative solution crafted to simplify the process of understanding and extracting information from complex insurance documents. Traditional methods of sifting through policy documents, claim guidelines, and legal jargon can be time-consuming and frustrating for users. This project eliminates these pain points by leveraging Retrieval-Augmented Generation (RAG) technology, which blends advanced information retrieval with the natural language generation capabilities of state-of-the-art AI models. <br>

Example Use Cases:<br>

"What is covered under my health insurance policy?"<br>
"How can I file a claim for vehicle insurance?"<br>



🔍 Key Features<br>
🌟 Precise Information Retrieval: Quickly fetch relevant data from insurance documents using advanced embeddings.<br>
⚡ Natural Language Understanding: Generates clear, concise answers in response to user questions.<br>
🌟 AI-Powered Insights: Extracts answers directly from policy documents using RAG techniques.<br>
⚡ Lightning-Fast Retrieval: Employs ChromaDB to store and query document embeddings along with Caching for rapid responses.<br>
🤖 Advanced Generation: Integrates OpenAI's GPT-4 or Gemini API or any other State-of-the-Art models for context-rich explanations.<br>
📄 Page-Level Chunking: Splits documents into manageable sections for optimal retrieval performance.<br>




🛠️ Tech Stack<br>
Language: Python-In Jupyter Notebook
Frameworks/Libraries: ChromaDB, PDFplumber<br>
APIs/Models: OpenAI's GPT-4/ GPT-4o/ GPT-4o-mini or Gemini API or any other State-of-the-Art models<br>





🛠️ Challenges/Issues Faced with fixes <br>
[Issue #1](For Preprocessing PDF file, many tools like PDFminer, PyPDF2 etc was tried, but they were not suitable for the task. PDFplumber was finally chosen.)<br>

[Issue #2](Extracting Tables from PDF was also a challenge. Whole data processing logic was reworked with PdfPlumber to extract the data from tables in readable format and then appended in the correct sequence.)<br>

[Issue #3](Cache layer was added in ChromaDB to prevent re-embedding of the data. This was done to avoid overloading the ChromaDB server with data and to make the retrieval process more efficient.)<br>

[Issue #4](Cross Encoder based Reranker was added to better select the most relevant passages from the document. This was done to improve the quality of the answers to the user queries.)<br>
[Issue #5](System Prompt was fully reworked to include few shot examples and a few instructions to guide the model to generate more relevant answers to the user queries. This was done to improve the quality of the answers to the user queries.)
<br>


**Project Approach**<br>
The project is built using three layers namely:<br>

Embedding Layer<br>
Search layer<br>
Generation Layer<br>
**Embedding Layer**<br>
Here the PDF document needs to be effectively processed, cleaned, and chunked for the embeddings. We have used the pdfplumber library to extract text/tables etc from multiple pdf’s In the pdf pre-processing we have removed the pages where text length is less than 10. The chunking strategy used is the Page-wise chunking as we can observe that mostly the pages contain few hundred words, maximum going upto 1000. So, we don't need to chunk the documents further; we can perform the embeddings on individual pages. This strategy makes sense for 2 reasons:<br>

The way insurance documents are generally structured, we will not have a lot of extraneous information in a page, and all the text pieces in that page will likely be interrelated.<br>
We want to have larger chunk sizes to be able to pass appropriate context to the LLM during the generation layer.<br>
**Search Layer**<br>
Here we have followed the following steps:<br>

Generate and Store Embeddings using OpenAI and ChromaDB<br>
We have embedded the pages in the dataframe through OpenAI's text-embeddingada-002 model, and store them in a ChromaDB collection.<br>
We have implemented the cache mechanism for faster retrieval of queries<br>
We have implemented the re-ranking block which enhances the quality of search results. Re-ranking the results obtained from our semantic search can sometime significantly improve the relevance of the retrieved results. This is often done by passing the query paired with each of the retrieved responses into a cross-encoder to score the relevance of the response w.r.t. the query.<br>
We have used the 'cross-encoder/ms-marco-MiniLM-L-12-v2' cross encoder from sentence-transformer library<br>
**Generation Layer**<br>


Once we have the final top search results, we can pass it to an GPT 3.5 along with the user query and a well-engineered prompt, to generate a direct answer to the query along with citations, rather than returning whole pages/chunks.
We have written an exhaustive prompt with clear instructions and passed the user query and top searched results to the LLM and generated a response.

