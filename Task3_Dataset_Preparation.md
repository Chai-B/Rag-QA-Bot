# Task 3: Dataset Preparation and Fine-Tuning Approaches

When building the RAG system, I focused on creating an efficient data processing pipeline that handles different document types while maintaining context. Here's a detailed look at how the data preparation works.

The system processes three main document formats: PDFs, Word documents, and plain text files. For PDFs, it uses PyPDF2 to extract text from each page. For Word docs, it leverages python-docx to read the content, and for text files, it performs a straightforward read operation. The text extraction is designed to preserve the document's structure and content accurately.

After extraction, the text undergoes a chunking process. Using tiktoken, the system splits the text into chunks of approximately 800 tokens each. The chunking is done at sentence boundaries to maintain context - it won't split in the middle of a sentence. This approach ensures that when the system retrieves information, it gets complete thoughts rather than fragmented ideas.

The embedding process uses OpenAI's text-embedding-3-small model to convert text into 1536-dimensional vectors. These vectors are then stored in Pinecone, a vector database that enables fast similarity searches. Each chunk gets a unique MD5 hash ID based on the document name and chunk index, ensuring efficient storage and retrieval.

When processing documents, the system handles them in batches of 100 chunks at a time for efficient upsert operations into Pinecone. This batching helps manage memory usage and API rate limits. The vector database is configured to use cosine similarity for finding the most relevant chunks during query time.

For question-answering, the system takes a query, generates its embedding, and retrieves the top 3 most similar chunks from Pinecone. These chunks are then passed to GPT-3.5-turbo along with the original question to generate a coherent answer. The temperature is set to 0.7 to balance creativity with factual accuracy.

One key aspect of the implementation is error handling. The code includes proper exception handling for file operations and API calls. It also implements a simple retry mechanism for API calls that might fail due to rate limiting or network issues.

The system is designed to be modular, making it easy to swap out components. For example, you could replace Pinecone with another vector database or use a different embedding model if needed. The code structure is clean and well-documented, making it maintainable for future improvements.


