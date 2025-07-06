# Task 2: RAG Optimization Techniques

I built a RAG system using OpenAI's GPT-3.5-turbo and Pinecone's vector database. The system is designed to process documents and answer questions based on their content. Here's how it works under the hood.

First, it handles different document types - PDFs, Word docs, and plain text files. For PDFs, it uses PyPDF2 to extract text from each page. For Word docs, it uses python-docx, and for text files, it just reads them directly. The text gets split into chunks of about 800 tokens each, making sure it doesn't cut off in the middle of a sentence. This chunking is done using tiktoken to count tokens accurately.

For the AI part, I'm using OpenAI's text-embedding-3-small model to create vector representations of the text. These embeddings capture the meaning of the text in a way that the computer can work with. Pinecone stores these vectors and helps find the most relevant chunks when you ask a question.

When you ask a question, the system does a few things: it creates an embedding of your question, finds the most similar text chunks in Pinecone using cosine similarity, and then uses GPT-3.5-turbo to generate a clear answer based on those chunks. The temperature is set to 0.7 to keep the answers creative but still focused.

One neat thing is how it handles chunking - it splits the text at sentence boundaries and makes sure each chunk stays under the token limit. This helps keep the context together, which makes the answers more coherent. The system also uses hashing to create unique IDs for each chunk, which helps with efficient storage and retrieval.

For deployment, the code is set up to work with Google Colab, but it could be adapted to run anywhere with Python. The API keys are handled securely using Google's userdata system.

There are some cool possibilities for improvement, like adding more file type support or making the chunking strategy even smarter. But even as is, it's a solid implementation that shows how powerful RAG systems can be when you combine large language models with efficient vector search.